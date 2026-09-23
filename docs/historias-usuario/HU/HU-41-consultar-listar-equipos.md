# HU-41 — Consultar / listar equipos y sus miembros

| Campo | Valor |
|---|---|
| Identificador | HU-41 |
| Nombre | Consultar / listar equipos y sus miembros |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DEC (`DEC-028`, `DEC-003`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-31 (equipos creados) |

> HU nueva creada en la consolidación (`DEC-028`) para cerrar un hueco: no había
> forma de listar equipos y miembros para gestionarlos. Habilita HU-32 y HU-33. Lo
> no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **consultar los equipos y sus miembros**,
para **localizar y seleccionar el equipo sobre el que voy a actuar (reasignar
coordinador o mover colaboradores)**.

## Contexto funcional

La gestión de equipos (reasignar coordinador HU-32, reasignar colaborador HU-33)
presupone poder ver los equipos y su composición. Esta HU expone la consulta de los
equipos de la única organización (`DEC-002`), con su Coordinador vigente
(`DEC-003(b)`) y sus colaboradores (`DEC-003(a)`). Módulo M5; actor: Administrador;
resultado: listado de equipos y miembros consultable. Origen: DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Listado de equipos | Tabla/listado (solo lectura) | N/A | — | — | Equipos de la organización | No | Nombre del equipo y su Coordinador vigente. Columnas exactas: Pendiente por definir. |
| Miembros del equipo | Detalle (solo lectura) | N/A | — | — | Colaboradores del equipo | No | Composición del equipo. Detalle de presentación: Pendiente por definir. |

---

# Validaciones funcionales

## VF-01. Acceso exclusivo del Administrador

**Qué se valida:** que solo el Administrador consulte equipos y miembros.
**Cuándo:** al abrir la vista.
**Si cumple:** muestra el listado.
**Si no cumple:** Pendiente por definir; autorización por rol (`DEC-010`).

---

# Reglas de negocio

## RN-01. Alcance de la organización única

El listado abarca los equipos de la única organización (mono-organización).
Origen: DEC (`DEC-002`).

## RN-02. Composición del equipo

Cada equipo muestra su Coordinador vigente único (`DEC-003(b)`) y sus colaboradores
(cada colaborador pertenece a un único equipo, `DEC-003(a)`).
Origen: DEC (`DEC-003`).

---

# Reglas de comportamiento

## RC-01. Selección para gestionar

Desde el listado, el Administrador puede seleccionar un equipo para reasignar su
coordinador (HU-32) o mover colaboradores (HU-33). Mecanismo exacto: Pendiente por
definir.

---

# Criterios de aceptación

## CA-01. Consulta de equipos y miembros

Cuando el Administrador abre la vista de equipos, el sistema muestra los equipos de
la organización con su Coordinador y sus colaboradores.

## CA-02. Selección de un equipo

Cuando el Administrador selecciona un equipo, el sistema permite proceder a
gestionarlo (HU-32/HU-33). Detalle Pendiente por definir.

---

# Escenarios de prueba

## CP-01 — Consulta de equipos

**Dado que** existen equipos creados
**Cuando** el Administrador abre la vista de equipos
**Entonces** el sistema muestra cada equipo con su Coordinador y sus colaboradores.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | No hay equipos creados | Estado vacío (detalle Pendiente por definir). |
| FA-02 | Usuario no Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: acceso restringido al Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-31 (equipos creados).

---

# Aclaraciones

- HU nueva de `DEC-028` para cerrar el hueco de cobertura de M5. Habilita HU-32 y
  HU-33.

---

# Fuera de alcance

- Reasignación de coordinador (HU-32) y de colaboradores (HU-33).
- Creación de equipos (HU-31).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Columnas del listado, presentación de miembros, orden y paginación (MEN-001 /
   detalle de listado).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-41-listar-equipos.svg` — equipos con
coordinador y colaboradores, acción "Gestionar → HU-32/HU-33"; presentación de
miembros marcada como Pendiente. El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Columnas/presentación Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos.
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Detalle de listado; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendiente el
detalle del listado.

**Pendientes:** 2 importantes.
