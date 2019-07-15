---
title: "PyData London 2019 Resumen"
date: 2019-07-13
tags: [post]
excerpt: "Recap de la experiencia en PyData London"
toc: true
toc_sticky: true
toc_label: "PyData London 2019, Dia 1"
toc_icon: "user-circle"  # corresponding Font Awesome icon name (without fa prefix)
og_image: https://pydata.org/london2019/static/images/logo.288981a8dfa8.png
---

![logo](https://pydata.org/london2019/static/images/logo.288981a8dfa8.png){: .align-center}

## Intro

PyData es una conferencia acerca de los usos de python en tareas aplicadas al análIsis de datos y data science en general. Durante 3 días hay más de 40 charlas para asistir en una cantidad de tópicos muy variados. Es organizada por Numfocus, quienes son la entidad legal que representa a casi todas las herramientas de open source del ecosistema como Jupyter, pandas, etc. 

Tuve la suerte de ser uno de las 500+ personas que participaron en esta edición y aquí haré un breve resumen de las charlas a las que asistí para ayudarme a entenderlas mejor y no olvidarme :)

Pueden ver la totalidad de las charlas y algunos repositorios [en este link](https://pydata.org/london2019/schedule/)

## Dia 1: Tutoriales

El día 1 sólo tiene tutoriales, de aprox 90 minutos, donde un expositor te guía y enseña sobre algun tema. Por lo general siempre van acompañados de un jupyter notebook para ir siguendo paso a paso.

En total fueron 12 charlas, de las cuales son 3 en paralelo, por lo que tuve que elegir 4:

* *Build and Deploy an End-to-end Streaming NLP Insights System:* 

En esta charla tenía mucho potencial y terminó siendo un poco "quien mucho abarca, poco aprieta", pero aprendí bastante de los pasos para conectarse a una API como la de reddit y terminar produciendo como resultado un bot de telegram. Me sorprendió lo fácil que es hacer un bot de telegram usando [Botfather](https://core.telegram.org/bots).

Link [github](https://github.com/MichaMucha/pydata2019-nlp-system)

* *Advanced Software Testing for Data Scientists:* 

En esta charla revisamos las principales formas de hacer testing cuando se desarrolla software, y las librerías que más aplican para los procesos en análisis de datos. Lo más rescatable fue conocer Pytruth, una librería que simplifica la vida a la hora de escribir tests. 

El repo lo encuentran [aquí](https://github.com/cambridgespark/pydata-testing-for-data-science)

* *An Introduction to Markov chain Monte Carlo using PyMC3:*

Ser Bayesiano te hace **cool** así que obviamente iba con todas las ganas a esta charla, que no decepcionó. El speaker era seco y explicó muy bien todos los conceptos desde lo básico y por primera vez usé pymc3. Recomiendo 100% descargar los notebooks del repo y seguir el paso a paso.

Link [github](https://github.com/fonnesbeck/mcmc_pydata_london_2019)


* *A/B-testing by clusters:*

A/B testing es fundamental para hacer mejoras en cualquier negocio digital, pero es fácil hacerlo mal si no se tienen los conocimientos necesarios para reconocer resultados falsos (que te pueden llevar a conclusiones apuradas y erradas).

Los speakers de esta charla lograron mostrar con ejemplos clarísimos cómo evitar estos errores y como evaluar tus datos para encontrar estas posibles *pillerías*. De lo más rescatable me llevo el concepto de AA testing, básicamente un test A/B de mentira, que de encontrar diferencias significativas, te permite establecer un % de falsos positivos. También el análisis de sensibilidad que demostraron fue muy interesante, dónde básicamente fabricaban algunos datos para ver que tan robustos eran los tests.

Link al repo : [github](https://github.com/bertilhatt/pydata_pres_small_sample)

## Dia 2: Charlas

Los días 2 y 3 son dedicados a charlas de dos tipos: keynotes grandes (para todo el mundo) y charlas mas específicas (3 simultáneas).

Este día comenzó con una keynote llamada "The Turing Way: A how to guide for reproducible research", mostrando el trabajo que están haciendo en el instituto Alan Turing para apoyar la investigacion reproducible. Están en el proceso de armar un libro que pueden ver [aqui](https://the-turing-way.netlify.com/introduction/introduction). Una de las cosas mas interesantes fue la demostración de [Binder](https://gke.mybinder.org/) que permite hacer esto mucho más fácil.

Luego decidí ir a las siguientes charlas:

* *A practical guide towards algorithmic bias and explainability in machine learning:*

Esta charla se centró en definir conceptos de sesgo y problemas para explicar los modelos black box que han traído los avances en deep learning. Introdujeron un paquete muy interesante llamado [Alibi](https://github.com/SeldonIO/alibi) que permite visualizar por qué el modelo predice una u otra clase.

Un ejemplo de clasificación de imágenes:


Predicción Gato           |  Explicación gato
:-------------------------:|:-------------------------:
![cat original](https://github.com/SeldonIO/alibi/raw/master/doc/source/methods/persiancat.png) |  ![explicación gato](https://docs.seldon.io/projects/alibi/en/v0.2.2/_images/examples_anchor_image_imagenet_18_1.png)


* *Embeddings! Embeddings everywhere! - How to build a recommender system using representation learning:*

Esta charla demostró el trabajo que han hecho en Allegro, el marketplace de ecommerce mas grande en Polonia, para crear sistemas recomendadores usando los últimos avances en Deep Learning. Por una parte hablaron del trabajo que han hecho con Collaborative Filtering, usando una adaptación de word2vec llamado prod2vec para crear embeddings de productos y asi buscar productos similares para recomendar mediante kmeans. Por otra lado también hablaron de como solucionaron el problema de cold start para productos nuevos o sin tanta información usando aproximaciones basadas en contenido.


* *On the Path to Causal Inference:*

Con mi formación en economía siempre me ha llamado la atención la poca importancia que se le da en el modelamiento a la causalidad y las relaciones entre variables, en general me he dado cuenta que en el "Data Science" la especificación correcta del modelo no es importante, lo primero es el poder predictivo.

Esta charla fue una breve introducción a *causal inference* y en general al modelo desarrollado por Judea Pearl (recomiendo leer su libro, The Book of Why) para identificar como las variables del modelo interactúan entre sí, qué variables actúan como **confounders** y finalmente como diagramar esto para lograr encontrar el efecto directo de X sobre Y con la menor posibilidad de sesgo.


* *Prophet at Scale: Using Prophet at scale to tune and forecast time series at Spotify:*

A esta charla le tenía fe, pensando que sería más aplicada a como Spotify forecasteaba y usaba esos forecast para tomar decisiones, lamentablemente fue una charla sólo orientada al lado de Data Engineering, ya que al ser una plataforma masiva, realizan mas de 10.000 forecast diarios, mostraron básicamente el stack que usan de Spark + Python + GCP y algunas formas que usan para optimizar hiperparámetros con tanto forecast.	

* *How to Validate Your Client Churn Model:* 

La última charla del día, bastante densa matemáticamente y tocando una tema super interesante *Survival Analysis*

Elena mostró como hacer análisis correctos, comenzando con un modelo básico como Kaplan Meier y luego uno paramétrico (Cox Proportional Hazard), además mostró varias técnicas para validar los modelos que nunca había escuchado y que me parecieron muy interesantes. Estoy estudiando este tema para hacer un post más detallado próximamente.

[Aquí](https://github.com/elena-sharova/SurvivalAnalysis) les dejo el link del repo y veré si puedo conseguir la presentación 


## Dia 3: Charlas

* *Combining Computer Vision and NLP for Multi-Task Fashion Attribute Modeling at Shoprunner*

El día comenzó con una presentación muy buena acerca de Multitask Learning, juntando la información de imágenes y texto de millones de productos para crear atributos en la tienda de ecommerce Shoprunner. Usando un ensemble de CNN y BERT adaptado al dominio de la ropa, lograron crear un modelo eficiente y eficaz a la hora de taggear nuevos productos.

Presentación en este [link](https://slack-files.com/T53T9UZA5-FL2JAESV9-0322ea0829)

* *The unreasonable effectiveness of feature hashing*

Esta charla se trató acerca de los beneficios de transformar variables categóricas en hashes, que permiten realizar estimaciones de manera mucho más rápida y tratar algunos problemas clásicos que se dan cuando se termina teniendo mas features que observaciones. Me pareció súper interesante sobrotodo ver como algo típico de criptografía se puede aplicar de otra forma al análisis de datos. 

Presentación en este [link](https://github.com/gcampanella/pydata-london-2019)

* *How to Constrain Artificial Stupidity.*

Una de las charlas que más me gustaron de la conferencia, Vincent [koaning.io](http://koaning.io/) habló de los problemas de predecir sin pensar de manera probablística y los problemas que eso trae. Básicamente fue un recordatorio de no concentrarse tanto en métricas como accuracy o ROC y pensar más en que tipo de features estamos usando y como eso impacta a las personas.


* *Just ask: designing intent-driven algos*

Stitchfix es una empresa que mensualmente te envía ropa según tus medidas y gustos, tú eliges las que te gustan y las que no, puedes devolverlas gratis. Esta charla fue particularmente interesante para mí porque mostraron como la cultura de los Datos está presente en toda su organización. Uno de los desarrollos mas interesantes que mostraron es un recomendador para los empleados que trabajan como *compradores* frente a las marcas. El sistema era capaz de hacer una optimización con la librería [PyOmo](http://www.pyomo.org/) según diferentes restricciones (margen, tallas, colores, temporada) y en base a eso retornar un set de productos que tenían alta probabilidad de ser escogidos por la gente cuando llegan a sus manos.


Presentación en este [link](https://www.slideshare.net/annarschneider/just-ask-designing-intentdriven-algos)


* *Deep Learning and Time Series Forecasting for Smarter Energy*

Esta fue la última charla, Igor mostró como modelaban miles de series de tiempo de manera agregada para predecir oferta y demanda en el mercado británico. Fue interesante ver que trabajan harto con Keras y que tiene una gran flexibilidad para montar varios modelos juntos.

Presentación en este [link](https://tech.octopus.energy/data-discourse/PyData2019/TimeSeries.html#/)

## Conclusión 

En resumen valió la pena 100% esta conferencia, ojalá que algún día en Chile podamos tener un evento así de masivo y con empresas nacionales mostrando como usan los datos para mejorar sus procesos y la experiencia de los usuarios.

