# HU-17 — Reasignar tarea (cambio de responsable con histórico)

| Campo | Valor |
|---|---|
| Identificador | HU-17 |
| Nombre | Reasignar tarea (cambio de responsable con histórico; conserva estado previo) |
| Módulo | M2 — Gestión de tareas |
| Actor | Coordinador de equipo |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §4, §5.2) + DEC (`DEC-001`, `DEC-005`, `DEC-015`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-13 (tarea con responsable) |

> HU nueva en `Borrador`. Es la HU central del cambio de responsable (`DEC-001`).
> Lo no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Coordinador de equipo** necesito **reasignar una tarea a otro responsable
conservando el registro de lo realizado por el anterior**, para **redistribuir el
trabajo sin perder trazabilidad**.

## Contexto funcional

Como una tarea tiene un único responsable en cada momento (`DEC-001`), el cambio de
responsable se realiza mediante reasignación. Al reasignar, la tarea pasa por el
instante de transición `En reasignación` (`DEC-015(a)`) y, al completarse, conserva
el estado que tenía antes (p. ej. si estaba `Retrasada`, sigue `Retrasada` con el
nuevo responsable). El histórico (HU-20) debe conservar el registro del responsable
anterior y de lo que realizó. La reasignación dispara una notificación (HU-21).
Módulo M2; actor: Coordinador; resultado: tarea con nuevo responsable e histórico
actualizado. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Nuevo responsable | Selección (un único Colaborador) | Sí | — | Un responsable | Colaboradores disponibles | Sí | Reemplaza al responsable anterior (`DEC-001`). Alcance de candidatos: Pendiente por definir (relacionado con `DEC-003`). |

---

# Validaciones funcionales

## VF-01. Nuevo responsable válido y distinto

**Qué se valida:** que se seleccione un Colaborador válido como nuevo responsable.
**Cuándo:** al reasignar.
**Si cumple:** la tarea queda con el nuevo responsable, conservando su estado previo.
**Si no cumple:** Pendiente por definir.

## VF-02. Reasignación no permitida en estado terminal

**Qué se valida:** que la tarea no esté en un estado que impida reasignar.
**Cuándo:** al reasignar.
**Si cumple:** procede.
**Si no cumple:** se rechaza. Los estados exactos que impiden reasignar son
Pendiente por definir (`DEC-015` define `Cerrada` como definitiva; el resto: por
confirmar).

---

# Reglas de negocio

## RN-01. Responsable único con reasignación

El cambio de responsable se hace por reasignación; no hay responsables simultáneos
(`DEC-001`).
Origen: DEC (`DEC-001`).

## RN-02. Conservación del estado previo

Tras la reasignación, la tarea conserva el estado que tenía antes de entrar a
`En reasignación` (`DEC-015(a)`).
Origen: DEC (`DEC-015(a)`).

## RN-03. Histórico del responsable anterior

El histórico debe conservar el registro del responsable anterior y de lo que
realizó antes de la reasignación.
Origen: DEC (`DEC-001`).

---

# Reglas de comportamiento

## RC-01. Transición En reasignación

La reasignación dispara el instante de transición `En reasignación` (evento de
auditoría), no un estado de reposo (`DEC-015(a)`).

## RC-02. Notificación de reasignación

Al reasignar, el sistema notifica al nuevo responsable (HU-21).

---

# Criterios de aceptación

## CA-01. Reasignación exitosa

Cuando el Coordinador selecciona un nuevo responsable válido y confirma, el sistema
cambia el responsable, conserva el estado previo de la tarea, registra el cambio en
el histórico (con el responsable anterior) y notifica al nuevo responsable.

## CA-02. Conservación del estado

Cuando la tarea estaba, por ejemplo, `Retrasada`, tras la reasignación sigue
`Retrasada` con el nuevo responsable.

## CA-03. Registro del responsable anterior

Cuando se reasigna, el histórico conserva quién era el responsable anterior y lo
realizado por él antes del cambio.

---

# Escenarios de prueba

## CP-01 — Reasignación conservando estado

**Dado que** una tarea `Retrasada` tiene responsable A
**Cuando** el Coordinador la reasigna al responsable B
**Entonces** la tarea queda con responsable B, sigue `Retrasada`, el histórico
registra a A y se notifica a B.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Nuevo responsable no válido | Pendiente por definir. |
| FA-02 | Tarea en estado que impide reasignar | Rechazo; estados exactos Pendiente por definir (`Cerrada` es definitiva, `DEC-015`). |
| FA-03 | Usuario sin permiso (no Coordinador) | Pendiente por definir; autorización (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Trazabilidad: registro del responsable anterior y del cambio en el histórico (RT-03, HU-20). |
| RNF-02 | Seguridad: autorización por rol de Coordinador (validación en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-13 (tarea con responsable).
- HU-20 (histórico), HU-21 (notificación), HU-15 (modelo de estados).

---

# Aclaraciones

- `En reasignación` es un instante de transición/evento de auditoría, no un estado
  de reposo (`DEC-015(a)`).
- El alcance de candidatos a nuevo responsable se relaciona con `DEC-003` (Equipo)
  y está Pendiente por definir (mismo IMP que HU-13).

---

# Fuera de alcance

- Asignación inicial (HU-13).
- Cancelación (HU-18) y cierre (HU-19).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Estados desde los cuales se permite reasignar (más allá de `Cerrada` definitiva).
2. Alcance de candidatos a nuevo responsable (relacionado con `DEC-003`; mismo
   pendiente que HU-13).
3. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-17-reasignar-tarea.svg` — conserva estado
previo (`DEC-015a`) e histórico del responsable anterior; candidatos y estados que
permiten reasignar marcados como Pendiente (IMP-018). El diseño visual final es
decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (Alcance de candidatos pendiente.)
- [x] Validaciones definidas. (Estados que impiden reasignar parcialmente pendientes.)
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Estados que permiten reasignar; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendientes
importantes de estados que permiten reasignar y alcance de candidatos.

**Pendientes:** 3 importantes.
