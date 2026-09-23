# HU-19 — Aprobar / cerrar tarea

| Campo | Valor |
|---|---|
| Identificador | HU-19 |
| Nombre | Aprobar / cerrar tarea (obligatoria para toda tarea completada) |
| Módulo | M2 — Gestión de tareas |
| Actor | Coordinador de equipo |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.2) + DEC (`DEC-005`, `DEC-007`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-15 (tarea en estado `Completada`) |

> HU nueva en `Borrador`. `DEC-007` la vuelve un paso obligatorio del ciclo de vida
> (deja de ser "cuando aplique"). Lo no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Coordinador de equipo** necesito **aprobar y cerrar una tarea que el
Colaborador marcó como completada**, para **dar por finalizado su ciclo de vida de
forma controlada**.

## Contexto funcional

Cuando el Colaborador lleva una tarea a `Completada` (HU-15), el Coordinador debe
aprobarla explícitamente para pasarla a `Cerrada`. Por `DEC-007`, esta aprobación
es obligatoria para toda tarea completada: no existe cierre automático. `Completada`
y `Cerrada` son estados independientes (`DEC-005`). El cambio se registra en el
histórico (HU-20). Módulo M2; actor: Coordinador; resultado: tarea en estado
`Cerrada`. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Acción de aprobación/cierre | Acción | Sí | — | — | Aprobar y cerrar | N/A | Transición `Completada` → `Cerrada`. |
| ¿Rechazo/devolución? | Pendiente por definir | Pendiente por definir | — | — | Pendiente por definir | N/A | Si el Coordinador puede rechazar y devolver la tarea (y a qué estado) no está definido. |

---

# Validaciones funcionales

## VF-01. Estado de origen Completada

**Qué se valida:** que la tarea esté en `Completada` antes de cerrarla.
**Cuándo:** al aprobar/cerrar.
**Si cumple:** la tarea pasa a `Cerrada`.
**Si no cumple:** se rechaza (solo se cierra desde `Completada`).

---

# Reglas de negocio

## RN-01. Aprobación obligatoria para cerrar

Toda tarea requiere aprobación explícita del Coordinador para pasar de `Completada`
a `Cerrada`; no hay cierre automático.
Origen: DEC (`DEC-007`).

## RN-02. Cierre exclusivo del Coordinador

El Colaborador no puede cerrar la tarea; el cierre lo realiza el Coordinador.
Origen: DEC (`DEC-007`).

## RN-03. Estados independientes

`Completada` y `Cerrada` son estados independientes del conjunto de `DEC-005`.
Origen: DEC (`DEC-005`).

---

# Reglas de comportamiento

## RC-01. Registro en histórico

La aprobación/cierre se registra en el histórico de la tarea (HU-20).

---

# Criterios de aceptación

## CA-01. Cierre tras aprobación

Cuando el Coordinador aprueba una tarea en estado `Completada`, el sistema la pasa a
`Cerrada` y lo registra en el histórico.

## CA-02. No cierre desde otro estado

Cuando la tarea no está `Completada`, el sistema no permite cerrarla.

## CA-03. El Colaborador no cierra

Cuando quien intenta cerrar no es el Coordinador, el sistema lo impide (consistente
con HU-15, RN-02).

---

# Escenarios de prueba

## CP-01 — Aprobación y cierre

**Dado que** una tarea está `Completada`
**Cuando** el Coordinador la aprueba
**Entonces** el sistema la marca `Cerrada` y lo registra en el histórico.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Tarea no `Completada` | No permite cerrar. |
| FA-02 | Intento de cierre por el Colaborador | No permite (corresponde al Coordinador). |
| FA-03 | ¿Rechazo/devolución de la tarea? | Pendiente por definir (¿a qué estado vuelve?). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Trazabilidad: la aprobación/cierre se registra en el histórico (RT-03, HU-20). |
| RNF-02 | Seguridad: autorización por rol de Coordinador (validación en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-15 (tarea en `Completada`), HU-20 (histórico).

---

# Aclaraciones

- `IMP-002` (`PENDIENTES.md`): `DEC-007` fija que la aprobación es obligatoria, pero
  el detalle de qué valida el Coordinador al aprobar (y si puede rechazar/devolver)
  sigue abierto.

---

# Fuera de alcance

- Reapertura de una tarea `Cerrada` (`Cerrada` es definitiva, `DEC-015(b)`).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Qué valida el Coordinador al aprobar? ¿Puede rechazar/devolver la tarea y a qué
   estado vuelve? (`IMP-002`).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-19-aprobar-cerrar-tarea.svg` — aprobación
obligatoria Completada→Cerrada (`DEC-007`); rechazo/devolución marcado como Pendiente
(IMP-002). El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (Rechazo/devolución Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Rechazo/devolución y mensajería pendientes.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (`IMP-002`; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendiente
`IMP-002` (condición de aprobación / rechazo).

**Pendientes:** 2 importantes.
