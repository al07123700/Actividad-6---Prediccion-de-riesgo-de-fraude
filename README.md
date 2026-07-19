# Actividad-6---Prediccion-de-riesgo-de-fraude
Reporte técnico de evaluación, validación e impacto en el negocio
# Predicción de Riesgo de Detección de Fraude en Tarjetas de Crédito
**Reporte Técnico de Evaluación, Validación e Impacto en el Negocio** *Desarrollado por: Yasmin Mtz Pinillo*

---

## 📋 1. Definición del Problema y Contexto

El problema de detección de fraude en transacciones con tarjeta de crédito surge de la necesidad crítica que tienen los bancos de identificar operaciones sospechosas en tiempo real. Cada día se procesan millones de transacciones y, dentro de ese volumen masivo, sólo una fracción diminuta corresponde a fraudes, sin embargo, su impacto económico y reputacional es muy grande. El dataset "Credit Card Fraud Detection" captura este escenario, con 10,000 transacciones.

En este contexto, el objetivo del modelo de clasificación es aprender patrones sutiles que distingan una transacción legítima de una fraudulenta, aun cuando ambas puedan parecer similares a simple vista. 

## 🎯 2. Objetivo SMART del Proyecto

* **Specific (Específico):** Desarrollar un modelo predictivo de clasificación capaz de detectar transacciones fraudulentas, utilizando variables predictoras como: horario de la transacción, si la operación es nacional o extranjera, entre otras).
* **Measurable (Medible):** Lograr que el modelo final obtenga un **AUC-ROC ≥ 0.95** y un **Recall (Sensibilidad) ≥ 85%** en la detección de casos positivos de fraude.
* **Achievable (Alcanzable):** El objetivo se alcanzará mediante el modelo de regresión logística y random forest.
* **Relevant (Relevante):** Minimizar los falsos negativos, críticos en fraude.
* **Time-bound (Temporal):** El ciclo completo de experimentación, optimización y diseño de la simulación de pruebas se completará en un plazo máximo de **4 semanas**.

---

## 📊 3. Tablero Visual de Resultados y Comparación con Baseline

Para justificar la complejidad técnica del modelo avanzado (**Random Forest**), se estableció una **Regresión Logística** como modelo *Baseline*. A continuación, se presenta la evaluación comparativa en el set de prueba.

En el archivo "Actividad 6 Yasmin Mtz P.ipynb" se muestran las gráficas de confusión de cada uno de los modelos, en donde se aprecia el alto número de verdaderos positivos en ambos modelos (Random forest ligeramente por arriba), y con 8 falsos negativos en Random Forest, lo que hace pensar que tiene un mejor desempeño, además de que el nivel de precisión y recall de dicho modelo también es superior tanto en la identificación de fraude como en los que no lo son. Toda la información aquí comentada se puede observar en el archivo con extensión "ipynb".

### 📉 Comparación de Modelos (Curva ROC)
La curva ROC compara el desempeño de los dos modelos —Regresión Logística y Random Forest— en la tarea de distinguir entre transacciones fraudulentas y legítimas. En este gráfico, cuanto más se acerque la curva a la esquina superior izquierda, mejor es la capacidad del modelo para maximizar verdaderos positivos mientras minimiza falsos positivos. Aquí se observa que ambos modelos tienen un rendimiento sobresaliente, pero el Random Forest destaca con una curva prácticamente pegada al borde superior y un AUC = 1.00, lo que indica una separación perfecta entre clases en los datos utilizados. La Regresión Logística también muestra un desempeño excelente con AUC = 0.99, aunque ligeramente inferior.

La línea diagonal representa un clasificador aleatorio, útil como referencia mínima. El hecho de que ambas curvas estén muy por encima de esta línea confirma que los modelos capturan patrones relevantes en los datos. Sin embargo, un AUC perfecto puede ser señal de sobreajuste, especialmente en problemas con desbalance extremo como el fraude financiero. Por eso, aunque la interpretación directa es que el Random Forest es superior, es importante validar si ese rendimiento se mantiene en datos nuevos y no vistos.

Ver archivo "Actividad 6 Yasmin Mtz P.ipynb"

### 🎯 Ajuste de Umbral Operativo e Interpretación de la Matriz de Confusión
Operando bajo el umbral estándar de 0.5, el modelo predecía correctamente 1970 operaciones y no generaba falsos positivos, lo cual es excelente pues evita alarmas innecesarias para los clientes, de igual forma el modelo identifica 22 fraudes de forma correcta, pero deja escapar 8 fraudes que, aunque parece un número pequeño, podría representar pérdidas significativas. omitía un volumen peligroso de pacientes enfermas. Al desplazar el umbral a 0.6, el número de fraudes que se escaparon incrementa a 14, por lo que no resulta conveniente hacer un cambio en el umbral a 0.6. Por otro lado, al modificar el umbral a 0.4 el número de fraudes que se dejaron escapar disminuyó a 4 e identifica 26 fraudes de forma correcta, por lo que pareciera que el umbral más bajo mejora el desempeño del modelo, aun con un nivel de exactitud alto (0.998).

Ver archivo "Actividad 6 Yasmin Mtz P.ipynb"

### 🔍 Interpretación de los Cuadrantes:
* **Verdaderos Positivos (VP):** 1970 operaciones correctas.
* **Falsos Positivos (FP):** No se identificaron, lo que resulta satisfactorio pues evita alarmas innecesarias a los clientes.
* **Falsos Negativos (FN):** 4 operaciones con fraude no identificadas, lo que pone en riesgo financiero a la entidad y de reputación **El peor escenario financiero.** Reducido drásticamente gracias al ajuste de umbral (0.4).

---

## 🚀 4. Evidencia de Experimentos y Validación Cruzada

Para garantizar la estabilidad del modelo ante fluctuaciones en los datos de entrada y mitigar el riesgo de sobreajuste (*overfitting*), el modelo avanzado fue evaluado mediante **Validación Cruzada Estratificada ($5\text{-}Folds$)** sobre el conjunto de entrenamiento.

| Métrica Evaluada | Media Obtenida (CV) | Desviación Estándar ($\sigma$) | Estado vs. Objetivo SMART |
| :--- | :---: | :---: | :---: |
| **AUC-ROC** | **1** | ± 0.05 | **Superado** (Meta ≥ 0.95) |
| **Recall (Sensibilidad)** | **0.87** | ± 0.02 | **Superado** (Meta ≥ 0.85 con ajuste) |
| **Accuracy (Exactitud)** | 0.998 | 

---

## 🧪 5. Análisis de Pruebas A/B e Impacto en el Negocio

Para validar la efectividad de la solución antes de su despliegue en producción, se ejecutó una simulación estadística de una **Prueba A/B** con un flujo de 1,000 transacciones:
* **Grupo A (500 transacciones):** Evaluación asistida por el método tradicional / Regresión logística.
* **Grupo B (500 transacciones):** Evaluación asistida por el Modelo Optimizado de Random Forest.

Como resultado de lo anterior, se observa que en el grupo A existen 491 transacciones evaluadas correctamente, genera un falso positivo lo cual podría representar un problema para los clientes, acierta con 5 fraudes, pero deja escapar 3, lo cual podría llegar a ser un número importante. Por otro lado, en el grupo B se observan 492 transacciones evaluadas correctamente, cero falsos positivos, identifica 7 fraudes y deja escapar 1. Con lo anterior el modelo de random forest sigue siendo mejor y valido para el negocio.

Ver archivo "Actividad 6 Yasmin Mtz P.ipynb"

**Justificación de Impacto Financiero:** El nivel de exactitud del 0.998 y recall de 0.87 del modelo ajustado representa un incremento importante en el número de fraudes identificados, lo que ayuda a mitigar el riesgo económico y reputacional que conlleva un fraude para una institución bancaria y sus clientes. 

---

## 🏁 6. Conclusiones

1. **Cumplimiento de Objetivos:** El proyecto alcanzó con éxito las métricas estipuladas en el objetivo SMART, consolidando un $AUC\text{-}ROC$ de 0.95 y un $Recall$ superior al 85% mediante el ajuste dinámico del umbral de decisión.
2. **Justificación del Enfoque:** La priorización del *Recall* sobre el *Accuracy* o la *Precisión* demostró ser la estrategia matemáticamente correcta para resolver un problema del sector financiero, donde un error en la detección de fraude puede llevar a una pérdida económica y reputacional significativa para las instituciones bancarias y sus clientes.


## 🏁 FASE II - DESPLIEGUE

Este proyecto implementa un sistema completo de detección de fraude en transacciones financieras.
Incluye:

* API REST desarrollada con FastAPI para recibir datos de transacciones y generar predicciones.

* Frontend interactivo en Streamlit para que el usuario ingrese datos y visualice resultados.

* Modelo de Machine Learning entrenado para clasificar transacciones como fraudulentas o legítimas.

* Contenedor Docker que integra backend + frontend + modelo en una sola imagen ejecutable.

El objetivo es ofrecer una solución modular, reproducible y fácilmente desplegable en entornos locales o en la nube (este último no se incluye aquí).

## 🏗️ Arquitectura del sistema
La arquitectura se compone de tres módulos principales:

🔹 1. Backend (FastAPI)

* Expone el endpoint /predict
* Recibe un JSON con los datos de la transacción
* Procesa el modelo de ML
* Devuelve probabilidad de fraude y clasificación

🔹 2. Frontend (Streamlit)

* Interfaz web para ingresar datos
* Envía solicitudes al backend
* Muestra resultados de predicción

🔹 3. Modelo de Machine Learning

* Entrenado previamente con datos de transacciones
* Serializado en formato .pkl o .joblib
* Cargado por el backend al iniciar el contenedor

🔹 4. Docker

* Unifica backend + frontend + modelo
* Ejecuta ambos servicios mediante start.sh
* Expone:
     API en localhost:8000
     Frontend en localhost:8501

## 🧰 Tecnologías usadas

Componente/	              Tecnología
* Backend/	                FastAPI
* Frontend/	              Streamlit
* Modelo ML/	              Scikit-learn
* Serialización/	          Joblib / Pickle
* Contenedores/	          Docker
* Lenguaje/	              Python 3.11
* Servidor API/	          Uvicorn
* Sistema operativo base/	python:3.11-slim

## 🐳 Cómo ejecutar con Docker

* Construir la imagen:
  docker build -t fraud-risk-app -f docker/Dockerfile .

* Ejecutar el contenedor
  docker run -p 8000:8000 -p 8501:8501 fraud-risk-app

* Acceder a servicios
  API: http://localhost:8000/docs
  Frontend: http://localhost:8501

Se incluyen todos los archivos requeridos para la ejecución, dentro de este proyecto.

## Evidencias

* Creación de una imagen funcional en dockers, resaltó que se realizaron varios ejercicios para corregir diferentes errores en el proceso, pero aquí se incluye una de ellas:
  
  <img width="736" height="439" alt="Imagen funcional dockers" src="https://github.com/user-attachments/assets/5af31fb4-8911-47f4-a421-db598236fdb3" />

* Imagen de la creación de la API:

  <img width="736" height="460" alt="Creacion de la API" src="https://github.com/user-attachments/assets/87140869-48e9-45eb-b4df-5002baf85706" />

* Ingreso de JSON, después de haber corregido algunos errores en las variables:

  <img width="736" height="419" alt="Imagen ingreso de JSON" src="https://github.com/user-attachments/assets/213d4637-2ff9-4c6a-8ac4-9b8413ba2334" />

* Evidencia del resultado en API después de haber ingresado JSON:
  
  <img width="736" height="458" alt="Imagen resultado después de JSON" src="https://github.com/user-attachments/assets/1e012937-c34d-47c9-894e-54a6f9d0c772" />

* Evidencia de Streamlit - Pruebas funcionales. Se observa la evaluación de transacción identificada como de fraude (caso extremo):

  <img width="736" height="424" alt="Imagen de streamlit" src="https://github.com/user-attachments/assets/46b56049-3fa9-4cb2-9b18-ecdde6256abe" />

## 📘 Manual de despliegue en la nube

🧩 1. Descripción general del proceso de despliegue

1.- Construir la imagen Docker que contiene:

* Backend (FastAPI)
* Frontend (Streamlit)
* Modelo de ML
* Script de arranque (start.sh)

2.- Probar la imagen localmente para asegurar que:

* La API responde en localhost:8000
* El frontend funciona en localhost:8501

3.- Subir la imagen a un registro de contenedores, como:

* Docker Hub
* GitHub Container Registry
* Azure Container Registry

4.- Elegir una estrategia de despliegue, por ejemplo:

* Plataforma como Servicio (PaaS)
* Contenedores administrados
* Orquestadores (ECS, AKS, Kubernetes)

5.- Configurar el servicio en la nube para ejecutar el contenedor.

6.- Validar el despliegue mediante pruebas funcionales.

🛠️ 2. Requerimientos técnicos

Software: 
* Docker desktop
* Git
* Plataforma de despliegue, como por ejemplo render, railway o azure, entre otros

Archivos requeridos:
* docker/Dockerfile
* docker/start.sh
* backend/requirements.txt
* frontend/requirements.txt
* model/fraud_model.pkl
* Código fuente del backend y frontend

Puertos: 8000 y 8501

RAM: 1.2 GB, 1 vCPU, 1 GB de almacenamiento y tiempo de ejecución de Linux x86_64
