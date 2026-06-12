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

### Justificación de Dataset
Nuestra construcción del modelo fue sustentada por diferentes artículos de los cuáles sacamos nuestras métricas, pero antes de construir nuestro modelo primero tuvimos que conseguir un dataset que tuviera la información necesaria, al final decidimos utilizar AI-SOCO, desarrollado por la organización PAN, algunas de las razones por la que escogimos este dataset fueron:

- Identificar al autor de un fragmento de código fuente a partir de su estilo de programación (Authorship Attribution).
- Origen de los datos: Los códigos fueron recopilados de envíos aceptados en la plataforma de programación competitiva Codeforces, garantizando que fueran programas correctos y compilables.
- Tamaño del dataset: Contiene 100,000 programas en C++ escritos por 1,000 autores, con 100 soluciones por cada autor
- Estructura: El conjunto se divide en entrenamiento (50,000), desarrollo (25,000) y prueba (25,000) ejemplos.
---
# Justificación Tecnologías
## CodeBERT CPP
Durante nuestra investigación, un factor en común en 3/6 de nuestros artículos fue el uso de CodeBERT,  un "traductor inteligente" que puede leer código de programación y comprender qué hace, relacionándolo con descripciones escritas en lenguaje humano, en los artículos que investigamos CodeBERT fue utilizado como una herramienta de apoyo para la extracción de elementos semánticos, pero cabe aclarar utilizamos una version llamada codebert-cpp, ya que el codebert original no esta entrenado C++.

## Multi Layer Perceptron
En base a Ramachandra. et.al. (2026) [1] el modelo con mejor desempeño es la combinación de modelos MLP, XGboost, Random Forest y SVM. Pero entre estos modelos, el de mejor desempeño entre estos el MLP o  Multi Layer Perceptron.

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
## Justificación de Métricas Utilizadas
En base a papeles escritos por la Karlsruhe Institute of Technology (KIT), la University of California Riverside y la Università degli studi dell’Aquila, determinamos estas 4 métricas para poder medir el rendimiento de nuestro modelo:
- Accuracy: Porcentaje total de predicciones correctas.
- Recall: Capacidad para encontrar los casos positivos reales.
- Precisión: Proporción de positivos predichos que son correctos.
- F1 Score: Balance entre precisión y recall.


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

# Referencias
[1] A. Ramachandra, S. Chaudhary, J. Tran, R. Desai, A. Pang, and M. Salloum, “Detecting AI-Generated Code in Introductory Programming Courses,” Proceedings of the 57th ACM Technical Symposium on Computer Science Education V.1, pp. 894–900, Feb. 2026, doi: 10.1145/3770762.3772522. Available: https://dl.acm.org/doi/10.1145/3770762.3772522.

[2] D. Álvarez-Fidalgo and F. Ortin, “CLAVE: A deep learning model for source code authorship verification with contrastive learning and transformer encoders,” Information Processing & Management, vol. 62, no. 3, p. 104005, May 2025, doi: 10.1016/j.ipm.2024.104005. Available: https://www.sciencedirect.com/science/article/pii/S0306457324003649?via%3Dihub.

[3] G. Boukili, S. EL Garouani, and J. Riffi, “A dataset for human-written and AI-generated code source classification,” Data in Brief, vol. 65, p. 112527, Apr. 2026, doi: 10.1016/j.dib.2026.112527. Available: https://www.sciencedirect.com/science/article/pii/S2352340926000806?via%3Dihub. 

[4] M. Hoq et al., “Detecting ChatGPT-Generated Code Submissions in a CS1 Course Using Machine Learning Models,” Proceedings of the 55th ACM Technical Symposium on Computer Science Education V. 1, pp. 526–532, Mar. 2024, doi: 10.1145/3626252.3630826. Available: https://dl.acm.org/doi/10.1145/3626252.3630826. 

[5] P. T. Nguyen, J. Di Rocco, C. Di Sipio, R. Rubei, D. Di Ruscio, and M. Di Penta, “GPTSniffer: A CodeBERT-based classifier to detect source code written by ChatGPT,” Journal of Systems and Software, vol. 214, p. 112059, Aug. 2024, doi: 10.1016/j.jss.2024.112059. Available: https://www.sciencedirect.com/science/article/pii/S0164121224001043?via%3Dihub.

[6] T. Sağlam, S. Hahner, L. Schmid, and E. Burger, “Automated Detection of AI-Obfuscated Plagiarism in Modeling Assignments,” Proceedings of the 46th International Conference on Software Engineering: Software Engineering Education and Training, pp. 297–308, Apr. 2024, doi: 10.1145/3639474.3640084. Available: https://dl.acm.org/doi/10.1145/3639474.3640084.

[7] AliOsm. (s. f.). GitHub - AliOsm/AI-SOCO: Official FIRE 2020 Authorship Identification of SOurce COde (AI-SOCO) task repository containing dataset, evaluation tools and baselines. GitHub. https://github.com/AliOsm/AI-SOCO/tree/master

[8] neulab/codebert-cpp · Hugging Face. (2023, 10 febrero). https://huggingface.co/neulab/codebert-cpp/
