# HU-10 — Editar información y rol de un usuario existente (incluye restablecer contraseña y activar/inactivar la cuenta)

| Campo | Valor |
|---|---|
| Identificador | HU-10 |
| Nombre | Editar información y rol de un usuario existente (incluye restablecer su contraseña y activar/inactivar la cuenta) |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.1 |
| Fuente principal | DOC (`compira-context.md` §5.1) + DEC (`DEC-004`, `DEC-024`, `DEC-036`) |
| Última actualización | 2026-10-03 |
| Dependencias | HU-08 (usuario creado), HU-40 (listar/seleccionar usuario) |

> HU en `Borrador`. `DEC-024` absorbe HU-11 (restablecer contraseña) como
> acción de esta HU. `DEC-004` establece multi-rol. `DEC-036` incorpora la
> **inactivación lógica reversible** (activar/inactivar) como acción de esta HU,
> en sustitución de la eliminación física descartada (`DEC-019`, HU-09). Lo no
> definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **editar la información y los roles de
un usuario existente, restablecer su contraseña y activar o inactivar su cuenta**,
para **mantener actualizada la administración de las cuentas sin perder su
información ni su historial**.

## Contexto funcional

El Administrador selecciona un usuario (HU-40) y edita sus datos, ajusta su(s)
rol(es), restablece su contraseña o cambia su estado (activo/inactivo). Por
`DEC-004`, un usuario puede tener más de un rol simultáneamente, por lo que la
asignación de rol es un **conjunto**, no una selección única. El restablecimiento de
contraseña (antes HU-11) es una acción dentro de esta HU (`DEC-024`); su flujo exacto
está en `IMP-001`.

COMPIRA **no elimina cuentas de forma permanente**: la baja de un usuario es una
**inactivación lógica reversible** (`DEC-036`). Al inactivar, el usuario pasa al
estado `DISABLED` y pierde el acceso a la plataforma de inmediato, pero su
información y su historial se conservan; la operación puede revertirse reactivando la
cuenta (estado `ACTIVE`). Esta capacidad sustituye a la eliminación física
descartada en `DEC-019` (HU-09). Módulo M5; actor: Administrador; resultado: usuario
actualizado. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Datos del usuario | Formulario | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Sí | Campos editables (¿nombre, apellido, teléfono?). ¿El correo es editable? Pendiente por definir. |
| Rol(es) | Selección múltiple | Sí | Conjunto | — | `ADMINISTRATOR`, `COORDINATOR`, `COLLABORATOR` | Sí | Un usuario puede tener varios roles (`DEC-004`). |
| Restablecer contraseña | Acción | No | — | — | — | N/A | Acción del Administrador (`DEC-024`). Flujo exacto: Pendiente por definir (`IMP-001`). |
| Estado de la cuenta | Acción / conmutador | No | — | — | `ACTIVE` (Activo), `DISABLED` (Inactivo) | Sí | Activar o inactivar la cuenta (`DEC-036`). Inactivar deshabilita el acceso de inmediato conservando datos e historial; activar restablece el acceso. Reversible. |

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

## VF-03. No inactivar la propia cuenta

**Qué se valida:** que el Administrador no inactive su propia cuenta.
**Cuándo:** al ejecutar la acción de inactivar.
**Si cumple:** aplica el cambio de estado a `DISABLED`.
**Si no cumple:** rechaza la operación con un mensaje de error ("No puedes inactivar tu propia cuenta") y mantiene la cuenta activa.

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

## RN-04. Baja lógica reversible (no eliminación física)

COMPIRA no elimina cuentas de forma permanente. La baja de un usuario se realiza
mediante **inactivación lógica**: el usuario pasa al estado `DISABLED`, conserva su
información e historial y puede reactivarse (estado `ACTIVE`) en cualquier momento.
Sustituye a la eliminación física descartada en `DEC-019` (HU-09).
Origen: DEC (`DEC-036`).

## RN-05. No autoinactivación

Un Administrador no puede inactivar su propia cuenta, para evitar que la
organización quede sin administrador operativo.
Origen: DEC (`DEC-036`).

---

# Reglas de comportamiento

## RC-01. Efecto del cambio de rol

Al cambiar el conjunto de roles, se actualiza lo que el usuario ve y puede hacer
(RBAC). El efecto en sesiones activas: Pendiente por definir.

## RC-02. Efecto de la inactivación en el acceso

Al inactivar una cuenta (`DISABLED`), el usuario pierde el acceso a la plataforma de
inmediato: las peticiones autenticadas se rechazan porque solo se admiten usuarios
en estado `ACTIVE`. Al reactivarla (`ACTIVE`), el usuario recupera el acceso con sus
roles y datos previos. La información y el historial del usuario se conservan durante
la inactivación.

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

## CA-04. Inactivar un usuario

Cuando el Administrador inactiva un usuario activo y confirma, el sistema cambia su
estado a `DISABLED`, el usuario deja de tener acceso de inmediato y su información e
historial se conservan.

## CA-05. Activar un usuario

Cuando el Administrador activa un usuario inactivo, el sistema cambia su estado a
`ACTIVE` y el usuario recupera el acceso con sus roles previos.

## CA-06. Impedir la autoinactivación

Cuando el Administrador intenta inactivar su propia cuenta, el sistema rechaza la
operación con el mensaje "No puedes inactivar tu propia cuenta" y la cuenta permanece
activa.

---

# Escenarios de prueba

## CP-01 — Cambio de roles

**Dado que** el Administrador seleccionó un usuario existente
**Cuando** le asigna los roles Coordinador y Colaborador y guarda
**Entonces** el sistema registra ambos roles para el usuario.

## CP-02 — Inactivar y reactivar un usuario

**Dado que** el Administrador seleccionó un usuario en estado Activo
**Cuando** lo inactiva y confirma la operación
**Entonces** el sistema cambia su estado a Inactivo (`DISABLED`), el usuario pierde
el acceso y, al volver a activarlo, recupera el acceso con sus roles previos.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Usuario sin rol tras la edición | Pendiente por definir (¿se permite?). |
| FA-02 | Editor no Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |
| FA-03 | Restablecimiento de contraseña falla en Cognito | Pendiente por definir (depende del flujo de `IMP-001`). |
| FA-04 | El Administrador intenta inactivar su propia cuenta | Operación rechazada con mensaje "No puedes inactivar tu propia cuenta"; la cuenta permanece activa (`DEC-036`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: acción exclusiva del Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |
| RNF-02 | Trazabilidad: Pendiente por definir (¿se audita la edición, el cambio de rol y el cambio de estado activo/inactivo?). |

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
- `DEC-036`: la baja de usuarios es una **inactivación lógica reversible**
  (estado `DISABLED`), incorporada como acción de esta HU en sustitución de la
  eliminación física descartada (`DEC-019`, HU-09).

---

# Fuera de alcance

- Creación de usuarios (HU-08) y su listado (HU-40).
- Eliminación física/permanente de usuarios (HU-09, `Descartada` por `DEC-019`); se
  sustituye por la inactivación lógica reversible que esta HU incorpora (`DEC-036`).

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

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-10-editar-usuario.svg` — edición con roles como
selección múltiple (`DEC-004`), acción de restablecer contraseña y conmutador de
estado activar/inactivar (`DEC-036`); campos editables y flujo del reset marcados como
Pendiente (IMP-001). El diseño visual final es decisión de UX del equipo.

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
