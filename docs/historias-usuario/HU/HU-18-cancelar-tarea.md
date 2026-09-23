# HU-18 — Cancelar tarea

| Campo | Valor |
|---|---|
| Identificador | HU-18 |
| Nombre | Cancelar tarea (desde cualquier estado excepto `Cerrada`) |
| Módulo | M2 — Gestión de tareas |
| Actor | Coordinador de equipo |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.2) + DEC (`DEC-005`, `DEC-015`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-12 (tarea creada) |

> HU nueva en `Borrador`. Lo no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Coordinador de equipo** necesito **cancelar una tarea**, para **retirarla
del flujo de trabajo cuando ya no debe ejecutarse**.

## Contexto funcional

El Coordinador cancela una tarea que ya no debe realizarse. `Cancelada` es un
estado terminal (`DEC-005`) y puede alcanzarse desde cualquier estado excepto
`Cerrada`, que es definitiva (`DEC-015(b)`). El cambio se registra en el histórico
(HU-20). Módulo M2; actor: Coordinador; resultado: tarea en estado `Cancelada`.
Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Confirmación de cancelación | Acción | Pendiente por definir | — | — | — | N/A | ¿Requiere confirmación explícita? Pendiente por definir. |
| Motivo de cancelación | Texto | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Pendiente por definir | ¿Se exige un motivo? No definido. |

---

# Validaciones funcionales

## VF-01. Estado cancelable

**Qué se valida:** que la tarea no esté `Cerrada`.
**Cuándo:** al cancelar.
**Si cumple:** la tarea pasa a `Cancelada`.
**Si no cumple:** se rechaza (una tarea `Cerrada` no puede cancelarse, `DEC-015(b)`).

---

# Reglas de negocio

## RN-01. Cancelable excepto si está Cerrada

`Cancelada` puede alcanzarse desde cualquier estado excepto `Cerrada`.
Origen: DEC (`DEC-015(b)`).

## RN-02. Estado terminal

`Cancelada` es un estado terminal del conjunto de `DEC-005`.
Origen: DEC (`DEC-005`).

## RN-03. Cancelador autorizado

Cancelar tareas es una función del Coordinador de equipo.
Origen: DOC (§5.2).

---

# Reglas de comportamiento

## RC-01. Registro en histórico

La cancelación se registra en el histórico de la tarea (HU-20).

---

# Criterios de aceptación

## CA-01. Cancelación exitosa

Cuando el Coordinador cancela una tarea que no está `Cerrada`, el sistema la lleva
al estado `Cancelada` y lo registra en el histórico.

## CA-02. Rechazo de cancelación de tarea Cerrada

Cuando la tarea está `Cerrada`, el sistema no permite cancelarla.

---

# Escenarios de prueba

## CP-01 — Cancelación válida

**Dado que** una tarea está `En progreso`
**Cuando** el Coordinador la cancela
**Entonces** el sistema la marca `Cancelada` y lo registra en el histórico.

## CP-02 — No cancelable si Cerrada

**Dado que** una tarea está `Cerrada`
**Cuando** el Coordinador intenta cancelarla
**Entonces** el sistema lo impide.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Tarea `Cerrada` | No permite cancelar (`DEC-015(b)`). |
| FA-02 | Usuario sin permiso (no Coordinador) | Pendiente por definir; autorización (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Trazabilidad: la cancelación se registra en el histórico (RT-03, HU-20). |
| RNF-02 | Seguridad: autorización por rol de Coordinador (validación en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-12 (tarea creada), HU-15 (modelo de estados), HU-20 (histórico).

---

# Aclaraciones

- Si la cancelación requiere confirmación explícita y/o un motivo obligatorio no
  está definido; se deja Pendiente por definir sin inventar.

---

# Fuera de alcance

- Reactivación de una tarea cancelada (no documentada).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿La cancelación requiere confirmación explícita y/o motivo obligatorio?
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-18-cancelar-tarea.svg` — confirmación y motivo
marcados como Pendiente por definir; `Cerrada` no es cancelable (`DEC-015b`). El
diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Confirmación/motivo Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Confirmación/motivo; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendientes
importantes de confirmación/motivo. El comportamiento de estados está definido por
`DEC-005`/`DEC-015`.

**Pendientes:** 2 importantes.
