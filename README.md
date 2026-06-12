# PlagIArismDetector

## Descripción

**PlagIArismDetector** es un proyecto enfocado en la detección de similitud entre códigos fuente mediante técnicas de aprendizaje automático. El sistema compara pares de programas y determina si existe una relación significativa de similitud entre ellos.

Para esto, se utiliza **CodeBERT** como extractor de embeddings semánticos del código y un modelo **MLP (Multilayer Perceptron)** como clasificador binario.

---

## Objetivo

Diseñar y desarrollar una herramienta computacional capaz de detectar posibles similitudes o infracciones a derechos de autor en código fuente, utilizando técnicas de machine learning, análisis de código y métodos cuantitativos.

---

## Dataset

El proyecto utiliza el dataset **AI-SOCO: Authorship Identification of SOurce COde**, el cual contiene código fuente en **C++** escrito por diferentes usuarios.

El dataset cuenta aproximadamente con:

* 1,000 usuarios
* 100 códigos por usuario
* 100,000 archivos de código fuente

Aunque AI-SOCO fue creado para identificación de autoría, en este proyecto se adaptó para una tarea de comparación entre pares de código.

---

## Funcionamiento general

El flujo principal del modelo es:

1. Carga y limpieza del código fuente.
2. Tokenización de los programas.
3. Generación de embeddings con **CodeBERT**.
4. Construcción de pares positivos y negativos.
5. Generación de características de similitud.
6. Clasificación binaria con un **MLP**.
7. Evaluación del desempeño del modelo.

---

## Construcción de pares

Los pares se construyeron usando las columnas principales del dataset:

* `pid`: identificador del archivo de código.
* `uid`: identificador del usuario o autor.

A partir de esto:

* Los pares positivos son códigos del mismo `uid`.
* Los pares negativos son códigos de diferentes `uid`.

Además, se incorporó una estrategia de **hard negatives**, donde se seleccionan códigos de usuarios distintos pero con alta similitud en el espacio de embeddings. Esto ayuda a que el modelo aprenda con ejemplos más difíciles.

---

## Modelo

La versión final del modelo combina:

* **CodeBERT**, para obtener representaciones semánticas del código.
* **Features de comparación**, como similitud coseno, distancia L1, distancia L2, diferencia absoluta y producto elemento a elemento.
* **MLP**, para clasificar si dos códigos son similares o no.

El MLP utiliza capas densas, activación ReLU, Batch Normalization, Dropout y optimización con AdamW.

---

## Resultados

El modelo final obtuvo los siguientes resultados en test:

| Métrica          | Resultado |
| ---------------- | --------: |
| Accuracy         |    0.9617 |
| Precision        |    0.9739 |
| Recall           |    0.9488 |
| F1-score         |    0.9612 |
| ROC-AUC          |    0.9907 |
| PR-AUC           |    0.9923 |
| Threshold óptimo |    0.5469 |

Estos resultados muestran que el modelo logró un desempeño alto y balanceado para distinguir entre pares de códigos similares y no similares.

---

## Tecnologías utilizadas

* Python
* Google Colab
* Google Drive
* PyTorch
* Transformers
* CodeBERT
* Scikit-learn
* Pandas
* NumPy
* Matplotlib

---