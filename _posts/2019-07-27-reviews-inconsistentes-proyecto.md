---
title: "Corrigiendo reviews inconsistentes en plataformas web"
date: 2019-07-27
tags: [post, project]
excerpt: "Una aplicación simple de procesamiento de texto para mejorar la calidad de reviews en plataformas web"
toc: true
toc_sticky: true
toc_label: "Indice"
toc_icon: "user-circle"  # corresponding Font Awesome icon name (without fa prefix)
---

## Intro

> "La ensalada estaba muy rica!!, ⭐️" 

¿Has visto algo así en los reviews de algún restaurant? Yo sí y varias veces.

![ex 1](https://i.imgur.com/mVnZ01P.png) 
![ex 2](https://i.imgur.com/gMtFQSU.png)


Siempre me llamó la atención que siendo un problema a mi parecer simple de resolver, ninguna plataforma parece hacer algo con estos reviews. Es por eso que quise crear una solución simple pero que pudiera ser aplicable para este tipo de casos.

La solución fue usar un modelo básico de análisis de sentimiento y disponibilizar este modelo como una API que se pueda consultar a la hora de escribir un review, para sugerir al usuario revisar la calificación o el texto.


## Datos

Para poder entrenar un modelo necesitamos datos, en este caso usé una parte del dataset de [Yelp](https://www.yelp.com/dataset), el cual tiene más de 4 millones de reviews. Luego de procesar los datos, eliminar los que no correspondían a restaurantes, obtuve la siguiente distribución.

| Estrellas | Cantidad | Porcentaje |
|-------|--------|---------|
| 1     | 496.113   | 12%   |
| 2     | 391.043  | 9%   |
| 3     | 558.747 | 13%  |
| 4     | 1.094.133  | 26%  |
| 5     |  1.642.828  | 39%   |

Como pueden ver, es un dataset bastante desbalanceado, con 65% de reviews teniendo 4 estrellas o más. Otro punto importante a la hora de poner esto en producción es tener en cuenta que este dataset probablemente no refleja la verdadera distibución de los datos. De hecho las [cifras oficiales](https://www.yelp.com/factsheet) de Yelp muestran que el 49% de los reviews son 5-estrella y 20% son 1-estrella. 

## El problema

Para pensar en formular una solución debemos pensar cómo es que se produce el problema y cómo detectarlo. 

En el caso de estos reviews, la causa más probable de la inconsistencia es que sea un error de input del usuario al momento de seleccionar las estrellas. El caso contrario no hace mucho sentido.

Para detectar el problema debemos encontrar una forma de contrastar la calificación en estrellas vs el sentimiento del texto escrito para alertar al usuario en caso de que no sea consistente. En consecuencia el problema se transforma en evaluar si **el texto es de connotación positiva o negativa**, para luego compararlo con la cantidad de estrellas.

## Modelo

Transformaremos nuestro dataset original para que pueda funcionar con la definición de problema que hicimos. Para esto eliminaremos los reviews con 3 estrellas por asumir que son *neutros* y que sólo nos interesa identificar reviews positivos o negativos. 

Al eliminar estos reviews, agrupamos los de 1 y 2 estrellas en la categoría *negativo* y los de 4 y 5 estrellas en la categoría *positivo*. Además como tenemos muchísimos más reviews positivos que negativos, tomaremos sólo una muestra de los reviews positivos para entrenar este modelo. Si bien perderemos algo de información, es posible que muchos de estos reviews positivos sean similares por lo que el efecto no será muy grande y un dataset muy desbalanceado podría perjudicar las métricas de evaluación de la clase más pequeña por overfitting.


|   Clase  | Cantidad | Porcentaje |
|:--------:|:--------:|:----------:|
| Negativo |   887.157 |     50%    |
| Positivo |   887.157 |     50%    |

Con nuestro dataset listo, debemos procesarlo para crear nuestras features, en este caso usaremos Bag of Words con TF-IDF, que es fácilmente utilizable con **TfidfVectorizer** de sklearn. En este caso probe remover stop words y no tuvo efecto alguno en la precisión. También usé una técnica de ***negation handling*** que marca con un sufijo _NOT a las palabras que vienen despues de una negación como "NOT" o palabras con "n't"

Una vez transformado el dataset en nuestra sparse matrix, procedemos a crear un train-test split de 80/20 y entrenar diferentes clasificadores.

En este caso el mejor clasificador fue la regresión logística, seguido muy de cerca por Linear SVM. Decidí usar la regresión logística porque es posible obtener probabilidades fácilmente, comparado con SVM.

![models](https://i.imgur.com/QYukkdL.png)

## Explicación

Una parte importante en este proyecto era encontrar una forma de explicarle al usuario de manera simple, por qué su review no tenia sentido. Para esto usé *LIME* que es un algoritmo que permite entregar "explicaciones" para cualquier tipo de modelo tratándolo como una caja negra. Es muy útil y simple, pueden revisar más en el [github](https://github.com/marcotcr/lime) de la librería.

Por ejemplo para explicar por qué un review era positivo, LIME entrega las siguientes palabras como las que influyen en que el review sea clasificado así.

![exp](https://i.imgur.com/k1yf1m9.png)



## Deployment

Con el modelo ya entrenado y guardado en formato *pickle* podemos armar una API básica usando Flask, que use los modelos entrenados anteriormente y al recibir un POST request ejecute el siguiente código

```python
#Load model
clf = joblib.load('model.pkl')
bow = joblib.load('bow.pkl')

# URL that we'll use to make predictions using get and post
@app.route('/predict',methods=['GET, POST'])

def predict():
    review = request.args.get('text') # get text review 
    c = make_pipeline(bow, clf) # pipeline for processing the text
    explainer = LimeTextExplainer(class_names=['negative', 'positive']) # creating the explanation instance
    exp = explainer.explain_instance(review, c.predict_proba, num_features=8) # explaining the model result
    word_list = exp.as_list() # converting explained words to list
    probs = exp.predict_proba # getting predicted class probabilities
    if probs[0] > probs[1]:
        return jsonify({'review_pol': 'Negative'}, {'words': get_negatives(word_list)}, {'proba': round(probs[0],2)*100}) # return JSON of negative prediction and explanation
    else:
        return jsonify({'review_pol': 'Positive'}, {'words': get_positives(word_list)}, {'proba': round(probs[1],2)*100}) # return JSON of positive prediction and explanation

```

Ahora por ejemplo si hacemos el siguiente request (http://<span></span>/url.com/predict?text='The food was great') ,  la API responde el siguiente JSON.

```json
[{"review_pol":"Positive"},{"words":[["great",0.6325334294750388],["The",0.001599870297545557]]},{"proba":100}]
```

Este jSON contiene la predicción de la polaridad del texto y además qué palabras son las más importantes en para clasificarla como tal. En este caso es la palabra **great**. La palabra **The** no la consideramos realmente porque su peso relativo a great es muy bajo (0.68 vs 0.001).

Con Flask también es posible hacer una web-app con la misma funcionalidad. En este [link](http://carancib.pythonanywhere.com/send) pueden ver un demo hecho con Bootstrap en frontend y Flask en backend.

## Conclusión

La idea de este proyecto fue encontrar una manera de corregir a un usuario antes de que cometa un error al hacer un review, además de aprender una forma simple y efectiva de implementar un modelo de ML disponible en la web. 

Quedé muy contento con cómo quedó la mini app web, y quién sabe, quizás algún día una plataforma de reviews implemente algo parecido.

![webexp](https://i.imgur.com/nafauTQ.png)
