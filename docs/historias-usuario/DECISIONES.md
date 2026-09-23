# Registro de decisiones funcionales — COMPIRA

Este archivo registra **decisiones funcionales confirmadas** por el Product Owner
o el equipo. Es distinto de `PENDIENTES.md` (lo que todavía no sabemos).

Reglas:

- No inventar el motivo si no fue proporcionado (usar `Pendiente por definir`).
- Cuando se resuelva un pendiente que constituya una decisión funcional
  relevante, registrarla aquí y actualizar los artefactos afectados (HU,
  inventario, matriz de cobertura, glosario) en el mismo paso.
- Los identificadores `DEC-XXX` son inmutables: no se renumeran ni se reutilizan.
- El agente NO crea decisiones por su cuenta: solo registra las que el equipo
  aprueba explícitamente.

---

## Plantilla

```
## DEC-XXX

Fecha: YYYY-MM-DD

Decisión:
[decisión]

Origen:
[Product Owner / equipo / documento / reunión]

Motivo:
[si fue proporcionado, si no: Pendiente por definir]

HU afectadas:
- HU-XXX

Módulos afectados:
- Mx

Estado:
Vigente
```

---

## DEC-001

Fecha: 2026-09-20

Decisión:
Una tarea (M2) tiene **un único responsable** (Colaborador) en cada momento,
no varios responsables simultáneos. El cambio de responsable se realiza
mediante la operación de **reasignación** (ya documentada como capacidad
propia y distinta de "asignación" en M2). El historial de la tarea (RT-05)
debe conservar el registro de cada responsable anterior y de lo que realizó
antes de que la tarea fuera reasignada.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, tras análisis
del agente `compira-hu-analyst` sobre la tensión entre el texto documentado y
el razonamiento del equipo. Registrada previamente como `BLOQ-005` en
`PENDIENTES.md`.

**Nota de divergencia respecto al documento oficial:** el documento oficial
(`docs/proyecto/COMPIRA_TDG_Desarrollo_SW_InformeTecnico_V1.docx`, resumido en
`.kiro/steering/compira-context.md` secciones 4 y 6) afirma textualmente
"asignación de responsables (uno o varios colaboradores)" en dos lugares. Esta
decisión **reduce el alcance** respecto a ese texto de forma consciente y
explícita; no es una corrección de un vacío, sino un cambio de regla de
negocio decidido por el equipo. Se recomienda reflejar este cambio en el
documento oficial del proyecto cuando corresponda (fuera del alcance de
escritura de este agente, ver `agent/compira-hu-analyst.prompt.md` —
"Límites de escritura").

Motivo:
Reduce la complejidad del modelo de datos (responsable único con historial,
en vez de una relación N:M tarea-responsables); es consistente con
"reasignar tareas" como capacidad ya documentada de forma separada de
"asignar tareas"; y refuerza la trazabilidad exigida por RT-05 mediante un
historial claro de responsables en vez de un estado agregado entre varios
responsables simultáneos.

HU afectadas:
- HU-13 — pasa de "Asignar tarea a uno o varios colaboradores" a "Asignar
  tarea a un responsable".
- HU-15 — ya no requiere modelar estado compartido/múltiple; el estado es del
  responsable único vigente.
- HU-17 — se convierte en la HU central del cambio de responsable; debe
  documentar explícitamente el registro en histórico de lo realizado por el
  responsable anterior.

Módulos afectados:
- M2 (Gestión de tareas)

Pendientes resueltos por esta decisión:
- `BLOQ-005` (`PENDIENTES.md`) — resuelto, ver referencia cruzada en ese
  archivo.

Estado:
Vigente

---

## DEC-002

Fecha: 2026-09-20

Decisión:
COMPIRA es **mono-organización por despliegue** (una instancia = una pyme).
Existe una entidad `Organización`/`Empresa` configurable (nombre,
configuración general, zona horaria, notificaciones), para que el software
sea genérico y reutilizable para cualquier pyme — pero el sistema **no**
soporta multi-tenant (varias organizaciones aisladas conviviendo en la misma
instancia) dentro del alcance de este proyecto académico.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, basado en:
- (a) filosofía de producto declarada por el equipo: la plataforma debe ser
  "adaptable a una u otra pyme" según sus necesidades — se interpreta como
  producto genérico/reutilizable, no como varios clientes compartiendo una
  misma instancia.
- (b) evidencia documental: sección 15 del documento oficial (`compira-
  context.md`) menciona "pruebas de usabilidad en **pyme experimental**"
  (singular) como el único escenario de uso real descrito.
- (c) sección 7 (alcance excluido): "implementación productiva en empresas
  reales con datos en producción" está fuera de alcance — no se contempla
  operar varias pymes reales simultáneamente.
- (d) sección 5.1: el Administrador se describe como responsable de "la
  organización" (singular).

Motivo:
Consistente con el alcance académico del proyecto (un despliegue de
demostración/prueba); evita la complejidad de aislamiento multi-tenant
(`organizacion_id` en todas las tablas, seguridad entre tenants) que no está
requerida por el documento ni por los objetivos declarados; preserva la
posibilidad de reutilizar el software para distintas pymes mediante
configuración, no mediante coexistencia de varias organizaciones en una
misma instancia.

**Nota de alcance futuro:** esta decisión no descarta permanentemente el
multi-tenant; si el proyecto evoluciona más allá del alcance académico
actual, sería una `Posible ampliación de alcance — requiere validación` a
evaluar en su momento, no una decisión irreversible de arquitectura.

HU afectadas:
- HU-28 (Configurar zona horaria y notificaciones generales) — pasa a ser,
  implícitamente, la configuración de la única `Organización` de la
  instancia.

Módulos afectados:
- M1 (no requiere flujo de alta de organización — se asume una organización
  preexistente por instancia).
- M5 (el Administrador administra una sola organización, no varias).

Pendientes resueltos por esta decisión:
- `BLOQ-006` (`PENDIENTES.md`) — resuelto.

Estado:
Vigente

---

## DEC-003

Fecha: 2026-09-20

Decisión:
Se confirma la entidad `Equipo` con la siguiente cardinalidad:
- (a) un Colaborador pertenece **exactamente a un** Equipo (no puede estar en
  más de uno simultáneamente).
- (b) un Equipo tiene **exactamente un** Coordinador a la vez (no varios
  coordinadores simultáneos sobre el mismo equipo).
- (c) el Coordinador de un Equipo **puede reasignarse**, y esa reasignación
  es una acción **exclusiva del Administrador**.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, en respuesta
directa a las preguntas de cardinalidad planteadas en `BLOQ-007` tras la
recomendación técnica conjunta DBA/backend/frontend (`00-analisis-
funcional.md`, sección 14).

Motivo:
Pendiente por definir (no se proporcionó un motivo explícito más allá de la
cardinalidad en sí).

HU afectadas:
- HU-13, HU-25, HU-26, HU-27, HU-30 (inventario existente) — ya no asumen
  "equipo" implícitamente, la entidad queda confirmada con esta cardinalidad.
- Nuevas HU necesarias para el ciclo de vida de `Equipo` (crear equipo,
  asignar/reasignar coordinador, asignar colaborador a un equipo) — ver
  propuesta en `01-inventario-hu.md` (HU-31 a HU-33).

Módulos afectados:
- M2 (alcance de asignación de tareas), M4 (alcance del panel), M5 (nueva
  capacidad de administración de equipos).

Pendientes resueltos por esta decisión:
- `BLOQ-007` (`PENDIENTES.md`) — resuelto en cuanto a cardinalidad. Queda
  abierto un pendiente nuevo, `IMP-009`, sobre el detalle operativo de creación
  de equipos y asignación inicial de colaboradores (qué campos tiene un
  Equipo, quién lo crea, cuándo se asigna el colaborador a su equipo).

Estado:
Vigente

---

## DEC-004

Fecha: 2026-09-20

Decisión:
Un mismo usuario **puede tener más de un rol simultáneamente** (p. ej. ser
Coordinador de un Equipo y Colaborador en otro, u otras combinaciones entre
Administrador, Coordinador y Colaborador). El rol no es único ni excluyente
por usuario.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-007`.

Motivo:
Pendiente por definir (no se proporcionó un motivo explícito).

HU afectadas:
Ninguna HU del inventario actual tiene contenido completo que dependiera de
rol único; esta decisión afecta el diseño futuro del modelo de autorización
(M1) y de asignación de roles (M5, HU-10).

Módulos afectados:
- M1 (modelo de autorización/RBAC), M5 (asignación de roles).

Pendientes resueltos por esta decisión:
- `IMP-007` (`PENDIENTES.md`) — resuelto.

Estado:
Vigente

---

## DEC-005

Fecha: 2026-09-20

Decisión:
Se confirma el conjunto cerrado de estados de una `Tarea` (M2), como valores
excluyentes de un único campo `estado`:

`Pendiente` → `En progreso` → (`Retrasada`) → `Completada` → `Cerrada`
`En reasignación` (transversal) · `Cancelada` (terminal)

Reglas de exclusividad confirmadas:
- **`Retrasada`** reemplaza al estado anterior (`Pendiente` o `En progreso`)
  cuando la tarea supera su fecha límite; no es una marca/indicador aparte,
  es un valor más del mismo campo `estado`.
- **`Completada`** y **`Cerrada`** son **dos estados independientes**:
  `Completada` es cuando el Colaborador marca la tarea como lista;
  `Cerrada` es un estado separado, no automático.
- **`En reasignación`** es un estado nuevo (no estaba en el documento
  oficial ni en el análisis original) para representar el momento en que una
  tarea cambia de responsable (`DEC-001`).

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, en respuesta
directa a las preguntas de `BLOQ-001`.

Motivo:
Pendiente por definir (no se proporcionó un motivo explícito más allá de la
definición del conjunto en sí).

**Nota de relación con `IMP-002`:** esta decisión confirma que existe un
estado `Cerrada` separado de `Completada`, pero **no** resuelve bajo qué
condición exacta una tarea requiere la aprobación del Coordinador para pasar
de `Completada` a `Cerrada`. `IMP-002` permanece abierto para esa regla de
negocio específica.

**Nota de detalle pendiente:** la transición exacta hacia/desde
`En reasignación` (¿a qué estado vuelve la tarea tras la reasignación:
`Pendiente` o el estado que tenía antes? ¿puede cancelarse una tarea desde
cualquier estado?) no fue especificada; se registra como `IMP-010`.

HU afectadas:
- HU-15 (Actualizar estado de una tarea) — debe operar sobre este conjunto
  cerrado de 7 estados.
- HU-17 (Reasignar tarea) — dispara la transición a `En reasignación`.
- HU-19 (Aprobar / cerrar tarea) — es la HU que produce la transición de
  `Completada` a `Cerrada`.
- HU-27 (Indicadores de cumplimiento y vencidas) — usa `Retrasada` como
  estado, no como cálculo aparte.

Módulos afectados:
- M2 (modelo de datos y transiciones), M4 (indicadores basados en estos
  estados).

Pendientes resueltos por esta decisión:
- `BLOQ-001` (`PENDIENTES.md`) — resuelto en cuanto al conjunto cerrado y su
  exclusividad. `IMP-002` permanece abierto (condición de aprobación); se
  agrega `IMP-010` (detalle de transiciones hacia/desde `En reasignación` y
  `Cancelada`).

Estado:
Vigente

---

## DEC-006

Fecha: 2026-09-20

Decisión:
El panel de seguimiento (M4) es **la misma vista/funcionalidad** para
Administrador y Coordinador; solo cambia el **alcance de datos**:
- Administrador: ve **todos los equipos** de la Organización (`DEC-002`,
  mono-organización).
- Coordinador: ve únicamente el/los equipo(s) donde es Coordinador vigente
  (`DEC-003`).

No existen capacidades adicionales exclusivas del Administrador en esta
pantalla (mismos filtros, mismos indicadores).

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `BLOQ-002`.

Motivo:
Pendiente por definir.

HU afectadas:
- HU-25, HU-26, HU-27 — alcance de datos ahora explícito por rol.
- HU-30 — **fusionada en HU-25** (confirmado por el equipo, 2026-09-20;
  `Estado: Fusionada en HU-25`, identificador no reutilizado).

Módulos afectados:
- M4.

Pendientes resueltos por esta decisión:
- `BLOQ-002` (`PENDIENTES.md`) — resuelto.

**Nota de granularidad aplicada:** dado que HU-25 y HU-30 son la misma
funcionalidad con alcance de datos parametrizado por rol, se fusionó HU-30
en HU-25 siguiendo la regla de granularidad (no fragmentar artificialmente
una capacidad con reglas y flujo idénticos). Confirmado explícitamente por
el equipo el 2026-09-20.

Estado:
Vigente

---

## DEC-007

Fecha: 2026-09-20

Decisión:
Toda tarea requiere **aprobación explícita del Coordinador**, sin excepción,
para pasar del estado `Completada` al estado `Cerrada` (`DEC-005`). No existe
un camino de cierre automático al completarse.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-002`.

Motivo:
Pendiente por definir.

HU afectadas:
- HU-19 (Aprobar / cerrar tarea) — deja de ser condicional ("cuando
  aplique"); es un paso obligatorio del ciclo de vida de toda tarea.
- HU-15 (Actualizar estado) — el Colaborador puede llevar la tarea hasta
  `Completada`, pero no hasta `Cerrada`.

Módulos afectados:
- M2.

Pendientes resueltos por esta decisión:
- `IMP-002` (`PENDIENTES.md`) — resuelto.

Estado:
Vigente

---

## DEC-008

Fecha: 2026-09-20

Decisión:
"Configurar notificaciones" (Administrador, M5) es un **interruptor global**
a nivel de la `Organización` (`DEC-002`): las notificaciones in-app (M3)
quedan activadas o desactivadas para todos los usuarios de la instancia. No
hay configuración por tipo de evento ni por usuario/rol.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-003`.

Motivo:
Pendiente por definir.

HU afectadas:
- HU-28 (Configurar zona horaria y notificaciones generales) — la parte de
  notificaciones es un único valor booleano/activo-inactivo, no un conjunto
  de configuraciones granulares.

Módulos afectados:
- M3 (efecto de la configuración), M5 (dónde se configura).

Pendientes resueltos por esta decisión:
- `IMP-003` (`PENDIENTES.md`) — resuelto.

Estado:
Vigente

---

## DEC-009

Fecha: 2026-09-20

Decisión:
Los reportes de productividad del Administrador (cumplimiento, retraso,
carga de trabajo, tiempos de cierre, históricos) son **solo de consulta en
pantalla** (sin exportación a PDF/Excel) y se generan **bajo demanda** (sin
periodicidad programada/automática).

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-004`.

Motivo:
Pendiente por definir.

HU afectadas:
- HU-29 (Consultar reportes de productividad del equipo).

Módulos afectados:
- M5.

Pendientes resueltos por esta decisión:
- `IMP-004` (`PENDIENTES.md`) — resuelto en cuanto a formato de entrega y
  periodicidad. El contenido/columnas exactas de cada reporte queda para el
  detalle de HU-29 en Fase 6, no es un pendiente bloqueante.

Estado:
Vigente

---

## DEC-010

Fecha: 2026-09-20

Decisión:
La deuda técnica registrada en ADR-10 (el backend aún no valida el token
Bearer ni aplica autorización por rol en el servidor; la autorización
efectiva recae hoy en AWS Cognito) se trata como **bloqueante de RNF de
seguridad**: ninguna HU nueva de M2–M5 puede clasificarse como `Lista para
desarrollo` hasta que exista validación de token/rol en el servidor.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-005`.

Motivo:
Con `DEC-003` (autorización por Equipo, no solo por rol) y `DEC-004`
(multi-rol), la superficie de autorización creció respecto al momento en que
se registró ADR-10; el equipo considera que ya no es aceptable dejar la
autorización completa del lado de Cognito sin validación server-side.

HU afectadas:
- Todas las HU de M2–M5 aún en `Borrador` (HU-10 a HU-33) — su Definition of
  Ready deberá incluir esta validación como requisito no funcional antes de
  pasar a `Lista para desarrollo`.

Módulos afectados:
- M2, M3, M4, M5 (todos dependen de autorización por rol/equipo).

Pendientes resueltos por esta decisión:
- `IMP-005` (`PENDIENTES.md`) — resuelto en cuanto a criterio de bloqueo. El
  diseño técnico de la validación (filtro, middleware, etc.) no se especifica
  aquí (regla de no sobreespecificación técnica).

Estado:
Vigente

---

## DEC-011

Fecha: 2026-09-20

Decisión:
La zona horaria configurada por el Administrador (M5, HU-28) es **global a
nivel de la Organización** (`DEC-002`): una sola zona horaria aplica a todos
los usuarios de la instancia. No existe configuración de zona horaria por
usuario.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-006`.

Motivo:
Pendiente por definir.

HU afectadas:
- HU-28 (Configurar zona horaria y notificaciones generales).

Módulos afectados:
- M3, M4, M5 (cálculo de vencimientos/retrasos usa una única referencia
  horaria, sin necesidad de conversión por usuario).

Pendientes resueltos por esta decisión:
- `IMP-006` (`PENDIENTES.md`) — resuelto. `BLOQ-004` (umbral exacto de
  "retraso") permanece abierto, pero ya no depende de husos horarios
  distintos por usuario — solo falta el valor del umbral en sí.

Estado:
Vigente

---

## DEC-012

Fecha: 2026-09-20

Decisión:
Una tarea pasa al estado `Retrasada` (`DEC-005`) de forma **inmediata**: en
cuanto se cumple su fecha/hora límite (según la zona horaria global de la
Organización, `DEC-011`), si no está en estado `Completada` o `Cerrada`. No
existe margen de tolerancia adicional.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `BLOQ-004`.

Motivo:
Pendiente por definir.

HU afectadas:
- HU-24 (Alertar tarea retrasada al coordinador).
- HU-27 (Indicadores de cumplimiento y tareas vencidas/próximas/retrasadas).
- HU-15 (Actualizar estado de una tarea) — el estado `Retrasada` se activa
  por esta regla, no por acción manual del usuario.

Módulos afectados:
- M2 (transición de estado), M3 (disparo de alerta), M4 (indicador).

Pendientes resueltos por esta decisión:
- `BLOQ-004` (`PENDIENTES.md`) — resuelto.

Estado:
Vigente

---

## DEC-013

Fecha: 2026-09-20

Decisión:
El Administrador **sí** necesita poder crear/definir nuevos tipos de reporte,
más allá de los 5 ya documentados (productividad, cumplimiento, retraso,
carga de trabajo, tiempos de cierre).

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-008`.

Motivo:
Pendiente por definir.

**Nota de ampliación de alcance (`Origen: DEC`, transparencia obligatoria):**
esta capacidad **no está en el documento oficial** — sección 5.1 solo lista
verbos de "consultar" sobre reportes fijos. Es una decisión consciente del
equipo que **amplía el alcance** respecto al TDG original. Se recomienda
reflejar este cambio en la documentación académica del proyecto cuando
corresponda (fuera del alcance de escritura de este agente).

HU afectadas:
- Nueva HU-34 (Crear/definir nuevo tipo de reporte) — propuesta en
  `01-inventario-hu.md`, prioridad `Requiere confirmación` porque el
  mecanismo exacto (constructor de reportes personalizados, plantillas
  configurables, etc.) no está definido — ver `IMP-011`.

Módulos afectados:
- M5.

Pendientes resueltos por esta decisión:
- `IMP-008` (`PENDIENTES.md`) — resuelto en cuanto a "si se necesita". Se
  abre `IMP-011` sobre el mecanismo exacto.

Estado:
Vigente

---

## DEC-014

Fecha: 2026-09-20

Decisión:
Ciclo de vida operativo de la entidad `Equipo` (`DEC-003`):

- (a) El **Administrador crea el Equipo manualmente** (acción explícita,
  HU-31), definiendo al menos su nombre y su Coordinador inicial.
- (b) Un Colaborador se asigna a su Equipo **en el momento de crear su
  usuario** (amplía HU-08, no es un paso administrativo separado).
- (c) Todo Colaborador **debe tener un Equipo asignado siempre**; no existe
  el estado "Colaborador sin equipo".
- (d) Un Equipo **no puede eliminarse**; solo puede reasignarse su
  Coordinador (HU-32). No se definen reglas de cascada de eliminación.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-009`.

Motivo:
Pendiente por definir.

**Impacto retroactivo importante (`⚠️`):** el punto (b) modifica el alcance
de **HU-08 (Registro de usuarios por parte del administrador)**, que ya está
`Completada` en el backlog oficial (TG1/TG2, sin campo de Equipo en su
implementación actual). Esto es una ampliación funcional sobre una HU ya
construida, no un detalle nuevo de una HU en borrador — debe tratarse
explícitamente como trabajo adicional sobre HU-08 (o una HU de extensión
asociada), no asumirse como ya cubierto.

HU afectadas:
- HU-08 (**ya completada** — requiere extensión: agregar selección de Equipo
  al formulario de registro de usuario).
- HU-31 (Crear equipo) — pasa de `Requiere confirmación` a definida.
- HU-33 — se redefine: ya no es "asignar/reasignar colaborador a un equipo"
  en general, sino específicamente **reasignar** un Colaborador existente a
  otro Equipo (la asignación inicial ocurre en HU-08).

Módulos afectados:
- M1 (extensión de HU-08), M5 (HU-31, HU-33).

Pendientes resueltos por esta decisión:
- `IMP-009` (`PENDIENTES.md`) — resuelto. El detalle de campos exactos de
  `Equipo` más allá del nombre (descripción, etc.) queda como detalle menor
  para Fase 6, no bloqueante.

Estado:
Vigente

---

## DEC-015

Fecha: 2026-09-20

Decisión:
Transiciones adicionales del estado de `Tarea` (`DEC-005`):

- (a) `En reasignación` es un **instante de transición** (evento de
  auditoría), no un estado de reposo: al completarse la reasignación, la
  tarea **conserva el estado que tenía antes** de entrar a `En reasignación`
  (p. ej. si estaba `Retrasada`, sigue `Retrasada` con el nuevo responsable).
- (b) `Cancelada` puede alcanzarse **desde cualquier estado excepto
  `Cerrada`** (una tarea `Cerrada` es definitiva y no puede cancelarse).

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-010`.

Motivo:
Pendiente por definir.

HU afectadas:
- HU-17 (Reasignar tarea).
- HU-18 (Cancelar tarea).
- HU-15 (Actualizar estado de una tarea).

Módulos afectados:
- M2.

Pendientes resueltos por esta decisión:
- `IMP-010` (`PENDIENTES.md`) — resuelto.

Estado:
Vigente

---

## DEC-016

Fecha: 2026-09-20

Decisión:
Mecanismo de "crear nuevo tipo de reporte" (`DEC-013`, HU-34):

- (a) El Administrador elige entre un conjunto de **plantillas predefinidas
  ampliables** — no es un constructor de consultas libres (sin selección
  arbitraria de campos/filtros/agrupaciones).
- (b) Los reportes nuevos solo pueden construirse sobre los **datos ya
  usados** por los reportes existentes (tareas, usuarios, equipos), no sobre
  cualquier dato del sistema.
- (c) Un reporte creado **se guarda para reutilizarse**, con las mismas
  reglas ya decididas en `DEC-009` (solo consulta en pantalla, bajo demanda).

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, respuesta
directa a `IMP-011`.

Motivo:
Pendiente por definir.

**Nota de alcance:** con esta definición, HU-34 es más acotada de lo que su
nombre sugiere — es "configurar y guardar una instancia parametrizada de una
plantilla de reporte ya soportada por el sistema", no "definir un tipo de
reporte completamente nuevo desde cero". Se deja constancia para que la
redacción de HU-34 en Fase 6 no la sobredimensione.

HU afectadas:
- HU-34 (Crear / definir nuevo tipo de reporte).

Módulos afectados:
- M5.

Pendientes resueltos por esta decisión:
- `IMP-011` (`PENDIENTES.md`) — resuelto.

Estado:
Vigente

---

## DEC-017

Fecha: 2026-09-20

Decisión:
En Fase 6 se **reconstruirá la documentación completa** (formato
`.kiro/steering/hu-template.md`) también para **HU-01 a HU-09**, no solo para
las HU nuevas (HU-10 en adelante). Hasta ahora estaban tratadas como
"trazabilidad, no modificar" en `01-inventario-hu.md`.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20.

Motivo:
Pendiente por definir (el equipo no especificó la razón más allá de la
instrucción en sí).

**Alcance de la reconstrucción (aclaración para no inventar):** esto es
**reconstrucción de documentación**, no reapertura de desarrollo. El agente
solo escribe en `docs/historias-usuario/**` (límite de escritura ya
establecido); no se reimplementa código ni se cambia el comportamiento ya
construido salvo que el equipo identifique explícitamente una discrepancia
funcional real durante la reconstrucción. El `Estado: Completada` de estas
nueve HU **no cambia automáticamente** por esta decisión.

**Fuente para la reconstrucción:** el comportamiento ya construido debe
documentarse a partir de evidencia real (`compira-context.md` sección 14 —
"Detalle de comportamiento del flujo de autenticación" — y cualquier
confirmación adicional que dé el equipo), no inventando campos, validaciones
o mensajes que no estén ya confirmados. Donde falte evidencia, se registra
como pendiente igual que para cualquier otra HU.

HU afectadas:
- HU-01 a HU-09 (las nueve HU completadas de M1).

Módulos afectados:
- M1.

Estado:
Vigente

---

## DEC-018

Fecha: 2026-09-20

Decisión:
Revisión de granularidad del inventario completo, tres cambios aplicados:

- (a) **HU-22 fusionada en HU-21**: "Notificar nueva asignación" y
  "Notificar reasignación" son el mismo mecanismo (notificación in-app)
  disparado por dos eventos distintos, sin reglas de negocio propias más
  allá del texto del mensaje. HU-21 pasa a llamarse "Notificar evento de
  asignación de tarea (nueva asignación / reasignación)".
- (b) **HU-26 fusionada en HU-25**: "Filtrar panel de seguimiento" no tiene
  valor funcional independiente sin la vista base (HU-25) ya construida, y no
  tiene reglas de negocio propias — se incorpora como parte de los criterios
  de aceptación de HU-25.
- (c) **HU-28 dividida** en HU-28 (Configurar zona horaria) y **HU-35**
  nueva (Configurar notificaciones generales): son dos configuraciones sin
  relación funcional entre sí, con impacto en módulos distintos (zona
  horaria afecta cálculos de M3/M4 vía `DEC-011`/`DEC-012`; notificaciones
  afecta solo entrega de M3 vía `DEC-008`), cada una testeable por separado.

Origen:
Agente `compira-hu-analyst` (análisis de granularidad según `hu-rules.md`),
confirmado explícitamente por el equipo (Norbey Alonso Mejía Cortés),
2026-09-20.

Motivo:
Evitar fragmentación artificial (HU-22, HU-26 no superaban la prueba de
"reglas y criterios propios") y evitar HU que agrupen capacidades sin
relación funcional entre sí (HU-28 original).

HU afectadas:
- HU-21 (renombrada), HU-22 (`Fusionada en HU-21`).
- HU-25 (renombrada), HU-26 (`Fusionada en HU-25`).
- HU-28 (alcance reducido a zona horaria), HU-35 (nueva).

Módulos afectados:
- M3, M4, M5.

**Nota de identificadores:** `HU-22` y `HU-26` no se eliminan ni se
reutilizan (regla de identificadores inmutables); quedan registrados como
`Fusionada en HU-XX` para trazabilidad. `HU-35` es un identificador nuevo,
no una reutilización de `HU-30` (ya fusionada).

Estado:
Vigente

---

## DEC-019

Fecha: 2026-09-20

Decisión:
**HU-09 (Eliminación de usuarios por correo) queda `Descartada`**, pese a
figurar como `Completada` en el backlog oficial (`.kiro/steering/compira-
context.md`, sección 14: Sprint 3, Alta, 3 SP, "Elimina usuarios con
confirmación previa"). El identificador `HU-09` **no se elimina ni se
reutiliza** — queda registrado con `Estado: Descartada` para trazabilidad.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20. El equipo
indicó explícitamente que "en ningún momento se habló de esto" al
confrontarla con la cita textual del backlog oficial, y confirmó
descartarla de todas formas.

Motivo:
Pendiente por definir (no se proporcionó una razón más allá de la
instrucción de descartarla).

**Nota de divergencia importante (`⚠️`, transparencia obligatoria):** esta
decisión **contradice el backlog oficial del documento fuente**, que marca
HU-09 como ya completada en Sprint 3. A diferencia de `DEC-001` (que
divergía de una descripción funcional, no de un registro de "ya
construido"), aquí la divergencia es sobre **si una funcionalidad ya
implementada sigue vigente o no**. Esta decisión documental **no elimina
código ni funcionalidad ya desplegada** — el agente no toca código
(`agent/compira-hu-analyst.prompt.md`, "Límites de escritura"). Si la
capacidad de "eliminar usuarios" existe realmente en el sistema construido,
descartar la HU crea una **discrepancia entre la documentación del backlog y
el comportamiento real del sistema**, que el equipo debería resolver
explícitamente (¿se retira la funcionalidad del producto, o solo se descarta
el registro de HU y la funcionalidad sigue activa sin HU que la respalde?).
Se recomienda que el equipo aclare esto antes del cierre del proyecto.

HU afectadas:
- HU-09 → `Estado: Descartada`.

Módulos afectados:
- M1.

Estado:
Vigente

---

## DEC-020

Fecha: 2026-09-20

Decisión:
El panel de seguimiento en tiempo real (M4) debe incluir el indicador
**"carga de trabajo por responsable"** (cantidad de tareas asignadas por
Colaborador), visible también para el **Coordinador** (alcance: los
Colaboradores de su Equipo, `DEC-003`) y no solo para el Administrador a
través del reporte bajo demanda de M5 (`HU-29`/`DEC-023`).

Se incorpora como un indicador más de **HU-27** (indicadores de cumplimiento
y tareas vencidas/próximas/retrasadas), no de HU-25 (vista base con filtros),
porque HU-25 es la vista consolidada de tareas y HU-27 es específicamente la
HU que agrupa indicadores calculados sobre esas tareas — "carga de trabajo
por responsable" es un indicador (conteo agregado), no un filtro de la vista
base.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, en respuesta a
un hallazgo de auditoría del inventario.

Motivo:
Pendiente por definir (no se proporcionó una razón más allá de la decisión en
sí; el criterio de asociación a HU-27 en vez de HU-25 es un análisis del
agente, no una razón dada por el equipo).

HU afectadas:
- HU-27 (Consultar indicadores de cumplimiento y tareas vencidas/próximas/
  retrasadas) — amplía su alcance para incluir el indicador de carga de
  trabajo por responsable, con alcance de datos por rol análogo a `DEC-006`
  (Coordinador: su equipo; Administrador: todos los equipos).

Módulos afectados:
- M4.

Pendientes resueltos por esta decisión:
- Ninguno (hallazgo nuevo de auditoría, no derivado de un pendiente previo
  registrado).

Pendientes nuevos derivados de esta decisión:
- `IMP-014` (`PENDIENTES.md`) — si, dado este indicador ya visible en el
  panel M4, sigue siendo necesario un reporte separado de "carga de trabajo"
  bajo demanda en M5 (ver `DEC-023`).

Estado:
Vigente

---

## DEC-021

Fecha: 2026-09-20

Decisión:
**"Gestionar alertas de desempeño"** (función explícita del Coordinador,
documento oficial, sección 5.2 — `.kiro/steering/compira-context.md`) es una
**capacidad funcional distinta** de simplemente recibir la alerta de tarea
retrasada ya cubierta por HU-24 (Alertar tarea retrasada al coordinador). No
es una redundancia: falta una Historia de Usuario propia para esta
capacidad.

El **mecanismo exacto** de "gestionar" (por ejemplo: ¿configurar umbrales de
alerta?, ¿marcar una alerta como revisada/atendida?, ¿escalar una alerta?,
¿alguna combinación de estas acciones?) **no fue definido por el equipo** y
no se infiere de ninguna fuente disponible. No se inventa este detalle.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, en respuesta a
un hallazgo de auditoría del inventario.

Motivo:
Pendiente por definir.

HU afectadas:
- Nueva **HU-36** (Gestionar alertas de desempeño) — creada en `Estado:
  Borrador`, actor Coordinador, módulo **Pendiente por definir entre M3 y
  M4** (no hay evidencia suficiente para asignar uno de los dos con
  certeza: M3 es el módulo de recordatorios/alertas ya construido
  conceptualmente, M4 es el módulo donde vive el seguimiento de desempeño).
  No se documenta contenido completo (campos, reglas, criterios) hasta
  resolver `IMP-013`.

Módulos afectados:
- M3 y/o M4 (Pendiente por definir cuál).

Pendientes nuevos derivados de esta decisión:
- `IMP-013` (`PENDIENTES.md`) — mecanismo exacto de "gestionar" una alerta de
  desempeño (qué acciones concretas puede realizar el Coordinador).

Estado:
Vigente

---

## DEC-022

Fecha: 2026-09-20

Decisión:
**HU-20 (Consultar historial y trazabilidad de una tarea)** se amplía a
ambos roles: **Coordinador y Administrador**, con el mismo mecanismo
funcional ya aprobado para el panel de seguimiento en `DEC-006` (misma
vista/funcionalidad, alcance de datos distinto por rol):
- Coordinador: historial de las tareas de su Equipo (`DEC-003`).
- Administrador: historial de todas las tareas de la Organización
  (`DEC-002`).

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, en respuesta a
un hallazgo de auditoría del inventario.

Motivo:
Pendiente por definir (no se proporcionó una razón más allá de la analogía
explícita con `DEC-006` indicada por el equipo).

HU afectadas:
- HU-20 — actor pasa de "Coordinador" a **"Coordinador / Administrador"**,
  con alcance de datos por rol análogo a `DEC-006`.

Módulos afectados:
- M2.

Estado:
Vigente

---

## DEC-023

Fecha: 2026-09-20

Decisión:
Se **divide HU-29** (que agrupaba los 5 reportes del Administrador:
productividad, cumplimiento, retraso, carga de trabajo por responsable,
tiempos de cierre) en varias HU independientes, una por cada reporte, porque
cada uno puede tener columnas y reglas de cálculo propias (criterios y
reglas propios → regla de granularidad, `hu-rules.md`).

Distribución de identificadores:
- **HU-29** conserva el reporte de **productividad** (criterio: es el nombre
  original de la HU y el primero listado en el documento oficial, sección
  5.1).
- **HU-37** (nueva) — reporte de **cumplimiento** de tareas.
- **HU-38** (nueva) — reporte de **retraso** de tareas.
- **HU-39** (nueva) — reporte de **tiempos de cierre** de tareas.
- **Carga de trabajo por responsable**: no se crea una HU nueva (no se
  asigna `HU-40`) en este paso. Dado que `DEC-020` ya incorpora este mismo
  indicador al panel de seguimiento en tiempo real (M4, HU-27), no es
  evidente si un reporte separado bajo demanda (M5) sigue siendo necesario o
  se vuelve redundante. El agente no decide esto por su cuenta: se registra
  `IMP-014` como pendiente. Si el equipo confirma que sí se necesita un
  reporte separado, el siguiente identificador disponible sería `HU-40`.

Todas las HU resultantes (HU-29, HU-37, HU-38, HU-39) heredan las reglas ya
decididas para los reportes: solo consulta en pantalla, sin exportación, bajo
demanda (`DEC-009`); y quedan sujetas a la misma deuda de RNF de seguridad de
`DEC-010`.

Origen:
Equipo del proyecto (Norbey Alonso Mejía Cortés), 2026-09-20, en respuesta a
un hallazgo de auditoría del inventario.

Motivo:
Cada reporte (productividad, cumplimiento, retraso, tiempos de cierre) puede
tener columnas, filtros y reglas de cálculo propias y verificables de forma
independiente; agruparlos en una sola HU dificulta definir criterios de
aceptación específicos por reporte.

HU afectadas:
- HU-29 (alcance reducido a "reporte de productividad").
- HU-37, HU-38, HU-39 (nuevas).

Módulos afectados:
- M5.

Pendientes nuevos derivados de esta decisión:
- `IMP-014` (`PENDIENTES.md`) — si el reporte de "carga de trabajo por
  responsable" (M5, bajo demanda) sigue siendo necesario como HU separada
  además del indicador ya incorporado al panel M4 (`DEC-020`).

Estado:
Vigente

---

## DEC-024

Fecha: 2026-09-21

Decisión:
Se **fusiona HU-11 (Restablecer contraseña de un usuario por el administrador)
en HU-10 (Editar información y rol de un usuario existente)**. El
restablecimiento de contraseña pasa a ser una acción dentro de la
administración de un usuario existente, no una HU independiente. El
identificador `HU-11` no se elimina ni se reutiliza (`Estado: Fusionada en
HU-10`).

Origen:
Equipo del proyecto, 2026-09-21, aprobando la propuesta de consolidación de
granularidad del agente `compira-hu-analyst` para un backlog realizable con 2
personas y alcance académico.

Motivo:
HU-11 no supera la prueba de valor/flujo independiente: es una acción más de la
gestión de un usuario ya existente. Reduce una HU sin perder capacidad.

HU afectadas:
- HU-11 → `Fusionada en HU-10`.
- HU-10 — incorpora la acción de restablecer contraseña (detalle de flujo en
  `IMP-001`).

Módulos afectados:
- M5.

Pendientes afectados:
- `IMP-001` — se mantiene abierto, reasignado a HU-10 (alcance exacto del reset).

Estado:
Vigente

---

## DEC-025

Fecha: 2026-09-21

Decisión:
Se **fusiona HU-23 (Alertar tarea próxima a vencer) en HU-24**, que pasa a
llamarse "Alertas de vencimiento de tarea (próxima a vencer / retrasada)". Un
solo mecanismo de alerta temporal de tarea, con dos escenarios de aceptación
(próxima a vencer / retrasada) y sus destinatarios. `HU-23` no se elimina ni se
reutiliza (`Estado: Fusionada en HU-24`).

Origen:
Equipo del proyecto, 2026-09-21, aprobando la propuesta de consolidación.

Motivo:
Mismo criterio ya aplicado en `DEC-018(a)` para las notificaciones de
asignación/reasignación: mismo mecanismo (notificación in-app disparada por una
condición temporal) sin reglas de negocio propias que justifiquen dos HU.

HU afectadas:
- HU-23 → `Fusionada en HU-24`.
- HU-24 — renombrada y con dos escenarios (próxima a vencer / retrasada).

Módulos afectados:
- M3.

Estado:
Vigente

---

## DEC-026

Fecha: 2026-09-21

Decisión:
Se **reunifican HU-28 (Configurar zona horaria) y HU-35 (Configurar
notificaciones generales) en una sola HU-28 "Configuración general de la
organización (zona horaria global + notificaciones generales)"**. `HU-35` no se
elimina ni se reutiliza (`Estado: Fusionada en HU-28`).

**Reabre `DEC-018(c)`**, que las había separado. El equipo decide, para el
alcance académico y un equipo de 2 personas, que a esta escala son dos ajustes
de configuración de la única `Organización` (`DEC-002`) que se documentan y
prueban mejor como una sola HU con dos escenarios de aceptación.

Origen:
Equipo del proyecto, 2026-09-21, aprobando la propuesta de consolidación
(reapertura consciente de `DEC-018(c)`).

Motivo:
El beneficio de separar (módulos distintos, testeable por separado, argumento de
`DEC-018(c)`) no compensa el costo de dos HU completas a esta escala. Reduce el
backlog sin perder capacidad; ambas configuran la misma entidad `Organización`.

HU afectadas:
- HU-35 → `Fusionada en HU-28`.
- HU-28 — cubre zona horaria global (`DEC-011`) y notificaciones generales
  (interruptor global, `DEC-008`).

Módulos afectados:
- M3 (efecto de notificaciones), M4 (efecto de zona horaria), M5 (dónde se
  configura).

Relación con decisiones previas:
- Reabre y sustituye el punto (c) de `DEC-018` (división de HU-28). Las demás
  partes de `DEC-018` (fusión HU-22→HU-21, HU-26→HU-25) siguen vigentes.

Estado:
Vigente

---

## DEC-027

Fecha: 2026-09-21

Decisión:
Se **reúnen los cuatro reportes del Administrador en dos HU** y se **difiere
HU-34**:
- **HU-29** pasa a "Reportes de productividad y cumplimiento de tareas"
  (absorbe HU-37).
- **HU-38** pasa a "Reportes de retraso y tiempos de cierre de tareas"
  (absorbe HU-39).
- `HU-37` y `HU-39` no se eliminan ni se reutilizan (`Estado: Fusionada en
  HU-29` / `Fusionada en HU-38`).
- **HU-34** (Configurar/guardar reporte desde plantilla) se marca `POSIBLE
  AMPLIACIÓN DE ALCANCE` y se **difiere** a "si sobra tiempo" (prioridad Baja,
  `Estado: Con pendientes`).

Además se **ajustan dependencias** mal puestas detectadas en la consolidación:
- HU-12 depende de M1 + HU-08 + HU-31 (no solo de HU-02).
- HU-20 depende también de HU-15 y HU-17 (existen cambios que historiar), no
  solo de HU-12.
- HU-27 depende de los datos de tareas (HU-15), no de la vista de panel (HU-25).

**Reabre `DEC-023`** (que había dividido los reportes en cuatro HU) y matiza
`DEC-013`/`DEC-016` (HU-34 no se descarta, se difiere).

Origen:
Equipo del proyecto, 2026-09-21, aprobando la propuesta de consolidación
(reapertura consciente de `DEC-023` y diferimiento de la ampliación de alcance
`DEC-013`/`DEC-016`).

Motivo:
Los cuatro reportes comparten fuente de datos (tareas) y patrón de presentación
(solo pantalla, bajo demanda, `DEC-009`); sus diferencias (columnas y cálculo)
se cubren con escenarios de aceptación distintos dentro de una HU, sin requerir
cuatro HU. HU-34 amplía el alcance más allá del documento oficial y no es
prioritaria para el objetivo académico. Las dependencias ajustadas evitan
distorsionar el orden de sprints de un equipo pequeño.

HU afectadas:
- HU-37 → `Fusionada en HU-29`; HU-39 → `Fusionada en HU-38`.
- HU-29 y HU-38 — renombradas (dos reportes cada una).
- HU-34 — diferida (`POSIBLE AMPLIACIÓN DE ALCANCE`).
- HU-12, HU-20, HU-27 — dependencias ajustadas.

Módulos afectados:
- M2 (dependencias), M5 (reportes).

Relación con decisiones previas:
- Reabre `DEC-023` (división de reportes). Mantiene `DEC-009` (formato y
  periodicidad de reportes). `DEC-013`/`DEC-016` siguen vigentes en cuanto al
  mecanismo de HU-34, pero su ejecución se difiere.

Estado:
Vigente

---

## DEC-028

Fecha: 2026-09-21

Decisión:
Dos puntos de cobertura:
- (a) Se **resuelve `IMP-014`**: el indicador "carga de trabajo por responsable"
  queda cubierto en el panel de seguimiento (M4, HU-27, `DEC-020`); **no** se
  crea un reporte separado bajo demanda en M5 (no se usa `HU-40` para ello).
- (b) Se **crean dos HU faltantes** detectadas en la consolidación, para cerrar
  huecos de cobertura de M5:
  - **HU-40** — Consultar / listar usuarios de la organización (habilita
    seleccionar el usuario a editar/administrar, HU-10).
  - **HU-41** — Consultar / listar equipos y sus miembros (habilita HU-32 y
    HU-33).

Con esto, el siguiente identificador disponible es `HU-42`.

Origen:
Equipo del proyecto, 2026-09-21, aprobando la propuesta de consolidación.

Motivo:
(a) Evitar duplicar el mismo indicador como vista y como reporte a esta escala.
(b) La administración de usuarios (editar, resetear) y de equipos (reasignar
coordinador/colaborador) presupone poder listarlos y seleccionarlos; sin estas
consultas, HU-10, HU-32 y HU-33 no son operables.

HU afectadas:
- HU-27 — mantiene el indicador de carga de trabajo (sin HU de reporte separada).
- HU-40 (nueva), HU-41 (nueva).

Módulos afectados:
- M4 (HU-27), M5 (HU-40, HU-41).

Pendientes afectados:
- `IMP-014` — resuelto.

Estado:
Vigente
