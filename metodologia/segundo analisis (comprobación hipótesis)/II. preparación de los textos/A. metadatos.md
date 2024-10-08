# metadatos
Más arriba dije que los scripts con los que descargué los PDFs generan un documento JSON. A continuación voy a explicar qué es un documento JSON, la importancia de los metadatos en los estudios literarios cuantitativos [VER] y cómo funcionan, a grandes rasgos, los scripts que escribí. 

### Qué es un documento JSON
JSON son las siglas de JavaScript Object Notation. JavaScript es un lenguaje de programación, y JSON es una forma de escribir documentos para transportar información fácilmente (que surgió de la forma en la que se usan los objetos en JavaScript [^s.II.A.1]). Lo bonito del formato JSON es que es muy fácil de leer, tanto para las personas como para las computadoras, lo que permite transportar información de un lenguaje de programación a otro y además modificar manualmente los datos. Una entrada de los metadatos de un trabajo de titulación se ve así:
	{"nombre": "Torres Gutiérrez, Cristian Miguel", 
	 "titulo": "\"A speaking eye\" : el discurso pictórico en Hero and leander de Christopher Marlowe", 
	 "año": "2017", 
	 "doc_num": "0755725"}.
Como es evidente, espero, es un formato muy útil para organizar bases de datos.

### Metadatos en los estudios literarios cuantitativos
[POR VER SI SÍ QUIERO ESCRIBIR ESTO AT ALL]

### Cómo conseguí los metadatos
Mis scripts para los metadatos (https://github.com/diego-g-fonte/crawler/tree/main/metadata) funcionan de la siguiente manera. Primero hay que, desde nuestro navegador web, hacer una búsqueda en TESIUNAM. Yo evidentemente busqué "licenciatura en lengua y literaturas modernas inglesas", pero mi código funciona con cualquier búsqueda. Hay que copiar y pegar el URL de la primera página de resultados en el archivo que llamé "url_manual". Después de eso, hay que correr los scripts llamados "1.py" y "0.2.py"[^s.II.A.2]. Éstos generan el archivo JSON con los metadatos. Mi código no es perfecto, entonces, después de que los scripts lo hayan escrito, hay que retocar el JSON manualmente un poco. Con eso hecho, se tiene una base de datos con los metadatos de todos los trabajos de titulación de letras inglesas (o de todos los trabajos de titulación que haya arrojado TESIUNAM en la búsqueda inicial). Guardé la base de datos en un documento JSON porque los scripts que descargan los metadatos están en Python, pero el análisis lo hice con el lenguaje de programación R, y necesitaba una manera de transportar la información de un lenguaje a otro.


[^s.II.A.1]: No voy a entrar en detalles respecto a JavaScript ni sus objetos porque no escribí nada en JavaScript para este proyecto, y no soy experto en ese lenguaje.
[^s.II.A.2]: Mi forma de nombrar los archivos no es la mejor, lo sé.