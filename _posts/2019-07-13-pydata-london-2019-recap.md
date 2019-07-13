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

* Advanced Software Testing for Data Scientists: 

En esta charla revisamos las principales formas de hacer testing cuando se desarrolla software, y las librerías que más aplican para los procesos en análisis de datos. Lo más rescatable fue conocer Pytruth, una librería que simplifica la vida a la hora de escribir tests. 

El repo lo encuentran [aquí](https://github.com/cambridgespark/pydata-testing-for-data-science)

* An Introduction to Markov chain Monte Carlo using PyMC3:

Ser Bayesiano es te hace *cool* así que obviamente iba con todas las ganas a esta charla, que no decepcionó. El speaker era seco y explicó muy bien todos los conceptos desde lo básico y por primera vez usé pymc3. Recomiendo 100% descargar los notebooks del repo y seguir el paso a paso.

Link [github](https://github.com/fonnesbeck/mcmc_pydata_london_2019)


* A/B-testing by clusters:

A/B testing es fundamental para hacer mejoras en cualquier negocio digital, pero es fácil hacerlo mal si no se tienen los conocimientos necesarios para reconocer resultados falsos (que te pueden llevar a conclusiones apuradas y erradas).

Los speakers de esta charla lograron mostrar con ejemplos clarísimos cómo evitar estos errores y como evaluar tus datos para encontrar estas posibles *pillerías*. De lo más rescatable me llevo el concepto de AA testing, básicamente un test A/B de mentira, que de encontrar diferencias significativas, te permite establecer un &Alpha; de falsos positivos. También el análisis de sensibilidad que demostraron fue muy interesante.

Link al repo : [github](https://github.com/bertilhatt/pydata_pres_small_sample)








