# Predicción de Churn en Telecomunicaciones

## Descripción
Proyecto de **Machine Learning** para predecir el abandono de clientes (**churn**) en una compañía de telecomunicaciones.  
Se implementan modelos de **Random Forest** y **XGBoost** con **ajuste de hiperparámetros**, además de técnicas de interpretabilidad para explicar las predicciones (LIME).

---

## Estructura del Proyecto
telco-churn-mlops/
├── data/
│ └── telco_churn.csv # Dataset original
├── notebooks/
│ ├── 1_eda_preprocessing.ipynb # Análisis exploratorio y preprocesamiento
│ ├── 2_model_training.ipynb # Entrenamiento y evaluación de modelos
│ └── 3_interpretability.ipynb # Interpretación de predicciones con LIME
├── app/
│ ├── api.py # API REST con FastAPI
│ ├── schemas.py # Modelos Pydantic
│ └── model.joblib # Modelo XGBoost entrenado
├── tests/
│ ├── test_api.py # Pruebas unitarias del endpoint
│ └── test_model.py # Validación de modelo
├── Dockerfile # Contenedor Docker
├── requirements.txt # Dependencias Python
├── .github/workflows/ci.yml # CI/CD GitHub Actions
└── README.md # Documentación

---

## Instalación y Uso Local

1. Crear un entorno virtual y activarlo (opcional pero recomendado):

python -m venv venv
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate     # Windows

2. Instalar dependencias:

pip install --upgrade pip
pip install -r requirements.txt

3. Ejecutar la API en modo desarrollo:

uvicorn app.api:app --reload --host 0.0.0.0 --port 8000


4. Probar endpoint /predict (ejemplo con curl):

curl -X POST "http://127.0.0.1:8000/predict" \
-H "Content-Type: application/json" \
-d '{
  "gender": "Female",
  "senior": "No",
  "partner": "Yes",
  "dependents": "No",
  "phone": "Yes",
  "phone_multiple": "No",
  "internet": "Yes",
  "internet_security": "No",
  "internet_backup": "No",
  "internet_protection": "No",
  "internet_support": "No",
  "streaming_tv": "No",
  "streaming_movies": "No",
  "contract": "Month-to-month",
  "paperless": "Yes",
  "payment": "Electronic check",
  "tenure": 5,
  "charges_monthly": 70.35
}'


5. Respuesta esperada:

{
  "churn_probability": 0.703,
  "churn_prediction": "Churn"

---

## Docker

1. Construir la imagen:

docker build -t telco-churn-api .


2. Ejecutar el contenedor:

docker run -p 8000:8000 telco-churn-api


La API estará disponible en http://localhost:8000.

Endpoints
Endpoint	Método	Descripción
/predict	POST	Recibe JSON con features del cliente y retorna probabilidad de churn y predicción final (Churn o No Churn).
Tests

## Ejecutar tests de la API:

pytest -v tests/test_api.py


## Validación del modelo:

pytest -v tests/test_model.py

## Interpretabilidad

Se incluye un notebook para interpretar predicciones usando LIME, lo que permite explicar qué features contribuyen más a la predicción de churn.

## Licencia

MIT License – libre uso y modificación.