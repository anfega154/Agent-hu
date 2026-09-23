# HU-13 — Asignar tarea a un responsable

| Campo | Valor |
|---|---|
| Identificador | HU-13 |
| Nombre | Asignar tarea a un responsable |
| Módulo | M2 — Gestión de tareas |
| Actor | Coordinador de equipo |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §4, §6) + DEC (`DEC-001`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-12 (tarea creada); HU-08/HU-41 (responsables disponibles) |

> HU nueva en `Borrador`. `DEC-001` reduce el alcance del texto oficial ("uno o
> varios colaboradores") a **responsable único**. Lo no definido se marca
> `Pendiente por definir`.

---

## Resumen ágil

Como **Coordinador de equipo** necesito **asignar una tarea a un responsable
(Colaborador)**, para **que quede claro quién debe ejecutarla y poder darle
seguimiento**.

## Contexto funcional

Una vez creada la tarea (HU-12), el Coordinador la asigna a un único responsable.
Por `DEC-001`, una tarea tiene un solo responsable en cada momento; el cambio de
responsable se hace mediante reasignación (HU-17), no mediante asignación múltiple.
La asignación dispara una notificación al Colaborador (HU-21). Módulo M2; actor:
Coordinador; resultado: tarea con responsable asignado. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Responsable | Selección (un único Colaborador) | Sí | — | Un responsable | Colaboradores disponibles | Sí | Único responsable de la tarea (`DEC-001`). Alcance de candidatos (¿solo colaboradores del equipo del Coordinador?): Pendiente por definir. |

---

# Validaciones funcionales

## VF-01. Responsable único válido

**Qué se valida:** que se seleccione exactamente un Colaborador válido como responsable.
**Cuándo:** al asignar.
**Si cumple:** la tarea queda asignada a ese responsable.
**Si no cumple:** Pendiente por definir (mensaje/comportamiento no definido).

---

# Reglas de negocio

## RN-01. Responsable único

Una tarea tiene un único responsable en cada momento; no se admite asignación
simultánea a varios colaboradores. El cambio de responsable se realiza por
reasignación (HU-17).
Origen: DEC (`DEC-001`).

## RN-02. Asignador autorizado

Asignar tareas es una función del Coordinador de equipo.
Origen: DOC (§5.2).

---

# Reglas de comportamiento

## RC-01. Notificación de asignación

Al asignarse la tarea, el sistema notifica al Colaborador responsable (HU-21).

---

# Criterios de aceptación

## CA-01. Asignación exitosa

Cuando el Coordinador selecciona un Colaborador válido y confirma, el sistema
registra a ese Colaborador como único responsable de la tarea y dispara la
notificación de asignación (HU-21).

## CA-02. Responsable único

Cuando ya existe un responsable, asignar a otro Colaborador no crea un segundo
responsable: se trata como reasignación (HU-17). El detalle de si esta operación
está disponible desde "asignar" o solo desde "reasignar" es Pendiente por definir.

---

# Escenarios de prueba

## CP-01 — Asignación exitosa

**Dado que** existe una tarea creada sin responsable
**Cuando** el Coordinador asigna un Colaborador válido
**Entonces** la tarea queda con ese único responsable y se notifica al Colaborador.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Responsable no válido / no seleccionado | Pendiente por definir. |
| FA-02 | Usuario sin permiso (no Coordinador) | Pendiente por definir; autorización por rol en servidor (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: exige autorización por rol de Coordinador (validación en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-12 (tarea creada).
- HU-08 / HU-41 (colaboradores/equipos disponibles).
- HU-21 (notificación de asignación).

---

# Aclaraciones

- `DEC-001` es una reducción consciente del alcance del texto oficial ("uno o
  varios colaboradores") a responsable único, con historial en la reasignación.
- El conjunto de candidatos a responsable (¿todos los colaboradores, o solo los del
  equipo del Coordinador?) no está definido; se relaciona con `DEC-003` (Equipo).

---

# Fuera de alcance

- Reasignación de la tarea (HU-17).
- Asignación múltiple (excluida por `DEC-001`).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Qué colaboradores pueden ser responsables: cualquiera de la organización o solo
   los del equipo del Coordinador? (relacionado con `DEC-003`; nuevo IMP-018).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-13-asignar-tarea.svg` — el alcance de
candidatos a responsable está marcado como Pendiente (IMP-018). El diseño visual
final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (Alcance de candidatos pendiente.)
- [x] Validaciones definidas. (Mensajería pendiente.)
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Alcance de candidatos; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendientes
importantes (alcance de candidatos, mensajería).

**Pendientes:** 2 importantes.
