# Descripción General

Esta es una prueba práctica de Data Engineering. Tu objetivo es:

Descargar (o guardar en memoria) tres archivos 10-K de Apple (_.html o _.xml) desde la base de datos de la SEC.
Parsear los 3 estados financieros relevantes (Estado de resultados, Balance general y Estado de flujos de efectivo), así como el período fiscal (fiscal period) de cada reporte.
Limpiar y transformar la información en formato Parquet.
Cargar los datos transformados a una base de datos SQLite de forma incremental (cuando se agregue un nuevo 10-K, no se duplique la información existente).
Realizar al menos 5 análisis básicos (ejemplo: crecimiento interanual de ingresos).
Entregar un resumen de cómo ejecutarlo todo y los resultados que obtuviste.

Tiempo estimado: 2-3 horas.

Documentos 10-K de Apple:
Puedes elegir estos o cualquier 10-K distinto de Apple que esté público en EDGAR.

10-K 2022: https://www.sec.gov/Archives/edgar/data/320193/000032019322000108/aapl-20220924.htm
10-K 2021: https://www.sec.gov/Archives/edgar/data/320193/000032019321000105/aapl-20210925.htm
10-K 2020: https://www.sec.gov/Archives/edgar/data/320193/000032019320000096/aapl-20200926.htm

Nota 1.- Puedes elegir estos o cualquier 10-K distinto de Apple que esté público en EDGAR.
Nota 2.- Si prefieres trabajar en formato XML o XBRL, puedes buscar esos documentos dentro de los mismos enlaces de la SEC.

# 🔰 Detalles por Layer

## 🪵 Detalles Bronze Layer

## Punto a desarrollar

Tienes dos opciones:

1. Descargar manualmente o programáticamente los tres archivos 10-K y guardarlos en la carpeta:  **bronze/**.
      - Asegúrate de nombrarlos de forma clara, por ejemplo:
        - aapl_10k_2022.html
        - aapl_10k_2021.html
        - aapl_10k_2020.html

2. Leer la información y mantenerla en memoria (dataframe) los archivos 10-K .

**Nota.-** En la SEC, los archivos 10-K se encuentran en la sección "Archives" y se pueden descargar en diferentes formatos.
Los links que te proporcioné son en formato HTML. Puedes usar los archivos en formato TXT, XML/XBRL, o PDF si prefieres.

## 🌱 Detalles Silver Layer (Bronze → Silver)

Despues de cargar/descargar los archivos de SEC 10-K, se tiene la necesidad de extraer la siguiente información:

- Estado de Resultados (CONSOLIDATED STATEMENTS OF OPERATIONS)
- Balance General (CONSOLIDATED BALANCE SHEETS)
- Estado de Flujos de Efectivo (CONSOLIDATED STATEMENTS OF CASH FLOWS)
- Período Fiscal (e.g., Año Fiscal 2022, etc.)

## Puntos a desarrollar

* 1.- Solo extrae los valores numéricos (ej. Ingresos, Utilidad Neta, Activos Totales, etc.) y conviertelos a formato tabular.
* 2.- Genera un archivo Parquet en la carpeta: **silver/**.

**Nota 1.-** Considera unidades y valores (miles, millones, etc.) para tener datos consistentes.
**Nota 2.-** Si necesitas normalizar o limpiar (por ejemplo, remover caracteres extra, valores nulos, etc.), hazlo en esta fase. Vamos a correr un analysis basico, asi que es necesario normalizar los datos.
**Nota 3.-** Tus archivos .parquet finales deben tener todas las columnas relevantes (por ejemplo: fiscal_year, revenue, net_income, total_assets, total_liabilities, cash_flow_operating, ...) con tipos de datos apropiados (numéricos).
**Nota 4.-** Trata de hacer el ETL , lo mas automatizado posible , sin setteos que puedan parecer manual.

## 🌳 Detalles Golden Layer (Silver → Gold) 

## Puntos a desarrollar

* 1.- Guarda la información limpia en la carpeta: **golden/**
* 2.- Al mismo tiempo crea un proceso de carga incremental a una base de datos PostgreSQL local (archivo, por ejemplo financial_data.db)(Si un 10-K ya fue cargado, no lo dupliques).
* 3.- Realiza al menos 5 pequeños análisis (En SQLite) . Por ejemplo:

   - Crecimiento Interanual de Ingresos: Comparar ingresos de 2020 vs 2021 vs 2022.
   - Márgen Neto: (Utilidad Neta / Ingresos) para cada año.
   - Evolución de Activos vs Pasivos: Porcentaje de cambio año contra año.
   - Flujo de Efectivo Operativo: Comparar la tendencia en los 3 años.
   - Indicador personalizado: Por ejemplo, (Efectivo y equivalentes / Pasivos corrientes) para medir liquidez.
     Resultado: Puedes imprimir (en consola) o generar un pequeño reporte CSV/Parquet con los resultados.

**Nota 1.-** La idea es que hagas un análisis básico y sencillo, no es necesario que hagas un análisis complejo.
**Nota 2.-** Asegúrate de incluir un script o un método claro para verificar el contenido cargado. Por ejemplo, un `"SELECT * FROM financials;"` que muestre las filas.

## ➡️ Entregables

**Código**

- Incluye los scripts (o notebooks) necesarios para que se pueda reproducir tu solución.
- Los nombres de funciones y métodos son libres, pero recomendamos separaciones claras (parseo, transformación, carga, análisis).
- La prueba debe ser reproducible, es decir, que si alguien más ejecuta el código, debe poder reproducir los resultados.

**Archivo de Instrucciones**

- Puede ser este mismo README.md actualizado o un README_PROYECTO.md adicional.
- Explica cómo correr cada paso (instalación de dependencias, comandos, etc.).

**Archivos Parquet/HTML**

- Tus archivos parquet finales en silver/, con la data limpia y transformada.

**Base de Datos SQLite**

- Evidencia de que cargaste y se puede cargar los datos.

**Resultados de Análisis**

- Un pequeño resumen (en la terminal o en archivo) con los 5 análisis solicitados.

## 💻 Componentes Técnicos

- Lenguaje: Python, SQL
  Ambiente/Contenerización: Docker/Docker-Compose
- Para Manejo y Análisis De Datos: Pandas, Pyspark, Apache Beam
--Librerías recomendadas:
  - requests o urllib para descarga (si decides automatizarla)
  - sqlite3 (embebida en Python) o SQLAlchemy para la base de datos.
  - BeautifulSoup o lxml para parsear HTML/XML.
- Manejo de Errores: try-except,logging, explica de manera simple cómo manejas datos faltantes o secciones que no existan.
- Performance: No es el foco principal, pero se valora si explicas optimizaciones.

##  🚀 Cómo Empezar

Haz un fork o clona este repositorio.
Crea tu script o pipeline que lea la carpeta bronze/, extraiga la info relevante y guarde en silver/.
Genera y actualiza la base de datos SQLite en la fase gold/.
Ejecuta tu análisis y muestra los resultados.

## ✍🏻Evaluación

Agregaremos el 10-K de Apple 2023 para probar tu solución.

- Si tu solución es correcta, debería poder cargar los datos y hacer los análisis correctamente.
- Deberia ser facil de entender y ejecutar.

¡Listo!
Con esto deberías tener una visión clara de los pasos y entregables requeridos. Cualquier decisión de diseño o implementación adicional (por ejemplo, esquemas de base de datos, nombre de tablas, formato de logs) es totalmente tuya. ¡Éxitos!
