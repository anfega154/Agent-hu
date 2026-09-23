# 01 — Inventario de Historias de Usuario (propuesta)

> Documento de la **Fase 3 (Inventario de HU)**. **Propuesta** para revisión y
> aprobación del equipo / Product Owner. **No** contiene HU desarrolladas.
>
> Inventario **reconciliado con las decisiones del equipo** (`DECISIONES.md`,
> DEC-001 a DEC-028) y con la **consolidación de granularidad y alcance** aprobada
> el 2026-09-21 para un equipo de 2 personas y el alcance académico definido. Ante
> conflicto, prevalece la decisión del equipo (jerarquía de `hu-rules.md`).
>
> Reglas aplicadas:
> - **Identificadores inmutables:** no se renumeran ni se reutilizan. Las HU
>   fusionadas o descartadas conservan su ID con el estado correspondiente.
> - **Estado de desarrollo vs conformidad documental** para HU-01..HU-08: su
>   software está `Completada`; su documentación se **reconstruirá** en Fase 6
>   (`DEC-017`), sin cambiar el estado de desarrollo.
> - No se crean HU por componentes técnicos, solo por capacidades funcionales.
> - **Prioridad HU-10 en adelante:** `RECOMENDACIÓN — requiere aprobación` (el
>   equipo confirmó que no es decisión formal).
>
> | Campo | Valor |
> |---|---|
> | Versión | 4.1 |
> | Última actualización | 2026-09-21 |
> | Estado del inventario | Propuesta consolidada — **Fase 6: M1..M5 generadas según plantilla (20 HU activas). Diferidas HU-34, HU-36 sin generar** |
>
> Cambios v4.0 (consolidación aprobada, DEC-024..DEC-028): fusiones HU-23→HU-24,
> HU-35→HU-28, HU-11→HU-10, HU-37→HU-29, HU-39→HU-38; HU-34 diferida por alcance;
> `IMP-014` resuelto sin crear reporte separado; HU-36 fuera de ruta crítica;
> nuevas HU-40 (listar usuarios) y HU-41 (listar equipos). Ajuste de dependencias.

---

## Leyenda

**Estados de HU (lista única):** `Borrador` · `En análisis` · `Con pendientes` ·
`Lista para validación` · `Aprobada` · `Lista para desarrollo` · `En desarrollo` ·
`En validación` · `Completada` · `Descartada` · `Fusionada en HU-XXX` ·
`Reemplazada por HU-XXX`.

**Conformidad documental** (ajuste a `hu-template.md`): `Conforme` ·
`Pendiente de reconstrucción` (Fase 6) · `N/A` (aún sin documento).

---

## Parte A — HU existentes (módulo M1)

Del backlog oficial (`compira-context.md` §14). Identificadores inmutables.
Estado de desarrollo `Completada`. Por `DEC-017`, su documentación completa se
**reconstruirá** en Fase 6 (reconstrucción documental, no reapertura de
desarrollo). Origen: DOC/HU/DEC.

| ID | Módulo | Nombre | Actor | Prioridad | Estado desarrollo | Conformidad documental | Notas |
|---|---|---|---|---|---|---|---|
| HU-01 | M1 | Cambio obligatorio de contraseña en el primer inicio de sesión | Todos | Alta | Completada | Conforme (Fase 6, v1.0) · DoR Cumple | `DEC-017` |
| HU-02 | M1 | Inicio de sesión con correo y contraseña | Todos | Alta | Completada | Conforme (Fase 6, v1.0) · DoR Cumple | `DEC-017` |
| HU-03 | M1 | Verificación en dos pasos mediante código OTP por correo | Todos | Alta | Completada | Conforme (Fase 6, v1.0) · DoR Cumple | `DEC-017` |
| HU-04 | M1 | Reenvío del código OTP de acceso | Todos | Media | Completada | Conforme (Fase 6, v1.0) · DoR Cumple | `DEC-017` |
| HU-05 | M1 | Solicitud de recuperación de contraseña por correo | Todos | Alta | Completada | Conforme (Fase 6, v1.0) · DoR Cumple | `DEC-017` |
| HU-06 | M1 | Confirmación del restablecimiento de contraseña con código | Todos | Alta | Completada | Conforme (Fase 6, v1.0) · DoR Cumple | `DEC-017` |
| HU-07 | M1 | Cierre de sesión y control de acceso a rutas protegidas | Todos | Alta | Completada | Conforme (Fase 6, v1.0) · DoR Cumple | `DEC-017` |
| HU-08 | M1 | Registro de usuarios por parte del administrador ⚠️ requiere extensión: capturar Equipo al crear el usuario | Administrador | Alta | Completada (flujo base) | Conforme (Fase 6, v1.1) · DoR **No cumple** para la extensión de Equipo | `DEC-014`, `DEC-017` |
| HU-09 | M1 | ~~Eliminación de usuarios por correo~~ | Administrador | — | (figura Completada en backlog oficial) | N/A | **Descartada** (`DEC-019`). ⚠️ Discrepancia con backlog oficial sin resolver (`IMP-012`). ID no reutilizado. |

---

## Parte B — HU nuevas activas (propuesta consolidada, sin desarrollar)

Estado `Borrador` salvo indicación. Conformidad documental `N/A`. `DEC-010`: la
validación de token/rol en el servidor (deuda ADR-10) es un **RNF de seguridad
bloqueante** para llevar cualquier HU de M2–M5 a `Lista para desarrollo` (aplica
transversalmente; no se repite por fila).

### M5 — Gestión continua de usuarios

Generadas en Fase 6 (v1.0) según `hu-template.md`. `Borrador`, DoR No cumple.

| ID | Nombre | Actor | Dependencias | Prioridad | Estado | Conformidad doc. | Decisiones |
|---|---|---|---|---|---|---|---|
| HU-10 | Editar información y rol de un usuario existente (incluye restablecer su contraseña) | Administrador | HU-08, HU-40 | Alta | Borrador | Generada · DoR No cumple (IMP-001, `DEC-010`) | `BLOQ-003`, `DEC-004`, `DEC-024` (absorbe HU-11), `IMP-001` |
| HU-40 | Consultar / listar usuarios de la organización | Administrador | HU-08 | Alta | Borrador | Generada · DoR No cumple (`DEC-010`) | `DEC-028` (hueco de cobertura). Habilita HU-10 |

### M2 — Gestión de tareas

Todas generadas en Fase 6 (v1.0) según `hu-template.md`. Todas en `Borrador` con
**DoR No cumple** (bloqueo transversal `DEC-010` + pendientes propios).

| ID | Nombre | Actor | Dependencias | Prioridad | Estado | Conformidad doc. | Decisiones |
|---|---|---|---|---|---|---|---|
| HU-12 | Crear tarea (título, descripción, fechas) | Coordinador | M1, HU-08, HU-31 | Alta | Borrador | Generada · DoR No cumple (IMP-017) | Dependencia ajustada (`DEC-027`) |
| HU-13 | Asignar tarea a un responsable (responsable único, no "uno o varios") | Coordinador | HU-12 | Alta | Borrador | Generada · DoR No cumple | `DEC-001` |
| HU-14 | Consultar tareas asignadas | Colaborador | HU-13 | Alta | Borrador | Generada · DoR No cumple | — |
| HU-15 | Actualizar estado de una tarea | Colaborador | HU-13 | Alta | Borrador | Generada · DoR No cumple (IMP-019) | `DEC-005` (7 estados), `DEC-007`, `DEC-012`, `DEC-015`. **HU de mayor esfuerzo: priorizar temprano** |
| HU-16 | Registrar observaciones de ejecución de una tarea | Colaborador | HU-13 | Media | Borrador | Generada · DoR No cumple | — |
| HU-17 | Reasignar tarea (cambio de responsable con histórico; conserva estado previo) | Coordinador | HU-13 | Alta | Borrador | Generada · DoR No cumple (IMP-018) | `DEC-001`, `DEC-005`, `DEC-015(a)` |
| HU-18 | Cancelar tarea (desde cualquier estado excepto `Cerrada`) | Coordinador | HU-12 | Media | Borrador | Generada · DoR No cumple | `DEC-015(b)` |
| HU-19 | Aprobar / cerrar tarea (obligatoria para toda tarea completada) | Coordinador | HU-15 | Alta | Borrador | Generada · DoR No cumple (IMP-002) | `DEC-005`, `DEC-007` |
| HU-20 | Consultar historial y trazabilidad de una tarea | Coordinador / Administrador | HU-12, HU-15, HU-17 | Media | Borrador | Generada · DoR No cumple (auditoría RT-03) | `DEC-022`; dependencia ajustada (`DEC-027`) |

### M3 — Recordatorios y alertas

Generadas en Fase 6 (v1.0) según `hu-template.md`. `Borrador`, DoR No cumple.

| ID | Nombre | Actor | Dependencias | Prioridad | Estado | Conformidad doc. | Decisiones |
|---|---|---|---|---|---|---|---|
| HU-21 | Notificar evento de asignación de tarea (nueva asignación / reasignación) | Colaborador | HU-13, HU-17 | Alta | Borrador | Generada · DoR No cumple (IMP-020, `DEC-010`) | `DEC-018(a)` (fusiona HU-22) |
| HU-24 | Alertas de vencimiento de tarea (próxima a vencer / retrasada) | Colaborador / Coordinador | HU-12, HU-15 | Alta | Borrador | Generada · DoR No cumple (**BLOQ-009**, `DEC-010`) | `DEC-012`, `DEC-025` (fusiona HU-23) |

### M4 — Panel de seguimiento

Generadas en Fase 6 (v1.0) según `hu-template.md`. `Borrador`, DoR No cumple.

| ID | Nombre | Actor | Dependencias | Prioridad | Estado | Conformidad doc. | Decisiones |
|---|---|---|---|---|---|---|---|
| HU-25 | Consultar panel de seguimiento (incluye filtros por usuario/estado/fecha) | Coordinador / Administrador | HU-13, HU-15 | Alta | Borrador | Generada · DoR No cumple (`DEC-010`) | `DEC-006`, `DEC-018(b)` (fusiona HU-26), `DEC-006` (fusiona HU-30) |
| HU-27 | Consultar indicadores de cumplimiento y tareas vencidas/próximas/retrasadas (incluye "carga de trabajo por responsable") | Coordinador / Administrador | HU-15 | Media | Borrador | Generada · DoR No cumple (**BLOQ-009**, IMP-021, `DEC-010`) | `DEC-006`, `DEC-020`, `DEC-012`, `DEC-028`; dep. ajustada `DEC-027` |

### M5 — Administración (organización, equipos, reportes)

Generadas en Fase 6 (v1.0) según `hu-template.md`. `Borrador`, DoR No cumple.

| ID | Nombre | Actor | Dependencias | Prioridad | Estado | Conformidad doc. | Decisiones |
|---|---|---|---|---|---|---|---|
| HU-28 | Configuración general de la organización (zona horaria global + notificaciones generales) | Administrador | — | Media | Borrador | Generada · DoR No cumple (IMP-023, `DEC-010`) | `DEC-002`, `DEC-011`, `DEC-008`, `DEC-026` (reúne HU-35) |
| HU-29 | Reportes de productividad y cumplimiento de tareas (solo pantalla, bajo demanda) | Administrador | HU-15, HU-19 | Media | Borrador | Generada · DoR No cumple (IMP-021, `DEC-010`) | `DEC-009`, `DEC-027` (reúne HU-37) |
| HU-38 | Reportes de retraso y tiempos de cierre de tareas (solo pantalla, bajo demanda) | Administrador | HU-15, HU-19 | Media | Borrador | Generada · DoR No cumple (IMP-021, `DEC-010`) | `DEC-009`, `DEC-027` (reúne HU-39) |
| HU-31 | Crear equipo (nombre + Coordinador inicial) | Administrador | — | Alta | Borrador | Generada · DoR No cumple (`DEC-010`) | `DEC-003`, `DEC-014(a)` |
| HU-32 | Asignar / reasignar coordinador de un equipo | Administrador | HU-31, HU-41 | Alta | Borrador | Generada · DoR No cumple (`DEC-010`) | `DEC-003(c)`, `DEC-014(d)` |
| HU-33 | Reasignar colaborador a otro equipo (la asignación inicial ocurre en HU-08) | Administrador | HU-31, HU-41 | Media | Borrador | Generada · DoR No cumple (IMP-022, `DEC-010`) | `DEC-014(b,c)` |
| HU-41 | Consultar / listar equipos y sus miembros | Administrador | HU-31 | Media | Borrador | Generada · DoR No cumple (`DEC-010`) | `DEC-028` (hueco de cobertura). Habilita HU-32/HU-33 |

### Diferidas / fuera de ruta crítica

| ID | Nombre | Actor | Dependencias | Prioridad | Estado | Decisiones |
|---|---|---|---|---|---|---|
| HU-34 | Configurar y guardar reporte a partir de plantilla predefinida | Administrador | HU-29 | Baja (diferida) | Con pendientes | `DEC-013`, `DEC-016`, **`DEC-023-bis` (`DEC-027`)**: `POSIBLE AMPLIACIÓN DE ALCANCE` — se difiere a "si sobra tiempo" |
| HU-36 | Gestionar alertas de desempeño (distinta de recibir la alerta de retraso, HU-24) | Coordinador | HU-24 | Baja (diferida) | Con pendientes | `DEC-021`, `IMP-013` (mecanismo y módulo M3/M4 sin definir). Fuera de la ruta crítica hasta resolver `IMP-013` |

---

## Parte C — Registro de identificadores no activos (trazabilidad)

Conservados por la regla de identificadores inmutables; no se reutilizan.

| ID | Estado | Motivo | Decisión |
|---|---|---|---|
| HU-09 | Descartada | Descartada pese a figurar `Completada` en el backlog oficial; discrepancia abierta (`IMP-012`). | `DEC-019` |
| HU-11 | Fusionada en HU-10 | Restablecer contraseña es una acción de la administración de un usuario existente; sin flujo independiente que justifique HU propia. | `DEC-024` |
| HU-22 | Fusionada en HU-21 | Mismo mecanismo de notificación in-app, dos eventos, sin reglas propias. | `DEC-018(a)` |
| HU-23 | Fusionada en HU-24 | Misma mecánica de alerta temporal de tarea; se unifican "próxima a vencer" y "retrasada" con escenarios de aceptación distintos. | `DEC-025` |
| HU-26 | Fusionada en HU-25 | Los filtros no tienen valor funcional independiente sin la vista base. | `DEC-018(b)` |
| HU-30 | Fusionada en HU-25 | Panel del Administrador es la misma vista con alcance de datos distinto. | `DEC-006` |
| HU-35 | Fusionada en HU-28 | Configuración de la única organización (zona horaria + notificaciones) tratada como una sola HU a esta escala. Reabre `DEC-018(c)`. | `DEC-026` |
| HU-37 | Fusionada en HU-29 | Reportes de productividad y cumplimiento comparten fuente y patrón; se cubren con escenarios de aceptación distintos en una HU. Reabre `DEC-023`. | `DEC-027` |
| HU-39 | Fusionada en HU-38 | Reportes de retraso y tiempos de cierre comparten fuente y patrón. Reabre `DEC-023`. | `DEC-027` |

> Siguiente identificador disponible: **HU-42**. (HU-40 y HU-41 se asignaron a las
> HU faltantes de esta consolidación.)

---

## Parte D — Decisiones que no crean HU pero condicionan el backlog

| Decisión | Efecto sobre el inventario |
|---|---|
| `DEC-002` | Mono-organización por despliegue: **no** hay HU de "alta de organización"; HU-28 configura la única organización. |
| `DEC-004` | Multi-rol por usuario: afecta el diseño de autorización (M1) y HU-10 (rol como conjunto), sin crear HU propia. |
| `DEC-010` | Validación de token/rol en servidor (deuda ADR-10) = RNF de seguridad **bloqueante** para pasar HU de M2–M5 a `Lista para desarrollo`. |
| `DEC-017` | Reconstrucción documental de HU-01..HU-08 en Fase 6 (no cambia estado de desarrollo). |
| `DEC-028` (`IMP-014`) | "Carga de trabajo por responsable" queda cubierta en HU-27; **no** se crea un reporte separado en M5. |

---

## Parte E — Resumen

- HU existentes M1: HU-01..HU-08 `Completada` (reconstrucción documental pendiente,
  `DEC-017`); HU-09 `Descartada` (`DEC-019`).
- HU nuevas activas (Borrador): HU-10, HU-12..HU-21, HU-24, HU-25, HU-27, HU-28,
  HU-29, HU-31, HU-32, HU-33, HU-38, HU-40, HU-41 = **22 HU activas**, de las cuales
  **2 diferidas** (HU-34, HU-36) → **20 en ruta crítica**.
- Identificadores no activos: HU-09 (Descartada); HU-11, HU-22, HU-23, HU-26,
  HU-30, HU-35, HU-37, HU-39 (Fusionadas).
- Último identificador usado: HU-41. Siguiente disponible: HU-42.
- Reducción por consolidación: de 30 HU nuevas activas (v3.0) a **20 en ruta
  crítica** + 2 diferidas, backlog realizable para 2 personas en el alcance académico.
- Bloqueo transversal `DEC-010`: ninguna HU de M2–M5 pasa a `Lista para desarrollo`
  hasta resolver la validación de token/rol en el servidor.

> Próximo paso: aprobación del inventario consolidado y de las prioridades. En
> Fase 6 se reconstruyen HU-01..HU-08 y se desarrollan las HU activas cuyos
> pendientes bloqueantes estén resueltos.
