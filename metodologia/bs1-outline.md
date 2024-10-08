Recordando lo que hice:

# Primer análisis (títulos 1962-2022)

- Con el lenguaje de programación Python, escribí un script para obtener el nombre de lx autorx, el título y el año de publicación de todos los trabajos de titulación de la licenciatura en lengua y literaturas modernas inglesas hasta el 2022. Esto está en (https://github.com/diego-g-fonte/metadatos).
- Con el lenguaje de programación R, grafiqué:
	+ la cantidad de publicaciones por año.
	+ la longitud promedio anual de los títulos de las publicaciones.
	+ el porcentaje de tíulos con el caracter ":".
	+ Esto está en (https://github.com/diego-g-fonte/analisis).

# Segundo análisis (comprobación de hipótesis)

## Preparación de los textos


-[DESCRIPCIÓN DETALLADA DE CÓMO CONSEGUÍ LOS METADATOS, Y DE QUÉ ES UN JSON]
	
-[DESCRIPCIÓN DETALLADA DE CÓMO CONSEGUÍ LOS PDFS. CÓMO FUNCIONA EL CRAWLER Y CÓMO USARLO]

-Pasé los trabajos de titulación de PDF a texto simple (es decir, texto sin formato: sin tipografía especificada, negritas, subrayados, itálicas, tamaños de letra variables, justificación, interlineado, etcétera) con los scripts en Python que están en (https://github.com/diego-g-fonte/pdfs_a_texto/). Por la naturaleza de los archivos PDF no pude preservar mucha información que hubiera sido valiosa, en específico la separación de los párrafos, la capitulación de los trabajos, qué texto es citado y qué texto es propio de lx autorx, etcétera. Esto es porque en el formato PDF, el texto no está almacenado en un cuadro de texto como en los procesadores de texto, sino que cada caracter es un elemento independiente, con información de su posición, su tamaño, su color, etcétera. Como dice el manual del paquete "pdfminer.six", "Most PDF files look like they contain well-structured text. But the reality is that a PDF file does not contain anything that resembles paragraphs, sentences or even words. When it comes to text, a PDF file is only aware of the characters and their placement" (pdfminer.six). Lo que resultó fue que un texto que originalmente se veía así: [FOTO DE LA PÁGINA LEGAL], se vea así: "UNAM – Dirección General de Bibliotecas Tesis Digitales Restricciones de uso DERECHOS RESERVADOS © PROHIBIDA SU REPRODUCCIÓN TOTAL O PARCIAL Todo el material contenido en esta tesis esta protegido por la Ley Federal del Derecho de Autor (LFDA) de los Estados Unidos Mexicanos (México). El uso de imágenes, fragmentos de videos, y demás material que sea objeto de protección de los derechos de autor, será exclusivamente para fines educativos e informativos y deberá citar la fuente donde la obtuvo mencionando el autor o autores. Cualquier uso distinto como el lucro, reproducción, edición o modificación, será perseguido y sancionado por el respectivo titular de los Derechos de Autor", o uno que se veía así: [FOTO DE LA PRIMERA PÁGINA DE UNA TESIS], se vea así: [FOTO DEL MISMO TEXTO EN VSCODE].

-Junté todo en un gran JSON. Describir el .json.

## análisis

-Todo mi análisis lo hice con el lenguaje R, inspirado sobre todo en el trabajo que hizo Matthew Jockers en su libro *Macroanalysis*, en específico los capítulos "Style", "Theme" y "Influence".

-RESUMEN RÁPIDO: Para el estilo usé una estrategia utilizada en los estudios de atribución autoral que está basada en las frecuencias relativas de las palabras. Frecuencia relativa sólo quiere decir que del 100% de las palabras de un texto, X% es la palabra "de", Y% es la palabra "la", etc. Por ejemplo, en la tesina titulada "El género policíaco y el gótico dentro de The virgin suicides de Jeffrey Eugenides", la palabra "de" conforma el 4.16% de las palabras, "la" el 3.29, etcétera. Generé, con el paquete "stylo", una tabla con las 5,000 palabras más comunes en todos los trabajos de titulación y su frecuencia relativa en cada uno. En estilometría, las frecuencias relativas de las palabras más comunes son lo que conforma el estilo de un texto. Ya teniendo la tabla con las frecuencias relativas, usé una función ("dist") que crea un espacio multidimensional (en este caso de 5,000 dimensiones) y coloca un punto por cada trabajo de titulación en ese espacio. Es más fácil pensar en un espacio de dos dimensiones en el que el eje X es la frecuencia relativa de la palabra "de", y el eje Y la de la palabra "la". Dentro de este espacio, el trabajo "El género policíaco ..." se convertiría en un punto con las coordenadas (4.16, 3.29). Otro trabajo de titulación, por ejemplo "Propuesta de lecciones-muestra para la enseñanza de la comprension de lectura en ingles en el marco de la ENA", tendría las coordenadas (7.11, 4.01). Podemos trazar una línea entre esos dos puntos y medir qué tan cerca o lejos están (en otras palabras, qué tan similares o diferentes son). La computadora puede hacer ese mismo proceso en 5,000 dimensiones y calcular las distancias entre todos los puntos. La primera gráfica que te envío ("estilo.png") visualiza el promedio de las distancias entre los trabajos de titulación publicados en un año, de los años 2006 al 2023. En palabras menos técnicas, si el punto es alto, los trabajos de ese año son diferentes, y si el punto es bajo, son similares.

-Para los temas utilicé una técnica llamada topic modeling. Usé un programa (MALLET, cuyas especificidades también me superan) que, al procesar los textos, produce una tabla con una cantidad X de temas (yo, arbitrariamente, elegí 50) y la cantidad de ese tema que tiene cada texto. Lo podemos pensar como que cada texto tiene un 100% de "tema", del cual 50% puede ser Jane Austen, 25% narratología, etc. Con esa tabla usé el mismo método de las muchas dimensiones y el resultado es la gráfica "temas.png". Es decir, si en un año, el punto es alto, los temas de los trabajos fueron diferentes (según MALLET), y si el punto es bajo, fueron similares.

### tema

#### [DESCRIPCIÓN DETALLADA DE CÓMO LLEGUÉ A ESTE MÉTODO]
+ Primero intenté sacar los temas con las palabras clave, pero eso no tiene sentido. [POR QUÉ NO TIENE SENTIDO]
+ Luego aprendí a usar MALLET. (Quita las stopwords)
	* [EXPLICAR CÓMO FUNCIONA MALLET, COMO PUEDA. DAR "READ MORE"]
	* Creí que tal vez sería buena idea graficar los temas a través del tiempo, pero no logré sacar información valiosa haciendo eso [GRÁFICA DE TODOS LOS TEMAS. PERSEGUIRLO O DEJARLO COMO UNA SUGERENCIA PARA FUTURAS INVESTIGACIONES].
	* Hice muchos experimentos moviendo el número de temas que generaba. Una curiosidad es que cuando se acotan los temas a dos, tres o cuatro, reconoce la diferencia entre una tesis/tesina, una traducción y un trabajo sobre didáctica (cuando son sólo dos, diferencia entre tesis/tesina y el resto de trabajos). [EVIDENCIA] Decidí no perseguir esto [CHANCE DECIDO SÍ PERSEGUIRLO, O LO DEJO COMO UNA SUGERENCIA PARA FUTURAS INVESTIGACIONES]
+ Al final decidí hacer como le hice en el estilo y usar la tabla que saca MALLET [FOTO DE LA TABLA].

#### [DESCRIPCIÓN DETALLADA DEL MÉTODO]

### estilo

#### [DESCRIPCIÓN DETALLADA DE CÓMO LLEGUÉ A ESTE MÉTODO]
+ Primero creí que iba a ser buena idea ver las longitudes promedio anuales de los trabajos. [POR QUÉ AL FINAL NO]
+ También intenté contar la cantidad de comillas. [POR QUÉ AL FINAL NO]
+ Al final vi la tabla de 5000 palabras y sus frecuencias relativas en cada trabajo y me iluminó el señor [CAMBIAR]. 

#### [DESCRIPCIÓN DETALLADA DEL MÉTODO]

- [DAR UNA EXPLICACIÓN DE LA ESTILOMETRÍA CON FUENTES Y EJEMPLOS]
- [EXPLICAR EL MÉTODO DETALLADAMENTE. LA TABLA Y DIST()]



BIBLIOGRAFÍA
https://pdfminersix.readthedocs.io/en/latest/topic/converting_pdf_to_text.html