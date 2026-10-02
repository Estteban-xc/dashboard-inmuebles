# Dashboard de ventas de inmuebles (2004–2007)

Dashboard interactivo para analizar el comportamiento de las ventas de una empresa inmobiliaria con operación en Barcelona, Girona, Lleida y Tarragona.

🔗 **Ver el dashboard:** https://estteban-xc.github.io/dashboard-inmuebles/
---

## Contexto

La dirección de la empresa necesitaba información clara para responder preguntas como:

- ¿Cuáles son los productos (tipos de inmueble) con mayor y menor venta?
- ¿Qué provincia genera mayores ventas?
- ¿Cómo se comportan las ventas a través del tiempo?
- ¿Qué tipos de clientes (operaciones) representan mayor participación?
- ¿Qué información debería conocer un gerente antes de tomar decisiones?

El objetivo fue transformar los datos disponibles en información visual, comprensible y útil para la toma de decisiones, siguiendo la secuencia **dato → información → hallazgo → decisión**.

## Datos

La fuente es el archivo [`BD-Inmuebles.xls`](BD-Inmuebles.xls), incluido en este repositorio sin modificaciones: **3.337 operaciones** cerradas entre abril de 2004 y mayo de 2007.

| Columna | Tipo | Descripción |
|---|---|---|
| Referencia | Entero | Identificador del inmueble |
| Fecha Alta | Fecha | Fecha en que el inmueble se registró |
| Tipo | Texto | Casa, Industrial, Local, Oficina, Parking, Piso o Suelo |
| Operación | Texto | Venta o Alquiler |
| Provincia | Texto | Barcelona, Girona, Lleida o Tarragona |
| Superficie | Entero | Área en m² (40 a 300) |
| Precio Venta | Entero | Valor de la operación (25 a 59 millones) |
| Fecha Venta | Fecha | Fecha en que se cerró la operación |
| Vendedor | Texto | Carmen, Jesús, Joaquín, Luisa, María o Pedro |

La base no indica la moneda, por lo que los valores se expresan en **millones de unidades monetarias (M u.m.)**.

## Metodología

1. **Revisión de calidad.** Se verificó que no hubiera valores nulos ni referencias duplicadas y que los rangos de precio y superficie fueran razonables.
2. **Validación de fechas.** Se calculó `Días cierre = Fecha Venta − Fecha Alta`. Esta validación reveló que **909 registros (27,2 %)** tienen una fecha de venta anterior a la fecha de alta, algo imposible. Todos corresponden a inmuebles dados de alta en 2006 y 2007.
3. **Tratamiento de los registros inconsistentes.**
   - Para el análisis por tipo, provincia, operación y vendedor se usan los 3.337 registros, porque esos campos no presentan errores.
   - Para el análisis temporal se compara la serie completa contra la serie con solo los 2.428 registros válidos, porque las fechas erróneas distorsionan la tendencia.
4. **Variable de cliente.** La base no contiene un campo de tipo de cliente, así que se usó el **tipo de operación** (venta o alquiler) como aproximación.
5. **Cálculo de indicadores.** Se calcularon totales, participaciones, ticket promedio, tiempo medio de cierre, series mensuales, trimestrales y anuales, y la correlación entre superficie y precio.
6. **Diseño de visualizaciones.** Cada pregunta de negocio se asoció al tipo de gráfico más adecuado:

| Necesidad | Visualización |
|---|---|
| Mostrar KPI | Tarjetas indicadoras |
| Comparar ventas por producto | Barras horizontales |
| Comparar regiones | Barras verticales y mapa de calor |
| Analizar evolución en el tiempo | Líneas (mensual, trimestral, anual) |
| Analizar participación | Barras apiladas |
| Analizar relación entre variables | Dispersión |

## Estructura del dashboard

- **Indicadores clave (KPI):** ventas totales, número de operaciones, ticket promedio, tiempo medio de cierre y porcentaje de registros inconsistentes.
- **Filtros:** provincia, tipo de inmueble, operación, vendedor, rango de años y un interruptor para excluir los registros inconsistentes. Todos los gráficos y KPI se recalculan al cambiar cualquier filtro.
- **Pestaña 1, Producto y región:** ventas por tipo de inmueble, ventas por provincia, mapa de calor provincia × tipo y ventas por vendedor.
- **Pestaña 2, Evolución en el tiempo:** línea de ventas con y sin registros inconsistentes, y ventas por año y provincia.
- **Pestaña 3, Participación y relaciones:** participación venta/alquiler por provincia, dispersión superficie vs. precio y composición por tipo de inmueble.
- **Pestaña 4, Calidad de datos:** distribución de los registros inconsistentes por año de alta y por días entre alta y venta.
- **Pestaña 5, Base de datos original:** tabla completa con buscador, orden por columna y registros inconsistentes resaltados; descarga del archivo original y en CSV; diccionario de datos.

## Principales hallazgos

1. **Calidad de datos.** El 27,2 % de los registros tiene fechas imposibles. Sin depurarlos, las ventas parecen alcanzar su pico en diciembre de 2004; con los datos válidos, el máximo es en mayo de 2006.
2. **Tendencia.** Las ventas crecieron en 2004–2005, se mantuvieron estables hasta mediados de 2006 y cayeron desde el tercer trimestre de 2006 (−32 % entre el segundo y el cuarto trimestre). Los datos de 2007 están incompletos.
3. **Provincia.** Lleida es la de menor venta (23,7 % del total, 8 % por debajo de Girona). Su rezago se debe al bajo volumen de operaciones, no al precio: tiene el ticket promedio más alto.
4. **Producto.** No hay un producto dominante. Industrial lidera con 14,8 % y Suelo es el más bajo con 13,6 %.
5. **Comportamientos inesperados.** El precio no se relaciona con la superficie (r = −0,02), y los alquileres registran montos equivalentes a las ventas.

## Tecnologías

- **HTML, CSS y JavaScript**, sin frameworks. Los datos están incluidos en la propia página, así que no requiere servidor.
- **[Plotly.js](https://plotly.com/javascript/)** para las gráficas interactivas.
- **Python (pandas)** para la exploración inicial, la limpieza y la exportación de los datos.
- **GitHub Pages** para la publicación. Al ser un sitio estático, está disponible de forma permanente.

## Estructura del repositorio

```
├── index.html          # Dashboard completo (datos incluidos)
├── BD-Inmuebles.xls    # Base de datos original, sin modificaciones
└── README.md
```

## Uso local

No requiere instalación: descarga el repositorio y abre `index.html` en cualquier navegador. Solo necesitas conexión a internet para cargar la librería de gráficas.

## Equipo

| Rol | Responsabilidad | Integrante |
|---|---|---|
| Analista de datos | Revisa, limpia e interpreta los datos | Samuel Pardo |
| Diseñador del dashboard | Define estructura, gráficos y distribución | Esteban Varela |
| Analista ejecutivo | Identifica hallazgos y conclusiones | Nelson Martinez |
| Presentador | Expone los resultados del equipo | Sebastian Torres |
