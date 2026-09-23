# HU-10 — Editar información y rol de un usuario existente (incluye restablecer contraseña)

| Campo | Valor |
|---|---|
| Identificador | HU-10 |
| Nombre | Editar información y rol de un usuario existente (incluye restablecer su contraseña) |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.1) + DEC (`DEC-004`, `DEC-024`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-08 (usuario creado), HU-40 (listar/seleccionar usuario) |

> HU nueva en `Borrador`. `DEC-024` absorbe HU-11 (restablecer contraseña) como
> acción de esta HU. `DEC-004` establece multi-rol. Lo no definido se marca
> `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **editar la información y los roles de
un usuario existente, y poder restablecer su contraseña**, para **mantener
actualizada la administración de las cuentas**.

## Contexto funcional

El Administrador selecciona un usuario (HU-40) y edita sus datos, ajusta su(s)
rol(es) o restablece su contraseña. Por `DEC-004`, un usuario puede tener más de un
rol simultáneamente, por lo que la asignación de rol es un **conjunto**, no una
selección única. El restablecimiento de contraseña (antes HU-11) es una acción
dentro de esta HU (`DEC-024`); su flujo exacto está en `IMP-001`. Módulo M5; actor:
Administrador; resultado: usuario actualizado. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Datos del usuario | Formulario | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Sí | Campos editables (¿nombre, apellido, teléfono?). ¿El correo es editable? Pendiente por definir. |
| Rol(es) | Selección múltiple | Sí | Conjunto | — | `ADMINISTRATOR`, `COORDINATOR`, `COLLABORATOR` | Sí | Un usuario puede tener varios roles (`DEC-004`). |
| Restablecer contraseña | Acción | No | — | — | — | N/A | Acción del Administrador (`DEC-024`). Flujo exacto: Pendiente por definir (`IMP-001`). |

> Qué campos son editables (y si el correo/identificador lo es) está Pendiente por
> definir.

---

# Validaciones funcionales

## VF-01. Usuario existente

**Qué se valida:** que el usuario a editar exista.
**Cuándo:** al abrir la edición.
**Si cumple:** muestra los datos editables.
**Si no cumple:** Pendiente por definir.

## VF-02. Al menos un rol

**Qué se valida:** que el usuario conserve al menos un rol tras la edición.
**Cuándo:** al guardar cambios de rol.
**Si cumple:** guarda el conjunto de roles.
**Si no cumple:** Pendiente por definir (¿se permite un usuario sin rol?).

---

# Reglas de negocio

## RN-01. Multi-rol

Un usuario puede tener más de un rol simultáneamente; la asignación de rol es un
conjunto, no una selección única.
Origen: DEC (`DEC-004`).

## RN-02. Restablecer contraseña como acción de edición

El restablecimiento de contraseña por el Administrador es una acción de la
administración del usuario, no una HU separada.
Origen: DEC (`DEC-024`).

## RN-03. Gestor autorizado

La edición de usuarios y roles es función exclusiva del Administrador.
Origen: DOC (§5.1).

---

# Reglas de comportamiento

## RC-01. Efecto del cambio de rol

Al cambiar el conjunto de roles, se actualiza lo que el usuario ve y puede hacer
(RBAC). El efecto en sesiones activas: Pendiente por definir.

---

# Criterios de aceptación

## CA-01. Edición de datos

Cuando el Administrador edita los datos permitidos de un usuario y guarda, el
sistema los actualiza. Campos editables: Pendiente por definir.

## CA-02. Cambio de roles (conjunto)

Cuando el Administrador modifica el conjunto de roles de un usuario y guarda, el
sistema registra todos los roles seleccionados (`DEC-004`).

## CA-03. Restablecer contraseña

Cuando el Administrador ejecuta la acción de restablecer contraseña, el sistema
inicia el restablecimiento. El flujo exacto (¿nueva temporal como en HU-08?, ¿correo
de recuperación como HU-05/06?) es Pendiente por definir (`IMP-001`).

---

# Escenarios de prueba

## CP-01 — Cambio de roles

**Dado que** el Administrador seleccionó un usuario existente
**Cuando** le asigna los roles Coordinador y Colaborador y guarda
**Entonces** el sistema registra ambos roles para el usuario.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Usuario sin rol tras la edición | Pendiente por definir (¿se permite?). |
| FA-02 | Editor no Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |
| FA-03 | Restablecimiento de contraseña falla en Cognito | Pendiente por definir (depende del flujo de `IMP-001`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: acción exclusiva del Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |
| RNF-02 | Trazabilidad: Pendiente por definir (¿se audita la edición y el cambio de rol?). |

---

# Dependencias

- HU-08 (usuario creado), HU-40 (listar/seleccionar usuario).
- AWS Cognito para el restablecimiento de contraseña (RT-01), según el flujo de `IMP-001`.

---

# Aclaraciones

- `DEC-024`: `HU-11` (restablecer contraseña) quedó fusionada en HU-10; su
  identificador no se reutiliza.
- El flujo del restablecimiento por el Administrador (nueva temporal vs código de
  recuperación) está en `IMP-001`, sin resolver.

---

# Fuera de alcance

- Creación de usuarios (HU-08) y su listado (HU-40).
- Eliminación de usuarios (HU-09, `Descartada`).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Flujo exacto del restablecimiento de contraseña por el Administrador (`IMP-001`).
2. Qué campos son editables (¿correo/identificador editable?) y si se permite un
   usuario sin rol.
3. Autorización por rol en el servidor y auditoría de la edición (`DEC-010`).

---

# Prototipo

**Requerido:** Pendiente.

Descripción: Pendiente por definir.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Campos editables y flujo de reset Pendiente por definir.)
- [x] Validaciones definidas. (Parcial.)
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos.
- [x] Criterios verificables. (CA-03 sujeto a `IMP-001`.)
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (`IMP-001`; campos editables; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendientes
el flujo de reset (`IMP-001`) y los campos editables.

**Pendientes:** 3 importantes.
