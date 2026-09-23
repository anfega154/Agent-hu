# HU-08 — Registro de usuarios por parte del administrador

| Campo | Valor |
|---|---|
| Identificador | HU-08 |
| Nombre | Registro de usuarios por parte del administrador |
| Módulo | M1 — Autenticación y gestión de roles (capacidad de gestión de usuarios, M5) |
| Actor | Administrador del sistema |
| Estado | Completada (desarrollo del flujo base) · documentación reconstruida (`DEC-017`) · **requiere extensión** (`DEC-014`) |
| Prioridad | Alta |
| Versión | 1.1 |
| Fuente principal | HU (PDF) + DOC (`compira-context.md` §14) + DEC (`DEC-014`, `DEC-004`) |
| Última actualización | 2026-09-21 |
| Dependencias | AWS Cognito `AdminCreateUser` (RT-01); HU-31 (Crear equipo) para la extensión de Equipo (`DEC-014`); RT-02 |

> Reconstrucción documental según `hu-template.md` (`DEC-017`). Versión 1.1 porque
> `DEC-014(b)` añade una **extensión funcional** (capturar el Equipo del
> Colaborador al crearlo) sobre una HU ya construida. Se distingue lo ya
> construido de lo pendiente de extensión.

---

## Resumen ágil

Como **administrador del sistema** necesito **registrar nuevos usuarios con sus
datos básicos, rol y contraseña temporal**, para **habilitar el acceso de nuevos
integrantes con un flujo de activación controlado**.

## Contexto funcional

Desde la pantalla protegida `/users/register`, el administrador diligencia el
formulario de registro. El sistema crea el usuario en Cognito (`AdminCreateUser`)
con una contraseña temporal, fija sus atributos y su preferencia de MFA, y crea el
perfil local. El nuevo usuario deberá cambiar su contraseña temporal en el primer
ingreso (HU-01). Módulo M1/M5; actor: Administrador; resultado: cuenta creada lista
para activación.

Extensión `DEC-014(b)`: al registrar un **Colaborador** debe capturarse el
**Equipo** al que pertenece (todo Colaborador debe tener siempre un Equipo,
`DEC-014(c)`). Esto es trabajo adicional sobre el flujo ya construido (que hoy no
captura Equipo). Origen: HU (PDF) / DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Nombre | Texto | Sí | Texto | Máx. 100 | — | Sí | Nombre del usuario. |
| Apellido | Texto | Sí | Texto | Máx. 100 | — | Sí | Apellido del usuario. |
| Correo electrónico | Texto (email) | Sí | Correo | Pendiente por definir | — | Sí | Será el usuario/identificador. |
| Código de país | Lista desplegable | Sí | Indicativo | — | Indicativos telefónicos (por defecto `+57`) | Sí | Prefijo para el teléfono. |
| Teléfono | Texto (solo dígitos) | Sí | E.164 al concatenar | `^\+[1-9]\d{7,14}$` | — | Sí | Se concatena con el indicativo en formato E.164. |
| Contraseña temporal | Texto (visible) | Sí | Texto | 10–128 | Debe cumplir la política de Cognito | Sí | El usuario la cambiará en el primer ingreso (HU-01). |
| Rol | Lista desplegable | Sí | Enum | — | `ADMINISTRATOR`, `COORDINATOR`, `COLLABORATOR` (por defecto `COLLABORATOR`) | Sí | Rol del usuario. Ver nota multi-rol (`DEC-004`). |
| Equipo | Pendiente por definir (control de selección) | Sí para Colaborador (`DEC-014`) | Pendiente por definir | Pendiente por definir | Equipos existentes (HU-31) | Sí | **Extensión `DEC-014(b)`:** equipo del Colaborador. Detalle de control y obligatoriedad por rol: Pendiente por definir. |

Canal MFA enviado: `EMAIL` (fijo desde la UI actual).

---

# Validaciones funcionales

## VF-01. Campos obligatorios y formato de teléfono

**Qué se valida:** presencia de campos obligatorios y teléfono en formato E.164.
**Cuándo:** al enviar el formulario.
**Si cumple:** continúa la creación.
**Si no cumple:** rechaza la creación con el mensaje de validación correspondiente
(p. ej. "El número de teléfono debe estar en formato E.164").

## VF-02. Correo no duplicado

**Qué se valida:** que el correo no exista ya.
**Cuándo:** al crear.
**Si cumple:** crea el usuario.
**Si no cumple:** `AUTH_001` (conflicto), no crea.

## VF-03. Política de contraseña temporal

**Qué se valida:** que la contraseña temporal cumpla la política de Cognito.
**Cuándo:** al crear.
**Si cumple:** crea el usuario.
**Si no cumple:** `AUTH_002`, no crea.

## VF-04. Equipo del Colaborador (extensión `DEC-014`)

**Qué se valida:** que se seleccione un Equipo existente al crear un Colaborador.
**Cuándo:** al crear (extensión pendiente de implementación).
**Si cumple:** asocia el usuario a su Equipo.
**Si no cumple:** Pendiente por definir (mensaje/comportamiento no documentado).

---

# Reglas de negocio

## RN-01. Teléfono en formato E.164

El teléfono debe almacenarse en formato E.164 (indicativo + número).
Origen: HU (PDF).

## RN-02. Cambio obligatorio en primer ingreso

Todo usuario nuevo queda obligado a cambiar su contraseña temporal en el primer
ingreso (HU-01).
Origen: HU (PDF).

## RN-03. Canal de MFA preferido

El canal de MFA preferido para los usuarios creados desde la UI actual es correo
(`EMAIL`).
Origen: HU (PDF).

## RN-04. Equipo obligatorio para el Colaborador

Todo Colaborador debe tener un Equipo asignado siempre; la asignación inicial
ocurre al crear el usuario.
Origen: DEC (`DEC-014(b,c)`).

## RN-05. Multi-rol

Un usuario puede tener más de un rol simultáneamente. Cómo se captura el conjunto
de roles en este formulario (hoy una sola lista desplegable) es Pendiente por
definir.
Origen: DEC (`DEC-004`).

---

# Reglas de comportamiento

## RC-01. Rol por defecto

Cuando no se especifica rol, el sistema asigna `COLLABORATOR`.

## RC-02. Consistencia ante fallo de persistencia local

Cuando el usuario se crea en Cognito pero falla la persistencia del perfil local,
el sistema revierte la creación eliminando el usuario en Cognito y propaga el
error, evitando cuentas huérfanas.

---

# Criterios de aceptación

## CA-01. Registro exitoso

Cuando el administrador diligencia correctamente correo, nombre, apellido, teléfono
(E.164 válido), rol y contraseña temporal, el sistema crea el usuario, confirma el
registro y deja la cuenta lista para el cambio de contraseña en el primer acceso. Se
muestra "Usuario <correo> creado correctamente. Recibirá un correo con las
instrucciones."

## CA-02. Correo duplicado

Cuando el correo ya existe, el sistema no crea el usuario y muestra `AUTH_001`.

## CA-03. Datos inválidos

Cuando falta un campo obligatorio o el teléfono no cumple E.164, el sistema rechaza
la creación y muestra el mensaje de validación correspondiente.

## CA-04. Contraseña temporal que no cumple la política

Cuando la contraseña temporal no cumple la política de Cognito, el sistema rechaza
la creación (`AUTH_002`).

## CA-05. Consistencia ante fallo de persistencia local

Cuando el usuario se crea en Cognito pero falla la persistencia local, el sistema
revierte la creación eliminando el usuario en Cognito y propaga el error.

## CA-06. Rol por defecto

Cuando no se especifica rol, el sistema asigna `COLLABORATOR`.

## CA-07. Asignación de Equipo al Colaborador (extensión `DEC-014`)

Cuando se registra un Colaborador, el sistema exige seleccionar un Equipo existente
y asocia el usuario a ese Equipo. Detalle de control/mensajes: Pendiente por definir.

---

# Escenarios de prueba

## CP-01 — Registro exitoso

**Dado que** el administrador está en `/users/register`
**Cuando** diligencia datos válidos (incluido Equipo para Colaborador) y envía
**Entonces** el sistema crea el usuario, lo asocia a su Equipo y muestra el mensaje
de éxito.

## CP-02 — Correo duplicado

**Dado que** el correo ya existe
**Cuando** el administrador intenta crear el usuario
**Entonces** el sistema muestra `AUTH_001` y no crea.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Correo duplicado | `AUTH_001` — "Ya existe una cuenta registrada con este correo electrónico". |
| FA-02 | Teléfono/campos inválidos | Mensajes de validación (`AUTH_014`). |
| FA-03 | Contraseña temporal débil | `AUTH_002`. |
| FA-04 | Sin token de sesión activo | "No se encontró un token de sesión activo." (UI). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad/permisos: la operación exige token de acceso. Restricción por rol `ADMINISTRATOR` en el servidor: Pendiente por definir (`DEC-010`, `IMP` de rol). |
| RNF-02 | Trazabilidad: el backend registra la creación de usuario con correo enmascarado. |

---

# Dependencias

- AWS Cognito con `AdminCreateUser`, atributos y SES configurados (RT-01).
- Sesión de administrador activa (token en `sessionStorage`).
- HU-31 (Crear equipo): necesaria para poder seleccionar el Equipo (extensión `DEC-014`).

---

# Aclaraciones

- Contrato observado (Origen: HU/PDF): `POST /api/v1/auth/register` con header
  `Authorization: Bearer <accessToken>`; request `{ email, password, firstName,
  lastName, phoneNumber, preferredMfaChannel: "EMAIL", roleCode }`; response
  `{ cognitoSub, userConfirmed, codeDeliveryDetails }`. Backend: `AdminCreateUser`
  (+ `AdminSetUserMFAPreference`), con rollback en Cognito si falla el guardado local.
- El correo de instrucciones al nuevo usuario lo gestiona Cognito/SES.
- RECOMENDACIÓN — requiere aprobación: la contraseña temporal se muestra en texto
  visible; evaluar si es aceptable o si debe generarse automáticamente (MEN-005).
- El request actual no incluye el campo Equipo; la extensión `DEC-014(b)` requiere
  ampliar el contrato (trabajo adicional sobre HU-08).

---

# Fuera de alcance

- Edición de usuarios existentes (HU-10).
- Confirmación de registro mediante código por el propio usuario (existe DTO en
  backend, no enrutado ni expuesto).
- Selección de canal MFA distinto de correo desde la UI.

---

# Preguntas pendientes

## Bloqueantes

Ninguna para el flujo base ya construido. Para la **extensión de Equipo**:

1. Control de UI, obligatoriedad por rol y comportamiento de error al capturar el
   Equipo del Colaborador (`DEC-014`); depende de HU-31/HU-41. (Detalle de campos
   de Equipo: MEN-004.)

## Importantes

1. ¿El backend restringe la operación al rol `ADMINISTRATOR` o basta un token
   válido de cualquier rol? (`DEC-010`).
2. ¿Debe generarse la contraseña temporal automáticamente? ¿Tiene periodo de
   validez? (MEN-005).
3. Con `DEC-004` (multi-rol), ¿cómo se capturan varios roles en este formulario?
   (hoy una sola lista).

---

# Prototipo

**Requerido:** Sí.

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-08-registro-usuarios.svg` — formulario de
registro con estados de uso, éxito y error (`AUTH_001/002/014`), y el control de
selección de Equipo marcado como extensión pendiente (`DEC-014`, IMP-016). Reemplaza
"M-06" del informe. El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (El campo Equipo tiene detalle Pendiente por definir.)
- [x] Validaciones definidas. (VF-04 con detalle pendiente.)
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [x] Criterios verificables. (CA-07 con detalle pendiente.)
- [x] Dependencias identificadas.
- [ ] Sin preguntas bloqueantes. (La extensión de Equipo depende de HU-31/HU-41 y del detalle de captura.)
- [ ] Sin supuestos funcionales críticos. (Restricción por rol en servidor `DEC-010`; captura de Equipo.)

## Resultado

**Estado DoR:** No cumple **para la extensión** (`DEC-014`): la captura de Equipo
depende de HU-31/HU-41 y de detalle no definido. El **flujo base** (sin Equipo) ya
está construido y documentado.

**Pendientes:** 1 bloqueante para la extensión (captura de Equipo) + 3 importantes.
