# HU-20 — Consultar historial y trazabilidad de una tarea

| Campo | Valor |
|---|---|
| Identificador | HU-20 |
| Nombre | Consultar historial y trazabilidad de una tarea |
| Módulo | M2 — Gestión de tareas |
| Actor | Coordinador / Administrador |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §4, §6) + DEC (`DEC-022`, `DEC-006`, `DEC-001`, `DEC-015`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-12 (tarea creada), HU-15 (cambios de estado), HU-17 (reasignaciones) |

> HU nueva en `Borrador`. `DEC-022` la amplía a Coordinador y Administrador con
> alcance de datos por rol (como `DEC-006`). Lo no definido se marca `Pendiente por
> definir`.

---

## Resumen ágil

Como **Coordinador o Administrador** necesito **consultar el historial y la
trazabilidad de una tarea**, para **entender su evolución (estados, responsables,
cambios) y sustentar el seguimiento**.

## Contexto funcional

El sistema registra el histórico de cambios de cada tarea (RT-03): cambios de
estado (HU-15), reasignaciones con el responsable anterior (HU-17, `DEC-001`),
cancelación (HU-18) y aprobación/cierre (HU-19). Esta HU expone la consulta de ese
histórico. Por `DEC-022`, el alcance de datos depende del rol: el Coordinador ve el
histórico de las tareas de su equipo (`DEC-003`); el Administrador, el de todas las
tareas de la organización (`DEC-002`). Módulo M2; resultado: historial consultable
de una tarea. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Entradas de histórico | Listado (solo lectura) | N/A | — | — | Eventos de la tarea | No | Registro cronológico de cambios. Qué campos exactos se registran (evento, actor, fecha/hora, valores anteriores/nuevos): Pendiente por definir (alcance de auditoría RT-03). |

> El alcance exacto de la auditoría (qué eventos y campos se registran, actor,
> marca temporal, visibilidad) está `Pendiente por definir` (RT-03 / pendiente de
> auditoría).

---

# Validaciones funcionales

## VF-01. Alcance de datos por rol

**Qué se valida:** que el usuario solo vea el histórico dentro de su alcance
(Coordinador: su equipo; Administrador: toda la organización).
**Cuándo:** al consultar.
**Si cumple:** muestra el histórico permitido.
**Si no cumple:** no muestra tareas fuera de su alcance.

---

# Reglas de negocio

## RN-01. Alcance de datos por rol

El alcance del histórico depende del rol, con el mismo mecanismo de `DEC-006`:
Coordinador = su equipo; Administrador = toda la organización.
Origen: DEC (`DEC-022`, `DEC-006`).

## RN-02. Contenido mínimo del histórico

El histórico conserva, al menos, el registro del responsable anterior en las
reasignaciones (`DEC-001`). El resto de eventos/campos: Pendiente por definir.
Origen: DEC (`DEC-001`) / RT-03.

---

# Reglas de comportamiento

## RC-01. Orden cronológico

Las entradas del histórico se presentan en orden cronológico. Detalle (ascendente/
descendente, paginación): Pendiente por definir.

---

# Criterios de aceptación

## CA-01. Consulta dentro del alcance

Cuando un Coordinador consulta el histórico, el sistema muestra el de las tareas de
su equipo; cuando lo consulta un Administrador, el de todas las tareas de la
organización.

## CA-02. Registro de reasignaciones

Cuando una tarea fue reasignada, el histórico muestra el responsable anterior y el
cambio (consistente con HU-17).

---

# Escenarios de prueba

## CP-01 — Histórico con reasignación

**Dado que** una tarea fue reasignada de A a B
**Cuando** el Coordinador de su equipo consulta el histórico
**Entonces** el sistema muestra el cambio de responsable de A a B.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Consulta fuera del alcance por rol | No muestra tareas fuera del alcance permitido. |
| FA-02 | Tarea inexistente | Pendiente por definir. |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Trazabilidad: integridad del histórico (RT-03; ACID según M2). |
| RNF-02 | Seguridad: alcance de datos garantizado en el servidor (validación de token/rol pendiente — `DEC-010`). |

---

# Dependencias

- HU-12 (tarea), HU-15 (cambios de estado), HU-17 (reasignaciones), HU-18/HU-19
  (cancelación/cierre).
- `DEC-003` (equipo, para el alcance del Coordinador).

---

# Aclaraciones

- Dependencia ajustada por `DEC-027`: HU-20 depende también de HU-15 y HU-17 (deben
  existir cambios que historiar), no solo de HU-12.
- El alcance de auditoría (qué se registra exactamente) es un pendiente transversal
  de RT-03; se documenta aquí sin inventar los campos.

---

# Fuera de alcance

- Exportación del histórico (no documentada; los reportes viven en M5).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Alcance exacto de la auditoría/histórico: qué eventos y campos se registran,
   actor, marca temporal y visibilidad (RT-03; pendiente de auditoría).
2. Autorización/alcance por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-20-historial-trazabilidad.svg` — línea de
tiempo del histórico con alcance por rol (`DEC-022`); contenido exacto de cada
entrada marcado como Pendiente (auditoría RT-03). El diseño visual final es decisión
de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Contenido exacto del histórico Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos.
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Alcance de auditoría; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010` y por el
alcance de auditoría no definido (RT-03).

**Pendientes:** 2 importantes.
