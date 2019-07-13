---
title: "PyData London 2019 Resumen"
date: 2019-07-13
tags: [post]
excerpt: "Recap de la experiencia en PyData London"
toc: true
toc_sticky: true
toc_label: "PyData London 2019, Dia 1"
toc_icon: "user-graduate"  # corresponding Font Awesome icon name (without fa prefix)
---

## Intro

PyData es una conferencia acerca de los usos de python en tareas aplicadas al analísis de datos y data science en general. Durante 3 días hay más de 40 charlas para asistir en una cantidad de tópicos muy variados. Es organizada por Numfocus, quienes son la entidad legal que representa a casi todas las herramientas de open source del ecosistema como jupyter, pandas, etc. 

Tuve la suerte de ser uno de las 500+ personas que participaron en esta edición y aquí haré un breve resumen de las charlas a las que asistí para ayudarme a entenderlas mejor y no olvidarme :)

Pueden ver la totalidad de las charlas y algunos repositorios [en este link](https://pydata.org/london2019/schedule/)

## Dia 1: Tutoriales

El día 1 sólo tiene tutoriales, de aprox 90 minutos, donde un expositor te guía y enseña sobre algun tema. Por lo general siempre van acompañados de un jupyter notebook para ir siguendo paso a paso.

En total fueron 12 charlas, de las cuales son 3 en paralelo, por lo que tuve que elegir 4:

* Build and Deploy an End-to-end Streaming NLP Insights System: 

En esta charla tenía mucho potencial y terminó siendo un poco "quien mucho abarca, poco aprieta", pero aprendí bastante de los pasos para conectarse a una api como la de reddit y terminar produciendo como resultado un bot de telegram. Me sorprendió lo fácil que es hacer un bot de telegram usando [Botfather](https://core.telegram.org/bots).

Link [github](https://github.com/MichaMucha/pydata2019-nlp-system)

* Advanced Software Testing for Data Scientists: 

En esta charla revisamos las principales formas de hacer testing cuando se desarrolla software, y las librerías que más aplican para los procesos en análisis de datos. Lo más rescatable fue conocer Pytruth, una librería que simplifica la vida a la hora de escribir tests. 

El repo lo encuentran [aquí](https://github.com/cambridgespark/pydata-testing-for-data-science)

* An Introduction to Markov chain Monte Carlo using PyMC3:

Ser Bayesiano es te hace *cool* así que obviamente iba con todas las ganas a esta charla, que no decepcionó. El speaker era seco y explicó muy bien todos los conceptos desde lo básico y por primera vez usé pymc3. Recomiendo 100% descargar los notebooks del repo y seguir el paso a paso.

Link [github](https://github.com/fonnesbeck/mcmc_pydata_london_2019)


* A/B-testing by clusters:

A/B testing es fundamental para hacer mejoras en cualquier negocio digital, pero es fácil hacerlo mal si no se tienen los conocimientos necesarios para reconocer resultados falsos (que te pueden llevar a conclusiones apuradas y erradas).

Los speakers de esta charla lograron mostrar con ejemplos clarísimos cómo evitar estos errores y como evaluar tus datos para encontrar estas posibles *pillerías*. De lo más rescatable me llevo el concepto de AA testing, básicamente un test A/B de mentira, que de encontrar diferencias significativas, te permite establecer un % de falsos positivos. También el análisis de sensibilidad que demostraron fue muy interesante, dónde basicamente fabricaban algunos datos para ver que tan robustos eran los tests.

Link al repo : [github](https://github.com/bertilhatt/pydata_pres_small_sample)

## Dia 2: Charlas

Los días 2 y 3 son dedicados a charlas de dos tipos: keynotes grandes (para todo el mundo) y charlas mas específicas (3 simultáneas).

Este día comenzó con una keynote llamada "The Turing Way: A how to guide for reproducible research", mostrando el trabajo que están haciendo en el instituto Alan Turing para apoyar la investigacion reproducible. Están en el proceso de armar un libro que pueden ver [aqui](https://the-turing-way.netlify.com/introduction/introduction). Una de las cosas mas interesantes fue la demostración de [Binder](https://gke.mybinder.org/) que permite hacer esto mucho más facil.

Luego decidí ir a las siguentes charlas:

* A practical guide towards algorithmic bias and explainability in machine learning:

Esta charla se centró en definir conceptos de sesgo y problemas para explicar los modelos black box que han traído los avances en deep learning. Introdujeron un paquete muy interesante llamado [Alibi](https://github.com/SeldonIO/alibi) que permite visualizar por qué el modelo predice una u otra clase.

Un ejemplo de clasificación de imagenes:


Prediccion Gato           |  Explicacion gato
:-------------------------:|:-------------------------:
![cat original](https://github.com/SeldonIO/alibi/raw/master/doc/source/methods/persiancat.png) |  ![explicacion gato](https://docs.seldon.io/projects/alibi/en/v0.2.2/_images/examples_anchor_image_imagenet_18_1.png)


* Embeddings! Embeddings everywhere! - How to build a recommender system using representation learning:

Esta charla demostró el trabajo que han hecho en Allegro, el marketplace de ecommerce mas grande en Polonia, para crear sistemas recomendadores usando los ultimos avances en Deep Learning. Por una parte hablaron del trabajo que han hecho con Collaborative Filtering, usando una adaptacion de word2vec llamado prod2vec para crear embeddings de productos y asi buscar productos similares para recomendar mediante kmeans. Por otra lado también hablaron de como solucionaron el problema de cold start para productos nuevos o sin tanta información usando aproximaciones basadas en contenido.


* On the Path to Causal Inference:

Con mi formación en economía siempre me ha llamado la atención la poca importancia que se le da en el modelamiento a la causalidad y las relaciones entre variables, en general me he dado cuenta que en el "Data Science" la especificación correcta del modelo no es importante, lo primero es el poder predictivo.

Esta charla fue una breve introducción a *causal inference* y en general al modelo desarrollado por Judea Pearl (recomiendo leer su libro, The Book of Why) para identificar como las variables del modelo interactúan entre sí, qué variables actuan como *cofounders* y finalmente como digramar esto para lograr encontrar el efecto directo de X sobre Y con la menor posibilidad de sesgo.


* Prophet at Scale: Using Prophet at scale to tune and forecast time series at Spotify:

A esta charla le tenía fe, pensando que sería más aplicada a como Spotify forecasteaba y usaba esos forecast para tomar decisiones, lamentablemente fue una charla sólo orientada al lado de Data Engineering, ya que al ser una plataforma masiva, realizan mas de 10.000 forecast diarios, mostraron básicamente el stack que usan de Spark + Python + GCP y algunas formas que usan para optimizar hyperparámetros con tanto forecast.	

* How to Validate Your Client Churn Model: 

La última charla del día, bastante densa matemáticamente y tocando una tema super interesante *Survival Analysis*

Elena mostró como hacer análisis correctos, comenzando con un modelo básico como Kaplan Meier y luego uno parametrico (Cox Proportional Hazard), además mostró varias tecnicas para validar los modelos que nunca había escuchado y que me parecieron muy interesantes. Estoy estudiando este tema para hacer un post mas detallado proximamente.

[Aquí](https://github.com/elena-sharova/SurvivalAnalysis) les dejo el link del repo y veré si puedo conseguir la presentación 


## Dia 3: Charlas
