# Desafio
1. El propósito del análisis realizado.
2. La estructura del proyecto y organización de los archivos.
3. Ejemplos de gráficos e insights obtenidos.
4. Instrucciones para ejecutar el notebook.
<img width="1414" height="603" alt="image" src="https://github.com/user-attachments/assets/0cb0eeab-282c-44f5-b5e1-e585adefe79a" />
<img width="1246" height="582" alt="image" src="https://github.com/user-attachments/assets/41d979d5-6260-4f3b-b6bd-c20a4e62ea4a" />
<img width="1268" height="549" alt="image" src="https://github.com/user-attachments/assets/88486237-396e-46de-af17-6b56d86cc0d8" />


======INFORME DE ANÁLISIS FINAL======
# ===============================================================
#  ANÁLISIS DE TIENDAS - PYTHON | GOOGLE COLAB
# Autor: Ruth Jimenez
# Objetivo: Analizar las métricas de rendimiento de varias tiendas
# ===============================================================

# ============================
# 1 Importar librerías
# ============================
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Configurar estilo visual
plt.style.use('seaborn-v0_8')
sns.set_palette('viridis')
plt.rcParams['figure.figsize'] = (10,6)

# ============================
# 2 Cargar base de datos
# ============================
# revisar si esta DataFrame llamado "df"
# df = pd.read_csv('/path/tu_archivo.csv')

# ============================
# 3 FUNCIONES ANALÍTICAS
# ============================

# ----  1: Calcular ingresos totales por tienda ----
def calcular_ingresos(df):
    ingresos = df.groupby('Tienda')['Precio'].sum().reset_index()
    ingresos.rename(columns={'Precio': 'Ingreso_Total'}, inplace=True)
    return ingresos

# ---- 2: Calcular productos vendidos por categoría ----
def productos_por_categoria(df):
    return df.groupby(['Tienda', 'Categoría']).size().reset_index(name='Cantidad_Vendida')

# ---- 3: Calificaciones promedio por tienda ----
def calificaciones_promedio(df):
    return df.groupby('Tienda')['Calificación'].mean().reset_index(name='Promedio_Calificación')

# ----  4: Productos más y menos vendidos ----
def productos_mas_menos_vendidos(df):
    conteo = df.groupby(['Tienda','Producto']).size().reset_index(name='Cantidad_Vendida')
    mas_vendidos = conteo.loc[conteo.groupby('Tienda')['Cantidad_Vendida'].idxmax()]
    menos_vendidos = conteo.loc[conteo.groupby('Tienda')['Cantidad_Vendida'].idxmin()]
    return mas_vendidos, menos_vendidos

# ----  5: Costo de envío promedio por tienda ----
def costo_envio_promedio(df):
    return df.groupby('Tienda')['Costo_envío'].mean().reset_index(name='Costo_Envio_Promedio')

# ============================
# 4 FUNCIONES
# ============================

ingresos = calcular_ingresos(df)
categorias = productos_por_categoria(df)
calificaciones = calificaciones_promedio(df)
mas_vendidos, menos_vendidos = productos_mas_menos_vendidos(df)
envio = costo_envio_promedio(df)

# Mostrar resultados en pantalla
print(" Ingresos Totales por Tienda:\n", ingresos)
print("\n Productos Vendidos por Categoría:\n", categorias)
print("\n Calificaciones Promedio:\n", calificaciones)
print("\n Productos Más Vendidos:\n", mas_vendidos)
print("\n Productos Menos Vendidos:\n", menos_vendidos)
print("\n Costo de Envío Promedio:\n", envio)

# ============================
# 5 VISUALIZACIONES DE LOS GRAFICOS
# ============================

# ---- 1: Ingresos Totales por Tienda ----
plt.figure()
sns.barplot(data=ingresos, x='Tienda', y='Ingreso_Total')
plt.title(' Ingresos Totales por Tienda')
plt.ylabel('Ingresos ($)')
plt.xlabel('Tienda')
plt.show()

# ----2: Calificaciones Promedio ----
plt.figure()
sns.lineplot(data=calificaciones, x='Tienda', y='Promedio_Calificación', marker='o')
plt.title(' Calificación Promedio por Tienda')
plt.ylabel('Promedio de Estrellas')
plt.xlabel('Tienda')
plt.show()

# ----  3: Distribución de Ventas por Categoría ----
plt.figure()
sns.barplot(data=categorias, x='Categoría', y='Cantidad_Vendida', hue='Tienda')
plt.title(' Distribución de Ventas por Categoría')
plt.ylabel('Cantidad Vendida')
plt.xlabel('Categoría')
plt.show()

# ============================
# 6 ANÁLISIS DE LOS GEOGRÁFICO
# ============================
plt.figure()
plt.scatter(df['lon'], df['lat'], c='blue', alpha=0.6)
plt.title(' Distribución Geográfica de Ventas')
plt.xlabel('Longitud')
plt.ylabel('Latitud')
plt.show()

# ============================
# 7 INFORME FINAL
# ============================

print("""
 INFORME DE ANÁLISIS FINAL

Tras el análisis de las tiendas:
- La tienda con **mayores ingresos** es: {}
- Las **categorías más populares** se encuentran en las tiendas con mayores ventas.
- En promedio, las **calificaciones más altas** corresponden a la tienda: {}
- Los **productos más vendidos** varían según la tienda, destacando {}.
- El **costo de envío más bajo** pertenece a: {}

 Recomendación:
El Sr. Juan debería vender en la tienda con mayores ingresos, mejor calificación promedio
y costos de envío razonables, ya que esto maximiza la rentabilidad y satisfacción del cliente.
""".format(
    ingresos.loc[ingresos['Ingreso_Total'].idxmax(), 'Tienda'],
    calificaciones.loc[calificaciones['Promedio_Calificación'].idxmax(), 'Tienda'],
    mas_vendidos['Producto'].unique(),
    envio.loc[envio['Costo_Envio_Promedio'].idxmin(), 'Tienda']
))
#======INFORME DE ANÁLISIS FINAL======
# ===============================================================
#======cAPTURAS DE PANTALLA======
# ===============================================================

<img width="722" height="509" alt="image" src="https://github.com/user-attachments/assets/e0a5964a-3ed5-4f6e-9bf7-370cb0f4fcfe" />
<img width="772" height="441" alt="image" src="https://github.com/user-attachments/assets/16e72d56-4aea-40a8-bd8d-8194fa17e0d9" />
<img width="678" height="292" alt="image" src="https://github.com/user-attachments/assets/40a2475c-6b0d-4cbd-abe1-afdc029a63bb" />
<img width="1227" height="329" alt="image" src="https://github.com/user-attachments/assets/78dea693-49a3-4098-812c-c80f9cec2026" />
