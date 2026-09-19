# README — Ejecución del agente `compira-hu-analyst` en Kiro

Este documento describe cómo ejecutar por primera vez el agente de Kiro encargado de analizar el proyecto **COMPIRA**, construir el inventario funcional y generar las Historias de Usuario de forma controlada.

> Este README inicia desde el punto en el que la estructura del agente, los archivos de `steering` y la carpeta de Historias de Usuario ya fueron creados.

---

# 1. Prerrequisitos

Antes de ejecutar el agente por primera vez, verifica que exista esta estructura:

```text
COMPIRA/
│
├── .kiro/
│   ├── agents/
│   │   └── compira-hu-analyst.md
│   │
│   └── steering/
│       ├── hu-rules.md
│       ├── compira-context.md
│       └── hu-template.md
│
├── docs/
│   ├── proyecto/
│   │   └── COMPIRA_TDG_Desarrollo_SW_InformeTecnico_V1.docx
│   │
│   ├── referencia/
│   │   └── RQ02v2-Historia-Usuario.pdf
│   │
│   └── historias-usuario/
│       └── HU/
```

Los siguientes archivos deben estar completos antes de iniciar:

- `.kiro/agents/compira-hu-analyst.md`
- `.kiro/steering/compira-context.md`
- `.kiro/steering/hu-rules.md`
- `.kiro/steering/hu-template.md`

---

# 2. Primera ejecución del agente

Abre el proyecto de COMPIRA en Kiro.

Selecciona el agente:

```text
compira-hu-analyst
```

En la **primera ejecución no debes pedirle que genere todas las Historias de Usuario**.

Primero debe analizar el proyecto completo y construir el mapa funcional.

Usa exactamente este prompt:

```text
Analiza completamente el contexto funcional de COMPIRA.

NO generes todavía las Historias de Usuario completas.

Quiero que primero realices el análisis funcional global.

Genera:

1. mapa de actores;
2. mapa de módulos;
3. capacidades funcionales por módulo;
4. funcionalidades identificadas;
5. reglas transversales conocidas;
6. dependencias funcionales;
7. contradicciones encontradas;
8. información faltante;
9. propuesta inicial de inventario de Historias de Usuario;
10. matriz de cobertura objetivo → módulo → capacidad → funcionalidad → HU.

Genera los resultados en:

docs/historias-usuario/00-analisis-funcional.md
docs/historias-usuario/01-inventario-hu.md
docs/historias-usuario/02-matriz-cobertura.md
docs/historias-usuario/PENDIENTES.md

NO desarrolles todavía el contenido completo de las HU.

No inventes comportamientos.

Si una decisión no aparece en las fuentes, regístrala como pendiente.
```

---

# 3. Resultado esperado de la primera ejecución

Después de ejecutar el prompt anterior, el agente debe crear o actualizar estos archivos:

```text
docs/historias-usuario/
│
├── 00-analisis-funcional.md
├── 01-inventario-hu.md
├── 02-matriz-cobertura.md
├── PENDIENTES.md
└── HU/
```

## 3.1 `00-analisis-funcional.md`

Debe contener como mínimo:

- objetivo general;
- objetivos específicos;
- problema funcional;
- actores;
- módulos;
- capacidades;
- funcionalidades;
- reglas transversales;
- dependencias;
- restricciones;
- alcance;
- fuera de alcance;
- contradicciones;
- vacíos de información.

## 3.2 `01-inventario-hu.md`

Debe contener una propuesta inicial del backlog:

```markdown
| ID | Módulo | Nombre | Actor | Dependencias | Prioridad | Estado |
|---|---|---|---|---|---|---|
| HU-001 | M1 | ... | ... | ... | ... | Borrador |
```

En esta fase las HU **no deben estar desarrolladas todavía**.

## 3.3 `02-matriz-cobertura.md`

Debe relacionar el alcance con las HU propuestas:

```markdown
| Objetivo | Módulo | Capacidad | Funcionalidad | HU |
|---|---|---|---|---|
```

La finalidad es poder verificar que:

```text
Objetivo
   ↓
Módulo
   ↓
Capacidad
   ↓
Funcionalidad
   ↓
Historia de Usuario
```

## 3.4 `PENDIENTES.md`

Debe centralizar toda la información que no puede determinarse con las fuentes.

Ejemplo:

```markdown
# Pendientes funcionales

## BLOQ-001

**HU relacionadas:** HU-004, HU-005

**Pregunta:**
¿Puede una tarea tener múltiples responsables o únicamente uno?

**Impacto:**
- Backend
- Frontend
- QA
- modelo de datos

**Estado:** Pendiente
```

---

# 4. Revisar manualmente el análisis inicial

Antes de permitir que el agente genere las HU completas, revisa:

```text
00-analisis-funcional.md
01-inventario-hu.md
02-matriz-cobertura.md
PENDIENTES.md
```

No continúes automáticamente.

Debes validar especialmente:

- que no haya funcionalidades inventadas;
- que todos los módulos correspondan al alcance real;
- que los actores sean correctos;
- que no existan HU técnicas;
- que no haya duplicados;
- que las HU propuestas tengan sentido funcional;
- que el número de HU sea razonable;
- que las preguntas pendientes sean reales;
- que el agente no haya convertido recomendaciones en requerimientos.

---

# 5. Segunda ejecución — revisión de granularidad

Una vez tengas el inventario inicial, pídele al agente que revise si las HU están correctamente divididas.

Usa:

```text
Revisa el inventario propuesto.

Quiero un backlog realizable para un equipo de 2 personas y el alcance académico definido.

Detecta:

- HU demasiado pequeñas;
- HU técnicas que deberían formar parte de otra HU;
- HU demasiado grandes;
- funcionalidades duplicadas;
- funcionalidades faltantes;
- dependencias innecesarias;
- funcionalidades fuera de alcance.

NO elimines ninguna HU automáticamente.

Genera una propuesta de consolidación indicando:

HU actual
→ problema
→ propuesta
→ justificación

No apliques los cambios hasta que los revise.
```

---

# 6. Qué revisar en la propuesta de granularidad

La revisión debe impedir casos como:

```text
HU-001 Crear botón de guardar
HU-002 Crear endpoint de tareas
HU-003 Crear tabla de tareas
HU-004 Crear repositorio
```

Eso no representa capacidades funcionales.

Una HU debería ser más cercana a:

```text
HU-XXX Crear y asignar tarea
```

y dentro de esa HU pueden existir:

```text
Frontend
Backend
Persistencia
Validaciones
Permisos
Criterios de aceptación
```

Una nueva HU debe existir cuando represente una capacidad funcional suficientemente independiente.

---

# 7. Criterios para aceptar una HU como independiente

Antes de aprobar el inventario, verifica si cada HU:

- entrega valor funcional;
- puede probarse de manera independiente;
- tiene reglas propias;
- tiene criterios propios;
- tiene un flujo identificable;
- puede implementarse razonablemente como una unidad.

Evita tanto:

```text
fragmentación excesiva
```

como:

```text
HU gigantes que contengan medio sistema
```

---

# 8. Aprobar el inventario

Después de revisar la propuesta, ajusta el inventario manualmente o dile al agente exactamente qué cambios debe aplicar.

Ejemplo:

```text
Apruebo la consolidación propuesta con estos cambios:

- Mantener HU-001.
- Unificar HU-004 y HU-005.
- Separar la gestión de usuarios de la gestión de roles.
- No incluir la funcionalidad X porque está fuera del alcance.

Actualiza:

docs/historias-usuario/01-inventario-hu.md
docs/historias-usuario/02-matriz-cobertura.md
docs/historias-usuario/PENDIENTES.md

No generes todavía las HU completas.
```

Revisa nuevamente los archivos.

---

# 9. Generar las Historias de Usuario

Solo cuando el inventario esté aprobado ejecuta:

```text
El inventario está aprobado.

Genera las Historias de Usuario una por una siguiendo:

.kiro/steering/hu-template.md
.kiro/steering/hu-rules.md

Guarda cada HU en:

docs/historias-usuario/HU/

Formato del nombre:

HU-001-nombre-funcional.md

Después de generar cada HU:

1. valida su Definition of Ready;
2. actualiza 01-inventario-hu.md;
3. actualiza 02-matriz-cobertura.md;
4. registra nuevas preguntas en PENDIENTES.md;
5. comprueba contradicciones con las HU previamente generadas.

No inventes reglas para completar campos vacíos.
```

---

# 10. Recomendación: generar las HU por bloques

Aunque el agente pueda generar todas las HU de una vez, es recomendable trabajar por módulo.

Por ejemplo:

```text
Genera únicamente las Historias de Usuario correspondientes al módulo M1 — Autenticación y gestión de roles.

No generes todavía HU de M2 a M5.

Al finalizar:

1. valida Definition of Ready;
2. revisa contradicciones;
3. actualiza inventario;
4. actualiza matriz de cobertura;
5. actualiza pendientes.
```

Luego:

```text
Genera las HU correspondientes al módulo M2 — Gestión de tareas.
```

Y así sucesivamente.

Esto facilita revisar la calidad antes de que el agente genere todo el backlog.

---

# 11. Flujo recomendado de módulos

El orden recomendado es:

```text
M1 — Autenticación y gestión de roles
        ↓
M2 — Gestión de tareas
        ↓
M3 — Recordatorios automáticos
        ↓
M4 — Panel de seguimiento
        ↓
M5 — Administración del sistema
```

Este orden ayuda a que las HU posteriores puedan referenciar dependencias previamente definidas.

---

# 12. Revisar cada HU

Después de generar una HU, revisa que incluya como mínimo:

```text
Identificador
Nombre
Módulo
Actor
Estado
Dependencias
Descripción funcional
Campos
Validaciones
Reglas de negocio
Reglas de comportamiento
Criterios de aceptación
Escenarios alternativos
Requisitos no funcionales cuando apliquen
Aclaraciones
Fuera de alcance
Preguntas pendientes
Prototipo
Definition of Ready
```

---

# 13. Qué no debe hacer el agente

El agente no debe:

- inventar reglas;
- inventar campos;
- inventar validaciones;
- inventar mensajes;
- inventar códigos HTTP;
- crear funcionalidades únicamente porque "serían útiles";
- asumir comportamiento estándar;
- ampliar el MVP;
- modificar código;
- implementar backend;
- implementar frontend;
- diseñar base de datos sin solicitud;
- convertir recomendaciones en requisitos.

Cuando algo no esté definido debe usar:

```text
Pendiente por definir
```

o:

```text
Requiere confirmación
```

---

# 14. Resolver preguntas pendientes

Cuando tengas respuesta para un pendiente, no edites únicamente `PENDIENTES.md`.

Dile al agente:

```text
Se resuelve el pendiente BLOQ-003 de la siguiente manera:

[respuesta]

Actualiza toda la documentación afectada.

Debes:

1. actualizar las HU relacionadas;
2. eliminar el pendiente resuelto;
3. actualizar criterios afectados;
4. actualizar reglas afectadas;
5. actualizar la matriz de cobertura si corresponde;
6. revisar dependencias;
7. volver a validar Definition of Ready.

No agregues simplemente una nota al final de la HU.
```

---

# 15. Gestionar una nueva decisión funcional

Si durante una reunión se define una nueva regla:

```text
Nueva decisión funcional:

[descripción]

Analiza qué HU están afectadas.

Antes de modificar archivos indícame:

- HU afectadas;
- reglas afectadas;
- criterios afectados;
- impacto de alcance;
- posibles contradicciones.

No apliques cambios todavía.
```

Después de revisar el análisis:

```text
Apruebo el cambio.

Actualiza toda la documentación relacionada y registra el cambio en la trazabilidad.
```

---

# 16. Gestionar cambios de alcance

Si aparece una nueva funcionalidad:

```text
Se solicita la siguiente funcionalidad:

[funcionalidad]

Analiza si:

- ya está cubierta;
- es una aclaración;
- modifica una regla;
- agrega una capacidad nueva;
- amplía el alcance;
- está fuera del alcance actual.

No crees una HU todavía.

Primero presenta el análisis de impacto.
```

El agente debe poder clasificarla como:

```text
Corrección
Aclaración
Nuevo detalle
Cambio de regla
Nueva capacidad
Ampliación de alcance
Posible control de cambio
```

---

# 17. Validación global del backlog

Cuando todas las HU estén generadas, ejecuta:

```text
Realiza una auditoría completa del backlog de COMPIRA.

Revisa todas las Historias de Usuario existentes.

Valida:

1. cobertura completa del alcance;
2. trazabilidad objetivo → módulo → funcionalidad → HU;
3. HU duplicadas;
4. contradicciones;
5. reglas inconsistentes;
6. campos con nombres diferentes para el mismo concepto;
7. estados inconsistentes;
8. dependencias rotas;
9. funcionalidades sin HU;
10. HU que no correspondan a ningún objetivo;
11. preguntas bloqueantes;
12. HU que no cumplan Definition of Ready.

No modifiques archivos todavía.

Genera primero un informe de auditoría.
```

---

# 18. Aplicar las correcciones de auditoría

Después de revisar el informe:

```text
Apruebo las siguientes correcciones:

[listado]

Aplica únicamente esas correcciones.

Después:

1. actualiza todas las HU afectadas;
2. actualiza inventario;
3. actualiza matriz de cobertura;
4. actualiza pendientes;
5. vuelve a ejecutar la validación de consistencia.
```

---

# 19. Preparar HU para Sprint Planning

Cuando quieras llevar HU al sprint:

```text
Analiza las HU candidatas para el próximo sprint.

Identifica cuáles cumplen Definition of Ready.

Genera una tabla:

| HU | Nombre | Módulo | Dependencias | DoR | Bloqueantes |

No asignes Story Points.

No decidas automáticamente cuáles deben entrar al sprint.

Solo presenta información para que el equipo tome la decisión.
```

---

# 20. Después de definir Story Points

Los Story Points deben ser acordados por el equipo.

Una vez definidos puedes decir:

```text
Actualiza el Product Backlog con los siguientes Story Points:

HU-001: 3
HU-002: 5
HU-003: 8

No modifiques ninguna otra información funcional.
```

---

# 21. Después de desarrollar una HU

Cuando una HU haya sido implementada:

```text
La HU-XXX fue desarrollada.

Revisa su documentación y genera una lista de evidencias necesarias para considerarla terminada.

Incluye cuando aplique:

- pruebas funcionales;
- escenarios exitosos;
- escenarios de error;
- curls;
- Swagger/OpenAPI;
- diagrama del flujo implementado;
- evidencia frontend;
- logs;
- auditoría;
- validación de criterios de aceptación.

No marques la HU como completada automáticamente.
```

---

# 22. Marcar una HU como completada

Solo después de verificar evidencia:

```text
La HU-XXX fue validada y cumple todos los criterios.

Actualiza su estado a:

Completada

Actualiza también:

- inventario;
- matriz de cobertura;
- backlog;
- trazabilidad.

No modifiques otras HU.
```

---

# 23. Comando de revisión rápida de una HU

Si solo quieres revisar una HU:

```text
Revisa HU-XXX.

Evalúa:

- descripción;
- campos;
- validaciones;
- reglas;
- criterios;
- errores;
- dependencias;
- alcance;
- Definition of Ready.

No modifiques el archivo.

Primero presenta los hallazgos.
```

---

# 24. Comando para actualizar una HU

Después:

```text
Apruebo las recomendaciones:

[listado]

Actualiza HU-XXX.

Después revisa:

- HU dependientes;
- inventario;
- matriz;
- pendientes.

No modifiques elementos no afectados.
```

---

# 25. Flujo completo recomendado

El proceso completo debe verse así:

```text
Documentación del proyecto
          ↓
Contexto funcional
          ↓
Análisis global
          ↓
Mapa de actores
          ↓
Mapa de módulos
          ↓
Capacidades
          ↓
Funcionalidades
          ↓
Inventario de HU
          ↓
Revisión de granularidad
          ↓
Aprobación humana
          ↓
Generación de HU
          ↓
Preguntas pendientes
          ↓
Resolución de pendientes
          ↓
Definition of Ready
          ↓
Sprint Planning
          ↓
Desarrollo
          ↓
Pruebas
          ↓
Evidencias
          ↓
Definition of Done
          ↓
HU completada
```

---

# 26. Regla operativa principal

Nunca uses:

```text
Genera todas las HU que consideres necesarias e impleméntalas.
```

Usa siempre el flujo controlado:

```text
Analizar
↓
Proponer
↓
Revisar
↓
Aprobar
↓
Generar
↓
Validar
```

La decisión final sobre alcance siempre debe permanecer en el equipo.

---

# 27. Comando recomendado para retomar el trabajo otro día

Cuando vuelvas a abrir Kiro:

```text
Revisa el estado actual de la documentación de Historias de Usuario de COMPIRA.

Lee:

docs/historias-usuario/00-analisis-funcional.md
docs/historias-usuario/01-inventario-hu.md
docs/historias-usuario/02-matriz-cobertura.md
docs/historias-usuario/PENDIENTES.md
docs/historias-usuario/HU/

Indícame:

1. HU existentes;
2. estado de cada una;
3. HU con pendientes;
4. pendientes bloqueantes;
5. módulos incompletos;
6. siguiente actividad recomendada dentro del proceso documental.

No modifiques ningún archivo hasta que te lo solicite.
```

---

# 28. Regla para evitar pérdida de contexto

Cada vez que se tome una decisión importante, esta debe quedar incorporada en alguno de estos lugares:

```text
HU correspondiente
PENDIENTES.md
compira-context.md
hu-rules.md
```

No depender únicamente del historial del chat del agente.

La documentación del repositorio debe ser la fuente persistente de verdad.

---

# 29. Resultado final esperado

Al finalizar el proceso, la estructura debería verse aproximadamente así:

```text
docs/
└── historias-usuario/
    ├── 00-analisis-funcional.md
    ├── 01-inventario-hu.md
    ├── 02-matriz-cobertura.md
    ├── PENDIENTES.md
    │
    └── HU/
        ├── HU-001-....md
        ├── HU-002-....md
        ├── HU-003-....md
        ├── HU-004-....md
        └── ...
```

Cada HU debe estar trazada contra el alcance del proyecto y debe poder ser comprendida por:

- Product Owner;
- desarrollo;
- frontend;
- backend;
- QA;
- UX/UI;
- arquitectura;
- asesor del proyecto.

---

# 30. Checklist antes de iniciar desarrollo

Antes de pasar una HU a desarrollo:

```text
[ ] Está en el inventario.
[ ] Está relacionada con una funcionalidad del alcance.
[ ] Tiene actor.
[ ] Tiene objetivo.
[ ] Tiene campos definidos.
[ ] Tiene validaciones.
[ ] Tiene reglas de negocio.
[ ] Tiene criterios de aceptación verificables.
[ ] Tiene escenarios de error relevantes.
[ ] Tiene dependencias identificadas.
[ ] No contiene supuestos críticos.
[ ] No tiene preguntas bloqueantes.
[ ] Cumple Definition of Ready.
[ ] Fue revisada por el equipo.
```

Si cualquiera de los puntos críticos falla, la HU debe continuar en refinamiento.

---

# Resumen operativo

Para comenzar por primera vez:

```text
1. Seleccionar compira-hu-analyst.
2. Ejecutar análisis global.
3. Revisar 00-analisis-funcional.md.
4. Revisar 01-inventario-hu.md.
5. Revisar 02-matriz-cobertura.md.
6. Revisar PENDIENTES.md.
7. Ejecutar revisión de granularidad.
8. Aprobar inventario.
9. Generar HU por módulo.
10. Resolver pendientes.
11. Validar Definition of Ready.
12. Llevar HU aprobadas al Sprint Planning.
```

No saltar directamente del documento del proyecto a desarrollo.
