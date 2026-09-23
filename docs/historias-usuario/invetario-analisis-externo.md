# Inventario de Historias de Usuario — COMPIRA (propuesta inicial)

> Fase 3 (reanálisis completo, sustituye la versión anterior). Propuesta de
> inventario, **sin desarrollar contenido completo de HU**. Revisión de
> granularidad ya aplicada (`DEC-018`, 2026-09-20). Pendiente de **aprobación
> humana final del inventario completo** antes de generar HU completas
> (Fase 6).
>
> HU-01 a HU-09 (módulo M1) ya existen y están **Completadas** según el
> backlog oficial (`.kiro/steering/compira-context.md`, sección 14). Por
> `DEC-017` (2026-09-20), **también se reconstruirá su documentación
> completa** en Fase 6 (formato `hu-template.md`), no solo la de las HU
> nuevas — es reconstrucción de documentación, no reapertura de desarrollo;
> su `Estado: Completada` no cambia por esto. La numeración nueva continúa
> desde HU-10 (identificadores inmutables, no se reutilizan ni se renumeran).
> Tras la auditoría del 2026-09-20 (`DEC-020` a `DEC-023`), el inventario
> llega hasta **HU-39**; el siguiente identificador disponible es `HU-40`.

## HU existentes (M1 — su documentación completa se reconstruirá en Fase 6, `DEC-017`)

| ID | Módulo | Nombre | Actor | Prioridad | Estado |
|---|---|---|---|---|---|
| HU-01 | M1 | Cambio obligatorio de contraseña en el primer inicio de sesión | Todos | Alta | Completada |
| HU-02 | M1 | Inicio de sesión con correo y contraseña | Todos | Alta | Completada |
| HU-03 | M1 | Verificación en dos pasos mediante código OTP por correo | Todos | Alta | Completada |
| HU-04 | M1 | Reenvío del código OTP de acceso | Todos | Media | Completada |
| HU-05 | M1 | Solicitud de recuperación de contraseña por correo | Todos | Alta | Completada |
| HU-06 | M1 | Confirmación del restablecimiento de contraseña con código | Todos | Alta | Completada |
| HU-07 | M1 | Cierre de sesión y control de acceso a rutas protegidas | Todos | Alta | Completada |
| HU-08 | M1 | Registro de usuarios por parte del administrador ⚠️ *(requiere extensión: capturar Equipo al crear el usuario, `DEC-014`)* | Administrador | Alta | Completada |
| HU-09 | M1 | ~~Eliminación de usuarios por correo~~ ⚠️ *(descartada pese a figurar `Completada` en el backlog oficial, `DEC-019`)* | Administrador | — | **Descartada** *(2026-09-20)* |

## Propuesta de nuevas HU (Borrador — requiere revisión de granularidad y aprobación)

> **Nota de prioridad — `RECOMENDACIÓN — requiere aprobación` (2026-09-20):**
> los valores de la columna `Prioridad` para **HU-10 a HU-39** son
> propuestas del agente `compira-hu-analyst` para facilitar la planificación
> inicial; el equipo confirmó explícitamente que estos valores **no
> constituyen una decisión formal** (no se registra como `DEC-XXX`) y
> permanecen como recomendación hasta que el Product Owner los apruebe o los
> ajuste. No se repite esta nota en cada fila.

| ID | Módulo | Nombre | Actor | Dependencias | Prioridad | Estado |
|---|---|---|---|---|---|---|
| HU-10 | M5 *(resuelto — `BLOQ-003`)* | Editar información y rol de un usuario existente | Administrador | HU-08 | Alta | Borrador |
| HU-11 | M5 *(resuelto — `BLOQ-003`)* | Restablecer contraseña de un usuario (acción del administrador) | Administrador | HU-08 | Media *(alcance exacto en `IMP-001`)* | Borrador |
| HU-12 | M2 | Crear tarea (título, descripción, fechas) | Coordinador | HU-02 | Alta | Borrador |
| HU-13 | M2 | Asignar tarea a un responsable *(DEC-001: responsable único, no "uno o varios")* | Coordinador | HU-12 | Alta | Borrador |
| HU-14 | M2 | Consultar tareas asignadas | Colaborador | HU-13 | Alta | Borrador |
| HU-15 | M2 | Actualizar estado de una tarea | Colaborador | HU-13 | Alta | Borrador |
| HU-16 | M2 | Registrar observaciones de ejecución de una tarea | Colaborador | HU-13 | Media | Borrador |
| HU-17 | M2 | Reasignar tarea (cambio de responsable con histórico del anterior; conserva el estado previo, `DEC-015`) *(DEC-001)* | Coordinador | HU-13 | Alta | Borrador |
| HU-18 | M2 | Cancelar tarea *(desde cualquier estado excepto Cerrada, `DEC-015`)* | Coordinador | HU-12 | Media | Borrador |
| HU-19 | M2 | Aprobar / cerrar tarea *(obligatoria para toda tarea completada, `DEC-007`)* | Coordinador | HU-15 | Alta | Borrador |
| HU-20 | M2 | Consultar historial y trazabilidad de una tarea *(ampliada a Coordinador/Administrador, alcance por rol análogo a `DEC-006` — `DEC-022`)* | Coordinador / Administrador | HU-12 | Media | Borrador |
| HU-21 | M3 | Notificar evento de asignación de tarea (nueva asignación / reasignación) *(`DEC-018`, fusión con HU-22)* | Colaborador | HU-13, HU-17 | Alta | Borrador |
| HU-22 | M3 | ~~Notificar reasignación de tarea~~ | Colaborador | HU-21 | — | **Fusionada en HU-21** *(2026-09-20, `DEC-018`)* |
| HU-23 | M3 | Alertar tarea próxima a vencer | Colaborador | HU-12 | Media | Borrador |
| HU-24 | M3 | Alertar tarea retrasada al coordinador *(umbral inmediato al vencimiento, `DEC-012`)* | Coordinador | HU-12 | Alta | Borrador |
| HU-25 | M4 | Consultar panel de seguimiento (incluye filtros por usuario/estado/fecha) *(alcance por rol: Coordinador = su equipo, Administrador = todos los equipos — `DEC-006`; fusión con HU-26, `DEC-018`)* | Coordinador / Administrador | HU-13, HU-15 | Alta | Borrador |
| HU-26 | M4 | ~~Filtrar panel de seguimiento por usuario/estado/fecha~~ | Coordinador / Administrador | HU-25 | — | **Fusionada en HU-25** *(2026-09-20, `DEC-018`)* |
| HU-27 | M4 | Consultar indicadores de cumplimiento y tareas vencidas/próximas/retrasadas *(incluye "carga de trabajo por responsable", visible a Coordinador —su equipo— y Administrador —todos los equipos—, `DEC-020`)* | Coordinador / Administrador | HU-25 | Media | Borrador |
| HU-28 | M5 | Configurar zona horaria de la organización *(global, `DEC-011`; dividida de las notificaciones — ver HU-35, `DEC-018`)* | Administrador | — | Media | Borrador |
| HU-29 | M5 | Consultar reporte de productividad del equipo *(solo pantalla, bajo demanda, `DEC-009`; dividida de los demás reportes — ver HU-37 a HU-39, `DEC-023`)* | Administrador | HU-15, HU-19 | Media | Borrador |
| HU-30 | M4 | ~~Monitorear panel de equipos (vista Administrador)~~ | Administrador | HU-25 | — | **Fusionada en HU-25** *(2026-09-20, `DEC-006`)* |
| HU-31 | M5 | Crear equipo (nombre + Coordinador inicial, `DEC-014`) | Administrador | — | Alta | Borrador |
| HU-32 | M5 | Asignar / reasignar coordinador de un equipo | Administrador | HU-31 | Alta | Borrador |
| HU-33 | M5 | Reasignar colaborador a otro equipo *(la asignación inicial ocurre en HU-08, `DEC-014`)* | Administrador | HU-31 | Media | Borrador |
| HU-34 | M5 | Configurar y guardar reporte a partir de plantilla predefinida *(`DEC-013`, `DEC-016` — ampliación de alcance, no constructor libre)* | Administrador | HU-29 | Media | Borrador |
| HU-35 | M5 | Configurar notificaciones generales (interruptor global, `DEC-008`) *(dividida de HU-28, `DEC-018`)* | Administrador | — | Media | Borrador |
| HU-36 | M3 / M4 *(Pendiente por definir cuál — `DEC-021`)* | Gestionar alertas de desempeño *(capacidad distinta de recibir la alerta de retraso, HU-24; mecanismo exacto pendiente, `IMP-013`)* | Coordinador | HU-24 | Media | Borrador |
| HU-37 | M5 | Consultar reporte de cumplimiento de tareas *(dividida de HU-29, `DEC-023`; solo pantalla, bajo demanda, `DEC-009`)* | Administrador | HU-15, HU-19 | Media | Borrador |
| HU-38 | M5 | Consultar reporte de retraso de tareas *(dividida de HU-29, `DEC-023`; solo pantalla, bajo demanda, `DEC-009`)* | Administrador | HU-15, HU-19 | Media | Borrador |
| HU-39 | M5 | Consultar reporte de tiempos de cierre de tareas *(dividida de HU-29, `DEC-023`; solo pantalla, bajo demanda, `DEC-009`)* | Administrador | HU-15, HU-19 | Media | Borrador |

**Nota sobre el reporte de "carga de trabajo por responsable" (`DEC-023`,
`IMP-014`):** no se crea `HU-40` en este paso. El indicador ya queda cubierto
en el panel de seguimiento en tiempo real (HU-27, `DEC-020`); si el equipo
confirma que además se necesita un reporte separado bajo demanda en M5, el
siguiente identificador disponible sería `HU-40`.

**Notas de esta propuesta (no aplicar cambios sin aprobación):**

- **⚠️ DEC-019 (2026-09-20):** HU-09 descartada pese a figurar `Completada`
  en el backlog oficial (Sprint 3, "Elimina usuarios con confirmación
  previa"). El identificador no se reutiliza. **Discrepancia sin resolver:**
  si la funcionalidad de eliminar usuarios existe realmente en el sistema
  construido, descartar la HU deja esa funcionalidad sin respaldo
  documental — el equipo debería aclarar si también se retira del producto.
- **DEC-018 (2026-09-20) — revisión de granularidad aplicada:**
  HU-22 fusionada en HU-21 (mismo mecanismo de notificación, disparado por
  dos eventos, sin reglas propias diferenciadas); HU-26 fusionada en HU-25
  (los filtros no tienen valor funcional independiente sin la vista base);
  HU-28 dividida en HU-28 (zona horaria) y **HU-35** nueva (notificaciones
  generales), por ser configuraciones sin relación funcional entre sí con
  impacto en módulos distintos (M3/M4 vs. M3). Los identificadores
  fusionados (`HU-22`, `HU-26`) no se reutilizan ni se eliminan.
- **⚠️ DEC-014 (2026-09-20):** el ciclo de vida de `Equipo` quedó definido
  (Administrador lo crea manualmente; el Colaborador se asigna a su Equipo
  al crearse; Equipo no se elimina). **Impacto retroactivo:** HU-08, ya
  `Completada`, necesita extenderse para capturar el Equipo al registrar un
  usuario — no es un detalle nuevo de una HU en borrador, es trabajo
  adicional sobre algo ya construido. HU-33 se redefinió: ya no es
  "asignar/reasignar", solo **reasignar** un Colaborador existente (la
  asignación inicial vive en HU-08).
- **DEC-010 (2026-09-20):** la validación de token/rol en el servidor
  (deuda ADR-10) es un RNF de seguridad **bloqueante** para llevar cualquier
  HU de M2–M5 a `Lista para desarrollo`. Aplica transversalmente a **HU-10 a
  HU-33** (cita corregida: `DECISIONES.md`/`DEC-010` dice textualmente
  "HU-10 a HU-33", no "HU-12 a HU-33" como decía una versión anterior de
  esta nota), no se repite la nota en cada fila. Por extensión de la misma
  regla, también aplica a las HU nuevas de este mismo paso (HU-36 a HU-39,
  todas en M3/M4/M5).
- **DEC-006 (2026-09-20):** el panel de seguimiento (M4) es la misma
  vista/funcionalidad para Coordinador y Administrador, solo cambia el
  alcance de datos. HU-25/26/27 ahora se asocian a ambos actores. **HU-30 fue
  fusionada en HU-25** (confirmado explícitamente por el equipo, 2026-09-20):
  el identificador `HU-30` no se elimina ni se reutiliza (regla de
  identificadores inmutables), queda registrado con `Estado: Fusionada en
  HU-25` para trazabilidad.
- **DEC-005 (2026-09-20):** conjunto cerrado de estados de tarea confirmado
  (`Pendiente`, `En progreso`, `Retrasada`, `En reasignación`, `Completada`,
  `Cerrada`, `Cancelada`). HU-15 (actualizar estado) opera sobre este
  conjunto; HU-17 (reasignar) dispara `En reasignación`; HU-19
  (aprobar/cerrar) produce la transición `Completada` → `Cerrada`. Detalle de
  transiciones exactas pendiente en `IMP-010`; condición de aprobación
  pendiente en `IMP-002`.
- **DEC-003 (2026-09-20):** se confirma la entidad `Equipo` (Colaborador 1:1
  Equipo; Equipo 1:1 Coordinador, reasignable por el Administrador). HU-13,
  HU-25, HU-26, HU-27 y HU-30 ya no asumen "equipo" implícitamente — la
  entidad está confirmada. Se agregan HU-31 a HU-33 para su ciclo de vida
  (alcance operativo exacto resuelto después en `DEC-014`).
- **DEC-004 (2026-09-20):** un usuario puede tener más de un rol
  simultáneamente. No genera una HU nueva por sí sola, pero afecta el diseño
  de autorización (M1) y de asignación de roles (HU-10): la asignación de rol
  ya no es una selección única, sino un conjunto de roles por usuario.
- **DEC-002 (2026-09-20):** COMPIRA es mono-organización por despliegue; se
  descarta la necesidad de una HU de "alta de organización" en M1. HU-28
  cubre la configuración de la única `Organización` de la instancia.
- **DEC-001 (2026-09-20):** HU-13 y HU-17 reflejan el modelo de responsable
  único con reasignación e histórico, decidido en `DECISIONES.md`, en
  divergencia consciente con el texto literal del documento oficial ("uno o
  varios colaboradores"). HU-15 (actualizar estado) ya no requiere modelar
  estado compartido entre varios responsables.
- **`BLOQ-003` resuelto (2026-09-20):** HU-08/HU-09 confirmadas en M1 (cita
  textual del documento oficial); HU-10/HU-11 confirmadas en M5
  (administración continua de usuarios, distinta del alta/baja de M1).
- **DEC-013 (2026-09-20):** el equipo confirmó que el Administrador sí
  necesita crear nuevos tipos de reporte — es una **ampliación de alcance
  consciente** respecto al documento oficial (HU-29 seguía cubriendo
  correctamente solo la consulta de los 5 reportes ya documentados). Se
  agrega HU-34 para esta capacidad nueva; su mecanismo exacto queda en
  `IMP-011`.
- No se generó contenido completo (campos, validaciones, criterios de
  aceptación) para ninguna HU nueva; corresponde a la Fase 6, solo tras
  aprobación del inventario.
- **DEC-020 (2026-09-20):** el panel de seguimiento (M4) debe incluir el
  indicador "carga de trabajo por responsable", visible también para el
  Coordinador (alcance: su equipo) y no solo para el Administrador vía el
  reporte de M5. Se incorpora a **HU-27** (indicadores), no a HU-25 (vista
  base con filtros).
- **DEC-021 (2026-09-20):** "gestionar alertas de desempeño" (función
  explícita del Coordinador, documento oficial sección 5.2) es una
  capacidad distinta de recibir la alerta de retraso (HU-24). Se crea
  **HU-36** en `Borrador`, módulo **Pendiente por definir entre M3 y M4**; el
  mecanismo exacto de "gestionar" queda en `IMP-013`.
- **DEC-022 (2026-09-20):** HU-20 se amplía a **Coordinador / Administrador**,
  con el mismo mecanismo de alcance de datos por rol de `DEC-006`
  (Administrador: toda la organización; Coordinador: su equipo).
- **DEC-023 (2026-09-20):** HU-29 se divide en varias HU independientes, una
  por cada reporte (cada uno con columnas y reglas de cálculo propias):
  HU-29 conserva "productividad" (nombre original); se crean **HU-37**
  (cumplimiento), **HU-38** (retraso) y **HU-39** (tiempos de cierre). El
  reporte de "carga de trabajo por responsable" no se convierte en HU nueva
  todavía (no se asigna `HU-40`): dado que `DEC-020` ya incorpora ese mismo
  indicador al panel M4 (HU-27), no es evidente si sigue haciendo falta un
  reporte separado en M5 — se registra `IMP-014` en vez de decidirlo.
- **⚠️ IMP-012 (2026-09-20, formaliza nota de `DEC-019`):** el equipo
  respondió `Pendiente por definir` sobre si la funcionalidad de eliminar
  usuarios (HU-09) sigue existiendo en el sistema construido. Este punto
  debe resolverse antes del cierre del proyecto — ver `PENDIENTES.md`.