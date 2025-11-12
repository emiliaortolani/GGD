# Predicción de Churn en E-Commerce  
**Universidad de Montevideo — Licenciatura en Ciencia de Datos para Negocios**  
**Materia:** Machine Learning Aplicado  
**Año:** 2025  
**Docente:** Francisco Esteves Muñoz  

---

## 1. Objetivo del proyecto

El propósito de este trabajo es desarrollar un modelo predictivo de *churn* (fuga de clientes) para una empresa de e-commerce.  
El objetivo es anticipar qué clientes dejarán de comprar dentro de un horizonte temporal determinado, con el fin de optimizar estrategias de retención (bonificaciones, cupones o beneficios personalizados).

El enfoque se centra en construir un flujo completo (*end-to-end*) de machine learning: desde la preparación del dataset hasta el registro de experimentos con MLflow, priorizando la reproducibilidad y el trabajo colaborativo.

---

## 2. Definición del target

Se definió **churn = cliente sin compras durante 90 días consecutivos** desde su última transacción.  
Además, se evaluaron horizontes alternativos (60 y 120 días) para comparar cobertura y precisión.

| Horizonte | Descripción | Justificación |
|------------|--------------|----------------|
| **H60** | Churn agresivo (mayor sensibilidad) | Permite campañas de retención tempranas |
| **H90** | Churn base | Equilibrio entre recall y estabilidad |
| **H120** | Churn conservador | Refleja clientes realmente inactivos |

---

## 3. Flujo de trabajo

1. **Exploración y limpieza de datos**  
   Integración de las tablas `clients`, `orders` y `products` provenientes de PostgreSQL.  
   Creación de variables a nivel cliente: frecuencia, gasto total y promedio, ratio de envío, diversidad de categorías, etc.

2. **Construcción de datasets analíticos (cinco variantes)**  
   - DS1_H90_base (baseline con ratios y shipping)  
   - DS2_H60 (horizonte 60 días)  
   - DS3_H120 (horizonte 120 días)  
   - DS4_no_ship (sin variables de envío)  
   - DS5_no_ratios (sin variables derivadas)

3. **Modelado supervisado**  
   Se entrenaron tres modelos:  
   - Regresión Logística (LogReg)  
   - Random Forest (RF)  
   - Gradient Boosting (GB)

4. **Evaluación de desempeño**  
   Métricas utilizadas: ROC-AUC, F1 Score, Accuracy y Average Precision (área bajo la curva Precision–Recall).

5. **Registro de experimentos con MLflow**  
   Todos los experimentos se registraron con métricas, parámetros y artefactos.  
   Los resultados se almacenan en la carpeta `notebooks/mlruns`.

---

## 4. Resultados principales

| Dataset | Modelo | ROC-AUC | F1_best | Horizonte |
|----------|---------|---------|---------|------------|
| DS1_H90_base | Gradient Boosting | 0.98 – 1.00 | 0.88 – 1.00 | 90 días |
| DS3_H120 | Gradient Boosting | 0.995 | 0.965 | 120 días |
| DS2_H60 | Random Forest | 0.975 | 0.91 | 60 días |
| DS4_no_ship | Gradient Boosting | 0.981 | 0.88 | sin shipping |
| DS5_no_ratios | Logistic Regression | 0.90 | 0.66 | sin ratios |

**Ganador con recency:** Gradient Boosting (DS1_H90_base) — AUC≈1.00, F1≈0.99  
**Ganador sin recency:** Gradient Boosting (DS3_H120) — AUC≈0.995, F1≈0.965  

---

## 5. Variables más influyentes

| Variable | Descripción | Influencia |
|-----------|--------------|-------------|
| `recency_days` | Días desde la última compra | Muy alta |
| `avg_shipping` | Promedio del costo de envío | Alta |
| `shipping_ratio` | Relación envío/total gastado | Alta |
| `avg_spent_per_cat` | Gasto promedio por categoría | Media |
| `n_orders` | Número total de compras | Media |

Sin incluir `recency_days`, destacan `avg_shipping`, `total_spent`, `avg_spent_per_cat`, `n_orders` y `age`.

---

## 6. Interpretación y acciones recomendadas

- Implementar alertas de retención entre los 60 y 75 días sin compra.  
- Bonificar envío a clientes con alto `shipping_ratio`.  
- Fomentar la compra cruzada (cross-selling) para aumentar `avg_spent_per_cat`.  
- Segmentar campañas según grupo etario y tipo de producto principal.  

---

## 7. Estructura del repositorio

```

├── data/                      # datasets derivados (opcional)
├── notebooks/
│   └── trabajo.ipynb          # notebook principal
├── src/                       # scripts reutilizables (ETL, features, model, evals)
├── reports/
│   ├── leaderboard_with_recency.csv
│   ├── leaderboard_no_recency.csv
│   └── gráficos y métricas (.png)
├── requirements.txt
├── README.md
└── .gitignore

````

---

## 8. Reproducción del entorno

1. Instalar dependencias:  
   ```bash
   pip install -r requirements.txt
````

2. Ejecutar el notebook:

   ```bash
   jupyter notebook notebooks/trabajo.ipynb
   ```

3. Visualizar los experimentos en MLflow:

   ```bash
   mlflow ui
   ```

   Luego abrir [http://127.0.0.1:5000](http://127.0.0.1:5000)

---

## 9. Colaboración y control de versiones

* Trabajo en grupo (hasta 3 integrantes).
* Evidencia de commits de todos los miembros.
* Branches y Pull Requests documentados.
* Código y notebooks reproducibles.

---

## 10. Evaluación (rúbrica oficial)

| Criterio                              | Descripción                                  | Ponderación |
| ------------------------------------- | -------------------------------------------- | ----------- |
| Definición y justificación del target | Claridad conceptual y horizonte temporal     | 15 %        |
| Preparación de datos                  | Limpieza, unión de tablas, dataset analítico | 20 %        |
| Modelado y evaluación                 | Calidad técnica y métricas                   | 25 %        |
| Uso de MLflow                         | Registro adecuado de experimentos            | 10 %        |
| Colaboración en GitHub                | Commits, branches, documentación             | 10 %        |
| Presentación y conclusiones           | Claridad e interpretación de resultados      | 20 %        |

---

## 11. Conclusiones

El proyecto logró construir un pipeline reproducible y trazable de predicción de churn.
El modelo de **Gradient Boosting** alcanzó un desempeño óptimo (ROC-AUC > 0.98) y permitió identificar variables clave asociadas al comportamiento de recompra.
La integración de métricas, MLflow y análisis interpretativo brinda una base sólida para implementar estrategias de retención efectivas.

