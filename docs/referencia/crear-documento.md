# OBJETIVO

Genera nuevamente el documento consolidado de Historias de Usuario de **COMPIRA**, pero esta vez debes reproducir con la **máxima fidelidad visual posible** la plantilla del siguiente PDF:

`/Users/andresganan/Desktop/COMPIRA/agent-hu/docs/referencia/RQ02v2 Historia de Usuario proyecto_DIAMESAÑO_V1.pdf`

El documento generado anteriormente **NO es una referencia válida de formato**.

El PDF indicado arriba es la **ÚNICA FUENTE DE VERDAD PARA EL FORMATO VISUAL**.

El documento anterior puede utilizarse únicamente como fuente del contenido de las HU, nunca como referencia de diseño.

---

# REGLA PRINCIPAL

## NO REDISEÑAR EL DOCUMENTO

No debes interpretar, mejorar, modernizar, simplificar ni adaptar libremente el diseño.

Debes **REPLICAR la plantilla del PDF original**.

Piensa en esta tarea como:

> Tomar el documento PDF original como una plantilla física y reemplazar únicamente su contenido por las HU de COMPIRA.

No como:

> Crear un documento nuevo inspirado en el PDF.

La diferencia es fundamental.

---

# 1. ANÁLISIS OBLIGATORIO DEL PDF ANTES DE GENERAR EL DOCUMENTO

Antes de escribir una sola HU debes inspeccionar **visualmente todas las páginas del PDF de referencia**.

No es suficiente extraer el texto.

Debes analizar la representación visual de las páginas para identificar:

* tamaño de página;
* orientación;
* márgenes;
* encabezados;
* líneas horizontales;
* colores exactos o visualmente equivalentes;
* tipografías;
* tamaños de fuente;
* negritas;
* espaciado;
* interlineado;
* tablas;
* anchos de columnas;
* alturas de filas;
* bordes;
* grosor de bordes;
* color de fondo de las celdas;
* alineación vertical;
* alineación horizontal;
* sangrías;
* viñetas;
* espacios entre historias;
* comportamiento de las HU cuando continúan en otra página;
* sección de prototipos;
* dimensiones del área destinada a los prototipos.

No generes el documento hasta terminar este análisis.

---

# 2. LA PLANTILLA DEL PDF DEBE RECONSTRUIRSE, NO APROXIMARSE

La estructura de una Historia de Usuario debe reproducirse como aparece en el PDF.

Especialmente:

### Cabecera de HU

Debe existir una tabla cuya primera fila conserve la estructura:

| Identificador de la historia | HUXX | Nombre de la historia | Nombre correspondiente |

Respeta:

* número de columnas;
* proporción entre columnas;
* bordes;
* colores;
* negritas;
* alineación;
* altura;
* distribución.

NO reemplazar esta estructura por títulos independientes.

---

# 3. CUERPO DE LA HISTORIA DE USUARIO

La frase:

**Como [rol] necesito [necesidad], para [objetivo].**

debe encontrarse dentro de la tabla, exactamente siguiendo el patrón visual del PDF original.

Después deben aparecer, manteniendo el formato original:

**Campos**

y posteriormente:

**Criterio de aceptación**

Respeta exactamente:

* color de fondo azul claro;
* bordes;
* sangría;
* viñetas;
* negritas;
* espaciado;
* ancho;
* alineación;
* estructura de las celdas.

No conviertas estas secciones en párrafos independientes fuera de la tabla.

---

# 4. ACLARACIONES

Cuando una HU tenga una sección de aclaración, debe ubicarse dentro del mismo bloque visual de la HU y respetar la forma en que el documento de referencia maneja este contenido.

No crear estilos nuevos para:

* Aclaraciones;
* notas técnicas;
* fuera de alcance;
* pendientes.

Deben integrarse al diseño existente.

---

# 5. PROTOTIPOS — REQUISITO CRÍTICO

Este punto es **OBLIGATORIO**.

Los prototipos se encuentran en:

`/Users/andresganan/Desktop/COMPIRA/agent-hu/docs/historias-usuario/prototipos`

Debes inspeccionar esa carpeta y localizar el prototipo correspondiente a cada HU.

Los archivos pueden estar en formatos como:

* `.svg`
* `.png`
* `.jpg`
* `.jpeg`
* u otros formatos gráficos disponibles.

## Está PROHIBIDO escribir únicamente la ruta del prototipo.

Por ejemplo, esto es INCORRECTO:

`Wireframe funcional: docs/historias-usuario/prototipos/HU-01-nueva-contrasena.svg`

También es incorrecto:

`Prototipo disponible en HU-01-nueva-contrasena.svg`

También es incorrecto colocar un hipervínculo.

### LO CORRECTO ES INSERTAR VISUALMENTE LA IMAGEN DEL PROTOTIPO.

La sección:

**Prototipo**

debe contener la **imagen real renderizada del prototipo**.

El lector debe poder ver el prototipo directamente dentro del documento sin abrir archivos externos.

---

# 6. TRATAMIENTO DE SVG

Si el prototipo está en SVG y el mecanismo utilizado para generar DOCX no permite insertarlo correctamente:

1. Renderiza el SVG.
2. Conviértelo temporalmente a PNG de alta resolución.
3. Conserva su proporción.
4. Inserta el PNG resultante en el documento.

La conversión es únicamente un mecanismo técnico.

No modifiques visualmente el prototipo.

No agregues marcos adicionales.

No rediseñes el SVG.

No reconstruyas manualmente el prototipo si puedes renderizar el original.

---

# 7. DIMENSIONAMIENTO DE LOS PROTOTIPOS

La imagen debe escalarse proporcionalmente para ocupar adecuadamente el área de prototipo.

Debe:

* conservar su relación de aspecto;
* quedar centrada;
* ser legible;
* no deformarse;
* no exceder los bordes de la tabla;
* no quedar cortada;
* no superponerse con otros elementos.

Utiliza el **ancho disponible de la sección Prototipo** como referencia principal.

Si el prototipo es muy alto, adapta su tamaño manteniendo la proporción.

Si no cabe razonablemente en el espacio restante de la página, permite que la HU continúe de acuerdo con el comportamiento de la plantilla.

**Nunca reduzcas un prototipo hasta hacerlo ilegible solamente para obligarlo a caber en una página.**

---

# 8. PROTOTIPO COMO PARTE DE LA TABLA

La sección de prototipo NO debe quedar como texto independiente.

Debe existir una fila:

**Prototipo**

con el mismo estilo visual de la plantilla.

Debajo debe existir una celda combinada que ocupe todo el ancho de la HU.

Dentro de esa celda debe insertarse la imagen correspondiente.

Conceptualmente:

| Prototipo |
| --------- |
|           |
| IMAGEN    |
| REAL      |
|           |

Pero utilizando exactamente el estilo del PDF de referencia.

---

# 9. SI UNA HU NO TIENE PROTOTIPO

Primero busca exhaustivamente en:

`/Users/andresganan/Desktop/COMPIRA/agent-hu/docs/historias-usuario/prototipos`

Intenta relacionar los archivos por:

1. identificador de HU;
2. nombre de HU;
3. nombres similares;
4. variantes razonables del nombre.

Si definitivamente no existe un prototipo correspondiente:

* conserva la sección "Prototipo";
* deja el espacio correspondiente vacío.

**NO inventes una imagen.**

**NO escribas una ruta.**

**NO escribas "prototipo pendiente".**

**NO escribas "placeholder".**

---

# 10. NO ESCRIBIR "PLACEHOLDER"

El documento generado anteriormente contiene referencias como:

`Wireframe funcional (placeholder): ...`

Esto queda explícitamente prohibido.

La palabra:

`placeholder`

no debe aparecer en el documento final asociada a los prototipos.

Si existe una imagen, se inserta.

Si no existe, se deja el espacio vacío siguiendo la plantilla.

---

# 11. CONTINUIDAD ENTRE PÁGINAS

El PDF de referencia permite que una HU continúe en la página siguiente.

Debes reproducir este comportamiento.

NO debes forzar artificialmente una HU completa dentro de una sola página si esto:

* reduce demasiado la fuente;
* comprime el contenido;
* deforma la tabla;
* reduce excesivamente el prototipo;
* genera espacios extraños.

Cuando una HU continúe en otra página:

* conserva los bordes;
* conserva el ancho;
* conserva el fondo;
* conserva la continuidad visual de la tabla.

---

# 12. ENCABEZADO DE LAS PÁGINAS

Replica el encabezado:

**Historia de Usuarios**

y la línea horizontal azul que aparece debajo.

Debe mantenerse en las páginas siguiendo el mismo patrón del documento original.

No reemplazarlo por un encabezado genérico de Word.

---

# 13. PORTADA

La portada debe mantener exactamente la composición visual de la plantilla.

Únicamente cambia la información necesaria para COMPIRA.

Debe conservar:

* posiciones;
* jerarquías;
* tamaños;
* alineaciones;
* colores;
* espacios.

No diseñes una portada nueva.

---

# 14. TABLA DE CONTENIDO

Conserva la estructura visual de la tabla de contenido del documento original.

Debe contener:

1. Control de cambios
2. Contexto del sistema
   2.1. Descripción del proyecto
3. Historias de usuario
4. Glosario de términos
5. Riesgos preliminares

Actualiza únicamente la numeración de páginas cuando corresponda.

---

# 15. CONTROL DE CAMBIOS

Replica exactamente la tabla visual del PDF:

| Fecha | Versión | Descripción | Autor |

Con los mismos:

* colores;
* bordes;
* proporciones;
* encabezados;
* alineaciones.

---

# 16. CONTEXTO DEL SISTEMA

Mantén:

**2. Contexto del sistema**

**2.1. Descripción del proyecto**

y la estructura visual utilizada por la plantilla.

Si existe un diagrama de contextualización construido para COMPIRA, insértalo visualmente.

No reemplaces una imagen disponible por una referencia textual a su ruta.

---

# 17. CONTENIDO DE LAS HU

Debes incluir **todas las HU construidas para COMPIRA**.

No debes:

* inventar requerimientos;
* eliminar criterios;
* resumir arbitrariamente;
* cambiar decisiones funcionales;
* agregar funcionalidades;
* cambiar el significado de las HU.

Puedes corregir exclusivamente errores evidentes de:

* ortografía;
* puntuación;
* consistencia de redacción.

---

# 18. NO CONFUNDIR CONTENIDO CON FORMATO

Existen dos fuentes diferentes:

## Fuente del FORMATO

`/Users/andresganan/Desktop/COMPIRA/agent-hu/docs/referencia/RQ02v2 Historia de Usuario proyecto_DIAMESAÑO_V1.pdf`

Esta fuente determina:

* diseño;
* colores;
* tablas;
* estructura;
* distribución;
* estilos;
* apariencia.

## Fuente de los PROTOTIPOS

`/Users/andresganan/Desktop/COMPIRA/agent-hu/docs/historias-usuario/prototipos`

Esta fuente determina las imágenes que deben aparecer dentro de cada sección "Prototipo".

## Fuente del CONTENIDO

Las Historias de Usuario construidas previamente para COMPIRA.

No mezclar las responsabilidades de estas fuentes.

---

# 19. VALIDACIÓN VISUAL OBLIGATORIA

Después de generar el DOCX, **NO lo entregues inmediatamente**.

Primero debes renderizarlo visualmente.

Luego compara páginas del documento generado contra las páginas equivalentes del PDF original.

Debes revisar visualmente:

### Tablas

* ancho;
* posición;
* bordes;
* colores;
* celdas;
* combinaciones;
* alturas.

### Tipografía

* familia;
* tamaño;
* negrita;
* alineación;
* interlineado.

### Página

* márgenes;
* encabezado;
* línea azul;
* espacios;
* saltos.

### Prototipos

Verifica que cada sección "Prototipo":

* tenga una imagen cuando exista;
* muestre realmente el contenido de la imagen;
* esté centrada;
* sea legible;
* mantenga proporciones;
* no esté deformada;
* no esté cortada;
* no salga de la tabla.

---

# 20. SEGUNDA ITERACIÓN OBLIGATORIA

Si la primera generación presenta diferencias visuales evidentes respecto al PDF:

**NO entregues todavía el archivo.**

Corrige el DOCX y vuelve a renderizarlo.

Realiza al menos una comparación visual después de la generación y aplica las correcciones necesarias.

El archivo se considera terminado únicamente cuando la estructura visual sea razonablemente equivalente a la plantilla.

---

# 21. VALIDACIÓN AUTOMÁTICA FINAL

Antes de entregar, comprueba además que:

* [ ] están todas las HU;
* [ ] no existen HU duplicadas;
* [ ] cada HU tiene identificador;
* [ ] cada HU tiene nombre;
* [ ] cada HU conserva "Campos";
* [ ] cada HU conserva "Criterio de aceptación";
* [ ] cada HU conserva "Prototipo";
* [ ] los prototipos existentes están incrustados como imágenes;
* [ ] ninguna ruta local aparece como sustituto de una imagen;
* [ ] no aparece `Wireframe funcional:` como sustituto del prototipo;
* [ ] no aparece `(placeholder)` como sustituto del prototipo;
* [ ] las imágenes mantienen su relación de aspecto;
* [ ] ninguna imagen sale de los márgenes;
* [ ] ninguna tabla sale de los márgenes;
* [ ] no existen textos superpuestos;
* [ ] no existen páginas vacías accidentales;
* [ ] no existen filas partidas de manera visualmente incorrecta;
* [ ] los encabezados son consistentes;
* [ ] los colores corresponden a la plantilla;
* [ ] la portada corresponde a la plantilla;
* [ ] la tabla de contenido corresponde a la plantilla;
* [ ] el glosario mantiene la estructura de la plantilla;
* [ ] los riesgos mantienen la estructura de la plantilla.

---

# 22. REGLA DE FIDELIDAD

Ante cualquier duda entre:

**A. Crear algo visualmente más bonito**

o

**B. Reproducir exactamente lo que hace el PDF**

elige siempre:

**B. Reproducir el PDF.**

La fidelidad a la plantilla tiene prioridad sobre cualquier criterio estético propio.

---

# 23. ENTREGABLE

Genera únicamente:

`RQ02_Historias_Usuario_COMPIRA_Final.docx`

El DOCX debe contener las imágenes de los prototipos **embebidas físicamente dentro del archivo**.

No deben depender de rutas locales para visualizarse.

Esto es importante: si el archivo se abre en otro computador, **todos los prototipos deben seguir siendo visibles**.

No entregues:

* Markdown;
* HTML;
* JSON;
* archivos temporales;
* imágenes convertidas;
* explicaciones;
* reportes de ejecución.

Solo entrega el `.docx` final.

---

# CRITERIO DE ACEPTACIÓN FINAL

Abre simultáneamente:

1. El PDF original de referencia.
2. El DOCX generado.

Al comparar ambos visualmente, debe percibirse que pertenecen a **la misma plantilla documental**.

Las diferencias deben corresponder principalmente al contenido de COMPIRA y a sus prototipos, no a cambios de diseño.

Especialmente, una HU debe verse conceptualmente como en el PDF original:

**[Identificador | HU | Nombre de la historia | Nombre]**

**[Como... necesito... para...]**

**[Campos]**

**[Criterio de aceptación]**

**[Prototipo]**

**[IMAGEN REAL DEL PROTOTIPO]**

y NO como texto corrido seguido de una ruta hacia un archivo.

## INSTRUCCIÓN FINAL

No me expliques qué vas a hacer.

No me entregues un resumen.

No me preguntes si deseas continuar.

Analiza los archivos, genera el documento, realiza la comparación visual, corrige las diferencias detectadas y entrega **únicamente el DOCX final**.
