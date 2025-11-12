
**Justificación conceptual del churn**

Definimos como churn a aquellos clientes que no realizaron compras en los 90 días posteriores al 31/08/2024. 
Este horizonte refleja el ciclo comercial típico del e-commerce (clientes frecuentes realizan al menos una compra trimestral). 
Por tanto, si un cliente no registra actividad en ese lapso, se considera inactivo o perdido.

En la fase de **feature engineering**, se generaron nuevas variables que representan comportamientos clave de los clientes:

spent_per_order: ticket promedio.
shipping_ratio: proporción de costo de envío sobre gasto total.
avg_spent_per_cat: gasto promedio relativo a la diversidad de categorías.
orders_per_recency: frecuencia ajustada al tiempo desde la última compra.
age_group: segmentación etaria para detectar diferencias demográficas.

Los análisis exploratorios confirmaron que la recencia y la frecuencia de compra son los principales predictores de churn.
Clientes con alta recencia, bajo gasto total y poca diversidad de productos son los más propensos a abandonar.
Este conocimiento orienta al negocio a diseñar campañas de retención específicas (por ejemplo, descuentos segmentados por inactividad o por categoría dominante).