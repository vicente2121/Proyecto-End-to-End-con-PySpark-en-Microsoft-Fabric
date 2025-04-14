# Proyecto End-to-End de Análisis Logístico con Microsoft Fabric y Power BI

Este proyecto fue desarrollado de forma individual como parte del reto **FP20**, y ejecutado en **1 hora y 20 minutos**. Se construyó un flujo completo de análisis de datos utilizando **Microsoft Fabric**, desde la carga del archivo hasta la visualización final en **Power BI**, pasando por la creación de un **modelo dimensional tipo estrella**.

[🎥 Ver el video del proyecto en YouTube](https://youtu.be/YsfH7ZyOsIY)

---

## 🛠️ Herramientas Utilizadas

- Microsoft Fabric (Dataflow Gen2, Lakehouse, Data Warehouse, Semantic Model)
- PySpark (en notebooks)
- Power BI
- Power Query
- DAX
- Excel

---

## 🎯 Objetivo del Proyecto

Analizar datos de inventario, entregas, productos y proveedores con un enfoque logístico. Se buscó generar un flujo estructurado en capas Bronce, Plata y Oro, integrando limpieza, modelado y visualización de datos para facilitar la toma de decisiones basada en información.

---

## 🧱 Arquitectura del Proyecto

### 🟫 Capa Bronce – Ingesta de Datos Crudos

- Carga directa de un archivo Excel (“FP 20 RETO ABRIL”) al **Lakehouse** sin transformaciones.
- Conservación de datos originales para trazabilidad y respaldo histórico.

![Capa Bronce](capabronce.png)

---

### 🪙 Capa Plata – Transformación y Limpieza

- Limpieza y normalización de datos desde notebooks PySpark:
  - Eliminación de duplicados
  - Tratamiento de valores nulos
  - Conversión de tipos y estandarización
- Carga a un **Data Warehouse** para modelado.

![Capa Plata](Capasilver.png)

---

### 🥇 Capa Oro – Modelado Dimensional y Semántico

- Construcción de un **modelo dimensional tipo estrella**, compuesto por:
  - Tabla de hechos: inventario y métricas de entrega
  - Tablas de dimensiones: Producto, Proveedor, Cliente, Transporte, Origen, Destino, Material, Calendario
- Desarrollo de un **modelo semántico** conectado a Power BI para exploración de datos.

![Capa Oro](capagold.png)
![Modelo Final](modelofinal.png)

---

## 📊 Visualización en Power BI

Se diseñó un dashboard con los siguientes indicadores:
Total de Inventario Disponible
Promedio de Días de Entrega
Ranking de Productos por Precio en su Categoría

![Dashboard Power BI](Visulizaacion.png)

---

## 📐 Medidas DAX Utilizadas

Durante la etapa de análisis, se crearon medidas clave en DAX para responder preguntas del negocio:

### ✅ Pregunta 1: Total de Inventario Disponible

```DAX
Pregunta 1 = 
    CALCULATE(SUM(fact_inventario[Cantidad_Inventario]))
Pregunta 2 = 
AVERAGE(fact_inventario[Días_Entrega])
Pregunta 3 = 
RANKX(
    FILTER(ALL(dim_producto), dim_producto[Categoria] = MAX(dim_producto[Categoria])),
    CALCULATE(MAX(fact_inventario[Precio_Unitario])),
    ,
    DESC
)


