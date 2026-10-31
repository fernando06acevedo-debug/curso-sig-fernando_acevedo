# C1 — Repaso de estándares con servicios reales

**Actividad:** Ruta C – C1  
**Tema:** Consulta y comparación de servicios geográficos web.

## 1\. Servicio WMS – DANE MGN 2024

Se consultó la operación `GetCapabilities` del servicio WMS del DANE correspondiente al Marco Geoestadístico Nacional (MGN) 2024. A partir de la revisión del documento XML se identificaron los siguientes elementos:

* **Capas identificadas:** 10, mediante las etiquetas `<Name>` del documento XML.
* **Sistemas de referencia (CRS):** `CRS:84`, `EPSG:4326` y `EPSG:3857`.
* **Formatos de salida de `GetMap`:** `image/bmp`, `image/jpeg`, `image/tiff`, `image/png`, `image/png8`, `image/png24`, `image/png32`, `image/gif` e `image/svg+xml`.

El servicio WMS permite visualizar información geográfica mediante imágenes cartográficas generadas desde el servidor. Los sistemas de referencia indican las opciones disponibles para solicitar dichas imágenes, mientras que los formatos permiten seleccionar cómo se representa y entrega el mapa al usuario.

### Evidencias WMS

!\[Etiqueta Name de una capa en GetCapabilities](evidencias\_C1/01\_image1.png)

!\[CRS declarados en GetCapabilities](evidencias\_C1/02\_image2.png)

!\[Formatos de salida de GetMap](evidencias\_C1/03\_image3.png)

## 2\. Servicio ArcGIS FeatureServer – Veredas de Boyacá 2024

Se consultó la capa 0 del servicio `Veredas\_Boyaca\_2024` mediante la operación REST `query`. La solicitud empleó los parámetros:

* `where=1=1`: consulta sin condición de filtrado por atributos.
* `outFields=\*`: solicita todos los campos disponibles.
* `resultRecordCount=3`: limita la respuesta a tres registros.
* `f=geojson`: solicita la respuesta en formato GeoJSON.

La respuesta contiene entidades dentro de `features`; los atributos se organizan en `properties` y la geometría en `geometry`. En las capturas se evidencia la consulta y los registros devueltos por el servicio.

### Evidencias FeatureServer

!\[Formulario de consulta del FeatureServer](evidencias\_C1/04\_image4.png)

!\[Respuesta JSON de la primera entidad](evidencias\_C1/05\_image5.png)

!\[Respuesta JSON de la segunda entidad](evidencias\_C1/06\_image6.png)

!\[Respuesta JSON de la tercera entidad](evidencias\_C1/07\_image7.png)

## 3\. Servicio OGC API Features – pygeoapi

Se revisó el endpoint de colecciones del servicio de demostración pygeoapi y se consultaron elementos de la colección `obs` con un límite de tres registros. La respuesta utiliza una estructura JSON basada en colecciones y entidades geográficas. En las entidades, los atributos se encuentran en `properties` y la geometría en `geometry`; también se pueden presentar enlaces de navegación en `links`.

La colección `obs` corresponde a datos de observaciones del servicio de demostración y no al mismo conjunto temático de las veredas de Boyacá. Por tanto, la comparación se realiza sobre la estructura y el funcionamiento de los servicios, no sobre la equivalencia temática de los datos.

### Evidencias pygeoapi

!\[Identificador de colección mostrado en la respuesta](evidencias\_C1/08\_image8.png)

!\[Respuesta de colecciones y enlaces](evidencias\_C1/09\_image9.png)

!\[Identificador de otra colección](evidencias\_C1/10\_image10.png)

!\[Detalle de colección y sus enlaces](evidencias\_C1/11\_image11.png)

!\[Identificador de colección adicional](evidencias\_C1/12\_image12.png)

!\[Respuesta JSON de colección y sus recursos](evidencias\_C1/13\_image13.png)

## 4\. Comparación de estándares y servicios

|Servicio o estándar|Qué entrega|Cómo se filtra|Cómo se pagina|
|-|-|-|-|
|**WMS**|Imágenes de mapas renderizadas por el servidor; no entrega normalmente las entidades vectoriales individuales.|Se seleccionan capas y parámetros de visualización en `GetMap`, como `LAYERS`, `BBOX`, `CRS` y `STYLES`. No es una consulta de registros de entidades.|No aplica paginación de entidades como en los servicios de datos; la respuesta corresponde a una imagen del área solicitada.|
|**WFS**|Entidades geográficas vectoriales con geometrías y atributos.|Mediante filtros espaciales y de atributos, por ejemplo OGC Filter.|Según la versión y capacidades del servidor, puede usar parámetros como `count` y `startIndex` (WFS 2.0); implementaciones anteriores pueden usar `maxFeatures`.|
|**ArcGIS FeatureServer**|Entidades (features) con geometría y atributos, en formatos como JSON o GeoJSON.|Mediante parámetros REST como `where`, `objectIds`, `geometry` y `outFields`.|Mediante `resultOffset` y `resultRecordCount`, cuando la capa y el servicio permiten paginación.|
|**OGC API Features**|Entidades geográficas organizadas en colecciones, con propiedades y geometrías, normalmente en JSON/GeoJSON.|Puede usar parámetros como `bbox` y `datetime`; filtros adicionales dependen de los requisitos implementados por el servidor y sus extensiones.|Utiliza `limit` y enlaces de navegación (`next`) cuando la respuesta está dividida en páginas.|



## 

