# Analisis de sentimiento sobre tweets en español
---
La tarea propuesta en este trabajo consiste en clasificar el texto de tweets en español en sentimientos positivos, neutrales o negativos.
Para esto, se utilizaron datasets provistos por la Sociedad Española del Procesado del Lenguaje Natural (SEPLN) para el Taller de Análisis de Sentimiento en español (TASS) en distintos años.

Se puede visualizar la notebook con el código en [Google Colab](https://colab.research.google.com/drive/1ZakIBJE4-2_WsFBQPoU-H5MUpB085Hjz) además de en este repositorio.

La tarea consta de las siguientes etapas:

1. *Parsing* de los archivos de los distintos datasets.
2. Preprocesamiento del texto.
3. Definición y entrenamiento del modelo.
4. Uso del modelo para determinar la polaridad de los tweets.

## Parsing de los archivos
Los datasets están en formato XML, por lo que se debe realizar un parsing para obtener los dataframes que podrán ser utilizados luego por el modelo.
Cada tweet dentro del archivo XML tiene la siguiente forma:

```xml
<tweet>
	<tweetid>769930322721013760</tweetid>
	<user>54924014</user>
	<content> texto del tweet </content>
	<date> Fecha </date>
	<lang>es</lang>
	<sentiment>
		<polarity><value> Valor de polaridad </value></polarity>
	</sentiment>
</tweet>
```

Al final de este proceso, se obtienen archivos .CSV

## Preprocesamiento del texto
En este paso, se tomó el texto de los tweets y se eliminaron:

- Tweets cuya polaridad sea NONE.
- Links.
- Nombres de usuario en menciones.
- Vocales con tilde (que fueron reemplazadas por su equivalente sin tilde)
- Hashtags.
- Risas.

Esto resulta en que solo queden las palabras del contenido del tweet.

## Definición del modelo
Se utilizó una representación doe n-gramas de palabras y el modelo [LinearSVC](https://scikit-learn.org/stable/modules/svm.html#svm-classification) provisto en la biblioteca [scikit learn](https://scikit-learn.org/stable/).
Como paso previo al entrenamiento del modelo, se generó una nueva columna en el dataframe, donde a cada polaridad se le asignó un número (0 para la negativa, 1 para la positiva y 2 para la neutral); esto será lo que predecirá el modelo. Entonces, se tiene un problema de clasificación de 3 clases.

## Uso del modelo para determinar la polaridad

Finalmente, el modelo obtiene una accuracy de aproximadamente 71% en el conjunto de entrenamiento de aproximadamente 11 mil tweets.

