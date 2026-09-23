# HU-12 — Crear tarea (título, descripción, fechas)

| Campo | Valor |
|---|---|
| Identificador | HU-12 |
| Nombre | Crear tarea (título, descripción, fechas) |
| Módulo | M2 — Gestión de tareas |
| Actor | Coordinador de equipo |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §4, §6) |
| Última actualización | 2026-09-21 |
| Dependencias | M1 (autenticación/RBAC), HU-08 (usuarios), HU-31 (equipos) |

> HU nueva en `Borrador`. Contenido derivado del documento oficial y de las
> decisiones aplicables. Los detalles no definidos se marcan `Pendiente por
> definir`, sin inventar. Bloqueo transversal `DEC-010` aplica al DoR.

---

## Resumen ágil

Como **Coordinador de equipo** necesito **crear una tarea con su información
básica (título, descripción, fechas)**, para **iniciar el ciclo de vida de la
tarea y poder asignarla y darle seguimiento**.

## Contexto funcional

La creación de la tarea es el punto de partida del ciclo de vida gestionado por
COMPIRA (creación → asignación → ejecución → seguimiento → cierre → histórico). El
Coordinador define la información inicial de la tarea. Una vez creada, podrá
asignarse a un responsable (HU-13). Módulo M2; actor: Coordinador; resultado
esperado: una tarea registrada en estado inicial `Pendiente` (`DEC-005`), lista
para ser asignada. Origen: DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Título | Texto | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Sí | Nombre de la tarea. Origen: DOC. |
| Descripción | Texto | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Sí | Detalle de la tarea. Origen: DOC. |
| Fecha(s) | Pendiente por definir (¿inicio y/o límite?) | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Sí | El documento menciona "fechas" sin precisar cuáles ni su formato. Pendiente por definir. |
| Estado inicial | Derivado (no editable) | N/A | Enum | — | `Pendiente` | No | Al crearse, la tarea queda en `Pendiente` (`DEC-005`). |

> No se inventan obligatoriedad, formatos ni longitudes de los campos: no están
> definidos en las fuentes (ver `PENDIENTES.md`).

---

# Validaciones funcionales

## VF-01. Campos obligatorios de la tarea

**Qué se valida:** presencia de los campos obligatorios de la tarea.
**Cuándo:** al crear.
**Si cumple:** se crea la tarea en estado `Pendiente`.
**Si no cumple:** Pendiente por definir (mensaje/comportamiento no definido).

> El conjunto exacto de campos obligatorios y las reglas de fecha (p. ej. fecha
> límite no anterior a hoy) están `Pendiente por definir`.

---

# Reglas de negocio

## RN-01. Estado inicial de la tarea

Toda tarea creada queda en estado `Pendiente` dentro del conjunto cerrado de
estados definido en `DEC-005`.
Origen: DEC (`DEC-005`).

## RN-02. Creador autorizado

La creación de tareas es una función del Coordinador de equipo.
Origen: DOC (§5.2).

---

# Reglas de comportamiento

## RC-01. Disponibilidad para asignación

Tras crearse, la tarea queda disponible para asignarse a un responsable (HU-13).

---

# Criterios de aceptación

## CA-01. Creación exitosa

Cuando el Coordinador diligencia los campos obligatorios de la tarea y confirma, el
sistema crea la tarea en estado `Pendiente` y la deja disponible para asignación.

## CA-02. Rechazo por campos obligatorios faltantes

Cuando falta un campo obligatorio, el sistema no crea la tarea. Mensaje y campos
exactos: Pendiente por definir.

---

# Escenarios de prueba

## CP-01 — Creación exitosa

**Dado que** el Coordinador está autenticado y en la pantalla de creación de tarea
**Cuando** diligencia los campos obligatorios y confirma
**Entonces** el sistema crea la tarea en estado `Pendiente`.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Campos obligatorios faltantes | No crea la tarea; comportamiento/mensaje Pendiente por definir (MEN-001). |
| FA-02 | Usuario sin permiso (no Coordinador) | Pendiente por definir; ligado a la autorización por rol en servidor (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: la operación exige autenticación y autorización por rol de Coordinador (validación en servidor pendiente — `DEC-010`). |

---

# Dependencias

- M1 (autenticación y control de acceso por rol).
- HU-08 (usuarios existentes) y HU-31 (equipos) para el contexto de la tarea.

---

# Aclaraciones

- El documento oficial indica "creación de tareas (título, descripción, fechas)"
  sin precisar cuáles fechas, obligatoriedad ni formatos; se documentan como
  Pendiente por definir.
- Dependencia ajustada por `DEC-027`: HU-12 depende de M1 + HU-08 + HU-31, no solo
  del login.

---

# Fuera de alcance

- Asignación de la tarea a un responsable (HU-13).
- Estados posteriores y transiciones (HU-15).

---

# Preguntas pendientes

## Bloqueantes

1. ¿Qué campos son obligatorios y qué reglas de fecha aplican (¿fecha de inicio y
   fecha límite?, ¿formato?, ¿fecha límite no anterior a hoy?)? (`PENDIENTES.md`:
   nuevo IMP-017).

## Importantes

1. Autorización por rol en el servidor para crear tareas (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder; contenido sujeto a IMP-017).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-12-crear-tarea.svg` — esquema del formulario;
los campos y reglas de fecha están marcados como Pendiente por definir (IMP-017). El
diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Obligatoriedad, formatos y fechas Pendiente por definir.)
- [ ] Validaciones definidas. (Dependen de los campos.)
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables. (Los definidos; faltan los de campos.)
- [x] Dependencias identificadas.
- [ ] Sin preguntas bloqueantes. (Campos/fechas de la tarea.)
- [ ] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** No cumple. Bloqueado por definición de campos/fechas (IMP-017) y
por el RNF de seguridad `DEC-010`.

**Pendientes:** 1 bloqueante (campos/fechas), 1 importante (autorización por rol).
