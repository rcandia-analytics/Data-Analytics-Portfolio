# 🔍 Drivers de Ingreso del Cliente — NovaRetail+ 2024

**Análisis correlacional para identificar qué comportamientos del cliente están más fuertemente asociados con el ingreso anual generado en una plataforma de e-commerce latinoamericana.**

---

## 🎯 Pregunta de Negocio

El equipo de **Crecimiento y Retención** de NovaRetail+ necesitaba responder antes del cierre de 2024:

> *¿Qué factores del comportamiento del cliente están más fuertemente asociados con el ingreso anual generado?*

⚠️ **Naturaleza del análisis:** exploratorio y correlacional. **Correlación ≠ causalidad.**

---

## 🗂️ Dataset

| Característica | Detalle |
|---|---|
| **Registros** | 15,000 clientes |
| **Columnas** | 12 (numéricas, binarias y categóricas) |
| **Variable objetivo** | `ingreso_anual` — ingreso generado por el cliente para la empresa |
| **Calidad** | Sin valores nulos ni duplicados; `edad` corregida de `float64` a `int` |

---

## 🛠️ Stack Tecnológico

* **Lenguaje:** Python (Jupyter Notebook)
* **Librerías:** Pandas, NumPy, Matplotlib, Seaborn, SciPy
* **Técnicas estadísticas:** Correlación de Pearson y Spearman, Punto-biserial, V de Cramér, detección de colinealidad

---

## ⚙️ Metodología

| Paso | Acción | Técnica |
|---|---|---|
| 1 | Carga y auditoría de calidad | `info()`, `describe()`, validación de rangos y categorías |
| 2 | Limpieza y documentación de supuestos | Corrección de tipos, validación de binarias y categóricas |
| 3 | Visualización de relaciones | Heatmap de correlación + Scatterplots de pares clave |
| 4 | Cálculo de correlaciones según tipo de variable | Pearson / Spearman · Punto-biserial · V de Cramér |
| 5 | Interpretación responsable | Evidencia → Interpretación no causal → Implicación |
| 6 | Limitaciones y próximos pasos | Qué no se puede concluir y cómo validarlo |

La selección del coeficiente se basó en el **tipo de variable**: numérica vs numérica (Pearson/Spearman), binaria vs numérica (Punto-biserial) y categórica vs categórica (V de Cramér).

---

## 📊 Visualizaciones Clave

### Matriz de correlación
![Heatmap de correlación](./images/heatmap_correlacion.png)

### Pares de variables con relación relevante
![Scatterplots de pares clave](./images/scatter_pares_clave.png)

---

## 💡 Hallazgos Principales (Framework C-F-I)

### 1. 🛒 La frecuencia de compra domina el ingreso
* **Contexto:** se evaluó la relación de 6 variables numéricas con el ingreso anual.
* **Hallazgo:** `compras_mes` presenta una correlación de **0.97** (Pearson y Spearman), lo que indica **colinealidad**: los clientes con 0 compras generan exactamente 0 de ingreso.
* **Implicación:** la palanca principal de crecimiento es **aumentar la frecuencia de compra**; las iniciativas deben medirse por su impacto en compras, no solo en tráfico.

### 2. 📱 Brecha de conversión: visitar no garantiza comprar
* **Contexto:** análisis de la actividad en la plataforma frente a compras e ingreso.
* **Hallazgo:** las visitas se asocian débilmente con compras (**0.35**) e ingreso (**0.34**); existen clientes con 15–20 visitas mensuales y 0 compras.
* **Implicación:** más que atraer tráfico, conviene optimizar lo que convierte una visita en compra (recomendaciones, checkout, ofertas personalizadas).

### 3. 📢 La publicidad se asocia más con visitas que con ingreso
* **Contexto:** evaluación del gasto en publicidad dirigida.
* **Hallazgo:** correlación moderada con visitas (**0.58**) pero débil con ingreso (**0.20**). Existe riesgo de **causalidad inversa**: la publicidad podría asignarse a usuarios que ya eran activos.
* **Implicación:** validar mediante un **experimento A/B** antes de incrementar el presupuesto publicitario.

### 4. 💎 Los clientes premium generan 26 % más sin comprar más seguido
* **Contexto:** correlación punto-biserial entre membresía premium e ingreso.
* **Hallazgo:** premium genera **44.58** vs **35.30** de no premium (r = 0.093, p < 0.001), sin relación con frecuencia de compra (0.003) ni visitas (-0.013).
* **Implicación:** el segmento premium es estratégico; se recomienda analizar su ticket promedio y probar experimentalmente la promoción de la membresía.

### 5. 🚪 El abandono no se explica con los datos disponibles
* **Contexto:** análisis del abandono frente a todas las variables del dataset.
* **Hallazgo:** sin relación con ingreso (r = -0.003), comportamiento, dispositivo ni región (V de Cramér < 0.02). Quienes abandonan generaban el mismo ingreso que quienes se quedan.
* **Implicación:** cada abandono es una pérdida de valor promedio; el equipo de retención debe **recolectar nueva información** (encuestas de salida, soporte, precios).

### 6. ⭐ Perfil, satisfacción y segmentación geográfica no influyen en el ingreso
* **Hallazgo:** satisfacción (0.056), ingreso personal (0.017), edad (0.017), dispositivo y región muestran asociación prácticamente nula con el ingreso.
* **Implicación:** segmentar por estas variables **no aporta valor**; las estrategias pueden aplicarse de forma general.

---

## ⚠️ Limitaciones

* Análisis correlacional: no permite establecer relaciones causales.
* Diferencia de escala temporal: métricas mensuales (`visitas_mes`, `compras_mes`) frente a una métrica anual (`ingreso_anual`).
* Variables faltantes clave: ticket promedio, antigüedad del cliente, canal de adquisición y motivos de abandono.
* Con 15,000 registros, relaciones mínimas resultan significativas; la interpretación se basó en la **magnitud**, no solo en el p-value.

---

## 🚀 Próximos Pasos

1. **Segmentación por comportamiento:** premium vs no premium (ticket promedio), usuarios activos sin compras y niveles de gasto publicitario.
2. **Experimentos A/B:** medir el efecto real de la publicidad y de la promoción premium.
3. **Enriquecimiento de datos:** información de abandono, series mensuales por cliente y modelos de regresión multivariada.

---

## 📁 Estructura del Proyecto

```
06_Python_Correlaciones_NovaRetail/
├── README.md
├── novaretail_analisis_correlacion.ipynb
├── data/
│   ├── novaretail_comportamiento_clientes_2024.csv
│   └── novaretail_comportamiento_clientes_2024_clean.csv
└── images/
    ├── heatmap_correlacion.png
    └── scatter_pares_clave.png
```

---
*Análisis desarrollado como parte de mi formación en Análisis de Datos. Todos los resultados son reproducibles ejecutando el notebook de principio a fin.*

[⬅️ Volver al portafolio principal](../)
