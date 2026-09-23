# HU-33 — Reasignar colaborador a otro equipo

| Campo | Valor |
|---|---|
| Identificador | HU-33 |
| Nombre | Reasignar colaborador a otro equipo (la asignación inicial ocurre en HU-08) |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DEC (`DEC-003`, `DEC-014`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-31 (equipos), HU-41 (listar/seleccionar), HU-08 (asignación inicial) |

> HU nueva en `Borrador`. `DEC-014` la redefine: solo **reasignar** (la asignación
> inicial ocurre al crear el usuario, HU-08). Lo no definido se marca `Pendiente por
> definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **reasignar un colaborador a otro
equipo**, para **reorganizar la composición de los equipos cuando cambian las
necesidades**.

## Contexto funcional

Un colaborador pertenece a exactamente un equipo (`DEC-003(a)`) y debe tener siempre
un equipo asignado (`DEC-014(c)`). La asignación inicial ocurre al crear el usuario
(HU-08); esta HU cubre el **cambio** de equipo de un colaborador existente. Módulo
M5; actor: Administrador; resultado: colaborador movido a otro equipo. Origen: DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Colaborador | Selección | Sí | — | Un colaborador | Colaboradores existentes | Sí | Colaborador a reasignar. |
| Equipo destino | Selección | Sí | — | Un equipo | Equipos existentes (HU-41) | Sí | Nuevo equipo del colaborador (uno solo, `DEC-003(a)`). |

---

# Validaciones funcionales

## VF-01. Equipo destino válido y distinto

**Qué se valida:** que el equipo destino exista y sea distinto del actual.
**Cuándo:** al reasignar.
**Si cumple:** el colaborador queda en el equipo destino.
**Si no cumple:** Pendiente por definir.

## VF-02. El colaborador siempre tiene equipo

**Qué se valida:** que la reasignación no deje al colaborador sin equipo.
**Cuándo:** al reasignar.
**Si cumple:** el colaborador pasa directamente al nuevo equipo.
**Si no cumple:** no aplica (siempre hay equipo destino, `DEC-014(c)`).

---

# Reglas de negocio

## RN-01. Un colaborador, un equipo

Un colaborador pertenece a exactamente un equipo a la vez.
Origen: DEC (`DEC-003(a)`).

## RN-02. Siempre con equipo

Todo colaborador debe tener un equipo asignado siempre; no existe el estado
"colaborador sin equipo".
Origen: DEC (`DEC-014(c)`).

## RN-03. Asignación inicial en HU-08

La asignación inicial del colaborador a un equipo ocurre al crear su usuario; esta
HU solo cubre la reasignación posterior.
Origen: DEC (`DEC-014(b)`).

---

# Reglas de comportamiento

## RC-01. Efecto en el alcance del panel

Al cambiar de equipo, las tareas y el seguimiento del colaborador pasan al alcance
del Coordinador del nuevo equipo (`DEC-006`). El efecto sobre tareas ya asignadas
antes del cambio es Pendiente por definir.

---

# Criterios de aceptación

## CA-01. Reasignación exitosa

Cuando el Administrador selecciona un colaborador y un equipo destino válido y
confirma, el sistema mueve al colaborador a ese equipo.

## CA-02. Colaborador siempre con equipo

Cuando se reasigna, el colaborador queda directamente en el equipo destino, sin
quedar en ningún momento sin equipo.

---

# Escenarios de prueba

## CP-01 — Reasignación de colaborador

**Dado que** un colaborador pertenece al equipo A
**Cuando** el Administrador lo reasigna al equipo B
**Entonces** el colaborador queda en el equipo B.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Equipo destino inexistente o igual al actual | Pendiente por definir. |
| FA-02 | Quien reasigna no es Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |
| FA-03 | Colaborador con tareas activas al cambiar de equipo | Comportamiento sobre esas tareas: Pendiente por definir. |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: acción exclusiva del Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-31 (equipos), HU-41 (listar/seleccionar), HU-08 (asignación inicial).

---

# Aclaraciones

- `DEC-014` redefinió esta HU: solo reasignación (la asignación inicial vive en
  HU-08).
- El efecto de la reasignación sobre las tareas ya asignadas al colaborador (¿se
  mantienen?, ¿cambian de alcance de seguimiento?) no está definido.

---

# Fuera de alcance

- Asignación inicial de equipo (HU-08).
- Reasignación de coordinador (HU-32).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Efecto de la reasignación sobre las tareas ya asignadas al colaborador (nuevo
   IMP-022).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-33-reasignar-colaborador.svg` — colaborador
siempre con equipo (`DEC-014c`); efecto sobre tareas activas marcado como Pendiente
(IMP-022). El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos.
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Efecto sobre tareas activas Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Efecto sobre tareas activas; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendiente el
efecto sobre tareas activas del colaborador (IMP-022).

**Pendientes:** 2 importantes.
