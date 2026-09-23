# HU-14 — Consultar tareas asignadas

| Campo | Valor |
|---|---|
| Identificador | HU-14 |
| Nombre | Consultar tareas asignadas |
| Módulo | M2 — Gestión de tareas |
| Actor | Colaborador |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.3, §6) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-13 (tareas asignadas) |

> HU nueva en `Borrador`. Lo no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Colaborador** necesito **consultar las tareas que me han sido asignadas**,
para **saber qué debo ejecutar y actualizar su estado**.

## Contexto funcional

El Colaborador accede a la lista de tareas de las que es responsable. Es la vista
operativa desde la cual ejecuta, actualiza estado (HU-15) y registra observaciones
(HU-16). Módulo M2; actor: Colaborador; resultado: lista de tareas asignadas al
usuario autenticado. Origen: DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Lista de tareas asignadas | Tabla/listado (solo lectura) | N/A | — | — | Tareas cuyo responsable es el usuario | No | Muestra las tareas asignadas al Colaborador. Columnas exactas: Pendiente por definir. |

> Columnas, orden, filtros, paginación y estados vacíos/carga: Pendiente por
> definir (MEN-001 / no documentados para M2).

---

# Validaciones funcionales

## VF-01. Alcance de datos del Colaborador

**Qué se valida:** que solo se muestren tareas cuyo responsable vigente sea el
usuario autenticado.
**Cuándo:** al cargar la lista.
**Si cumple:** muestra únicamente sus tareas.
**Si no cumple:** no aplica (regla de filtrado por responsable).

---

# Reglas de negocio

## RN-01. Alcance por responsable

El Colaborador consulta únicamente las tareas de las que es responsable vigente.
Origen: DOC (§5.3).

---

# Reglas de comportamiento

## RC-01. Estado vacío

Cuando el Colaborador no tiene tareas asignadas, la vista muestra un estado vacío.
Texto/comportamiento exacto: Pendiente por definir.

---

# Criterios de aceptación

## CA-01. Consulta de tareas propias

Cuando el Colaborador accede a la vista, el sistema muestra únicamente las tareas
cuyo responsable vigente es él.

## CA-02. Estado vacío

Cuando no tiene tareas asignadas, el sistema muestra un estado vacío (detalle
Pendiente por definir).

---

# Escenarios de prueba

## CP-01 — Consulta con tareas

**Dado que** el Colaborador tiene tareas asignadas
**Cuando** abre la vista de tareas asignadas
**Entonces** el sistema lista solo sus tareas.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Sin tareas asignadas | Estado vacío (detalle Pendiente por definir). |
| FA-02 | Sesión inválida | Redirige al login (coherente con HU-07). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: el alcance de datos debe garantizarse en el servidor (validación de token/rol pendiente — `DEC-010`). |

---

# Dependencias

- HU-13 (tareas asignadas).

---

# Aclaraciones

- Columnas, filtros, orden y paginación de la lista no están definidos en las
  fuentes; se documentan como Pendiente por definir (los filtros del panel general
  viven en HU-25, no aquí).

---

# Fuera de alcance

- Actualización de estado (HU-15) y observaciones (HU-16).
- Panel de seguimiento del equipo (HU-25, otro actor/alcance).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Columnas, orden, filtros y paginación de la lista de tareas asignadas (MEN-001 /
   IMP de detalle de listado).

---

# Prototipo

**Requerido:** Pendiente.

Descripción: Pendiente por definir.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Columnas Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Detalle de listado; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendiente
el detalle de columnas/filtros del listado.

**Pendientes:** 1 importante (detalle del listado).
