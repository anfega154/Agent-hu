# HU-40 — Consultar / listar usuarios de la organización

| Campo | Valor |
|---|---|
| Identificador | HU-40 |
| Nombre | Consultar / listar usuarios de la organización |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DEC (`DEC-028`) + DOC (`compira-context.md` §5.1) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-08 (usuarios creados) |

> HU nueva creada en la consolidación (`DEC-028`) para cerrar un hueco: no había
> forma de listar/seleccionar usuarios para administrarlos. Lo no definido se marca
> `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **consultar la lista de usuarios de la
organización**, para **localizar y seleccionar el usuario que voy a administrar
(editar, cambiar rol, restablecer contraseña)**.

## Contexto funcional

La administración de usuarios (editar/rol/reset en HU-10) presupone poder ver y
seleccionar usuarios. Esta HU expone la consulta del directorio de usuarios de la
única organización (mono-organización, `DEC-002`). Es una capacidad habilitadora de
HU-10. Módulo M5; actor: Administrador; resultado: listado de usuarios consultable.
Origen: DEC / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Listado de usuarios | Tabla/listado (solo lectura) | N/A | — | — | Usuarios de la organización | No | Columnas exactas: Pendiente por definir (candidatas: nombre, apellido, correo, rol(es), equipo). |
| Búsqueda / filtro | Control de búsqueda | No | Pendiente por definir | — | Pendiente por definir | Sí | ¿Búsqueda por correo/nombre?, ¿filtro por rol/equipo? Pendiente por definir. |

> Columnas, búsqueda, filtros, orden y paginación: Pendiente por definir (MEN-001 /
> detalle de listado).

---

# Validaciones funcionales

## VF-01. Acceso exclusivo del Administrador

**Qué se valida:** que solo el Administrador acceda al listado de usuarios.
**Cuándo:** al abrir la vista.
**Si cumple:** muestra el listado.
**Si no cumple:** Pendiente por definir; autorización por rol en servidor (`DEC-010`).

---

# Reglas de negocio

## RN-01. Alcance de la organización única

El listado abarca los usuarios de la única organización de la instancia
(mono-organización).
Origen: DEC (`DEC-002`).

## RN-02. Gestor autorizado

La gestión de usuarios es función exclusiva del Administrador.
Origen: DOC (§5.1).

---

# Reglas de comportamiento

## RC-01. Selección para administrar

Desde el listado, el Administrador puede seleccionar un usuario para administrarlo
(HU-10). Mecanismo exacto: Pendiente por definir.

---

# Criterios de aceptación

## CA-01. Consulta del listado

Cuando el Administrador abre la vista de usuarios, el sistema muestra los usuarios
de la organización.

## CA-02. Selección de un usuario

Cuando el Administrador selecciona un usuario del listado, el sistema permite
proceder a administrarlo (HU-10). Detalle Pendiente por definir.

---

# Escenarios de prueba

## CP-01 — Consulta de usuarios

**Dado que** el Administrador está autenticado
**Cuando** abre la vista de usuarios
**Entonces** el sistema muestra la lista de usuarios de la organización.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Usuario no Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |
| FA-02 | Organización sin usuarios (salvo el propio admin) | Estado vacío (detalle Pendiente por definir). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: acceso restringido al Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-08 (usuarios creados).

---

# Aclaraciones

- HU nueva de `DEC-028` para cerrar el hueco de cobertura de M5 (no había HU de
  listado de usuarios). Habilita HU-10.
- Las columnas y capacidades de búsqueda/filtro no están definidas; se dejan
  Pendiente por definir sin inventar.

---

# Fuera de alcance

- Edición, cambio de rol o restablecimiento de contraseña de un usuario (HU-10).
- Eliminación de usuarios (HU-09, `Descartada`).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Columnas del listado, búsqueda/filtros, orden y paginación (MEN-001 / detalle de
   listado).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-40-listar-usuarios.svg` — listado con acción
"Editar → HU-10"; columnas y búsqueda marcadas como Pendiente (MEN-001). El diseño
visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Columnas/búsqueda Pendiente por definir.)
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
