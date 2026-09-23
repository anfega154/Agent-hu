# HU-02 — Inicio de sesión con correo y contraseña

| Campo | Valor |
|---|---|
| Identificador | HU-02 |
| Nombre | Inicio de sesión con correo y contraseña |
| Módulo | M1 — Autenticación y gestión de roles |
| Actor | Usuario registrado (Administrador, Coordinador o Colaborador) |
| Estado | Completada (desarrollo) · documentación reconstruida (`DEC-017`) |
| Prioridad | Alta |
| Versión | 1.0 |
| Fuente principal | HU (PDF) + DOC (`compira-context.md` §14) |
| Última actualización | 2026-09-21 |
| Dependencias | AWS Cognito (flujo `USER_PASSWORD_AUTH`, RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`). Contenido de
> evidencia real; lo no confirmado se marca `Pendiente por definir`.

---

## Resumen ágil

Como **usuario registrado** necesito **iniciar sesión con mi correo y mi
contraseña**, para **acceder a la plataforma de forma segura y continuar mis
actividades según el rol asignado**.

## Contexto funcional

Es el punto de entrada a la plataforma (`/auth/login`). El usuario ingresa correo
y contraseña; el sistema autentica contra Cognito (`InitiateAuth`, flujo
`USER_PASSWORD_AUTH`). Según la respuesta, el sistema puede: (a) emitir tokens y
dar acceso directo; (b) exigir cambio de contraseña obligatorio (HU-01); o (c)
exigir el segundo factor por OTP (HU-03). Módulo M1; actor: cualquier rol;
resultado: acceso o desvío al desafío correspondiente. Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Correo electrónico | Texto (email) | Sí | Correo | Pendiente por definir | Correo registrado | Sí | Identificador del usuario. |
| Contraseña | Texto (password, con mostrar/ocultar) | Sí | Texto | Pendiente por definir | — | Sí | Contraseña del usuario. |

- El botón "Iniciar sesión" permanece deshabilitado mientras el correo o la
  contraseña estén vacíos.
- Enlace visible "Recuperar contraseña" que lleva a HU-05.

---

# Validaciones funcionales

## VF-01. Campos requeridos

**Qué se valida:** que correo y contraseña no estén vacíos.
**Cuándo:** en el cliente, antes de habilitar el envío.
**Si cumple:** se habilita "Iniciar sesión".
**Si no cumple:** el botón permanece deshabilitado.

## VF-02. Autenticación de credenciales

**Qué se valida:** que las credenciales sean válidas contra Cognito.
**Cuándo:** al enviar el formulario.
**Si cumple:** Cognito responde `AUTHENTICATED` o un desafío (`NEW_PASSWORD_REQUIRED`, `EMAIL_OTP`, `SMS_MFA`).
**Si no cumple:** se muestra `AUTH_005` y no se otorga acceso.

## VF-03. Estado de la cuenta

**Qué se valida:** que la cuenta esté confirmada y no requiera restablecer contraseña.
**Cuándo:** al autenticar.
**Si cumple:** continúa el flujo.
**Si no cumple:** `AUTH_006` (no confirmada) o `AUTH_008` (requiere restablecer).

---

# Reglas de negocio

## RN-01. Rol determinado automáticamente

El rol del usuario se determina automáticamente al autenticar (la pantalla indica:
"El sistema asignará tu rol automáticamente al iniciar sesión").
Origen: HU (PDF).

> Nota de consistencia: `DEC-004` establece que un usuario puede tener más de un
> rol simultáneamente. Cómo se refleja el multi-rol en el resultado del login
> (rol activo vs conjunto de roles) es `Pendiente por definir` (ver Preguntas).

---

# Reglas de comportamiento

## RC-01. Desvíos según respuesta de Cognito

Según la respuesta, el sistema: da acceso directo a `/` (`AUTHENTICATED`), o
redirige a `/auth/new-password` (`NEW_PASSWORD_REQUIRED`, HU-01), o a `/auth/verify`
(`EMAIL_OTP`/`SMS_MFA`, HU-03).

---

# Criterios de aceptación

## CA-01. Acceso directo con credenciales válidas sin desafíos

Cuando el usuario ingresa credenciales válidas y Cognito responde `AUTHENTICATED`,
el sistema guarda la sesión y lo redirige a `/`.

## CA-02. Desvío a cambio obligatorio de contraseña

Cuando Cognito responde `CHALLENGE_REQUIRED` con `NEW_PASSWORD_REQUIRED`, el
sistema redirige a `/auth/new-password` (HU-01).

## CA-03. Desvío a verificación en dos pasos

Cuando Cognito responde `CHALLENGE_REQUIRED` con `EMAIL_OTP` o `SMS_MFA`, el
sistema redirige a `/auth/verify` (HU-03).

## CA-04. Rechazo por credenciales inválidas

Cuando el correo o la contraseña son incorrectos, el sistema muestra `AUTH_005`
("Las credenciales ingresadas no son válidas") y no otorga acceso.

## CA-05. Rechazo por estado de la cuenta

Cuando la cuenta no está confirmada, muestra `AUTH_006`. Cuando Cognito exige
restablecer la contraseña, muestra `AUTH_008`.

---

# Escenarios de prueba

## CP-01 — Acceso directo válido

**Dado que** el usuario tiene credenciales válidas y MFA no aplica
**Cuando** envía correo y contraseña correctos
**Entonces** Cognito responde `AUTHENTICATED` y el sistema muestra `/`.

## CP-02 — Desvío a OTP

**Dado que** la cuenta requiere segundo factor
**Cuando** las credenciales son válidas
**Entonces** Cognito responde `EMAIL_OTP` y el sistema redirige a `/auth/verify`.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Credenciales inválidas | `AUTH_005` — "Las credenciales ingresadas no son válidas". |
| FA-02 | Cuenta no confirmada | `AUTH_006` — "La cuenta aún no ha sido confirmada". |
| FA-03 | Requiere reset de contraseña | `AUTH_008` — "Debes restablecer la contraseña antes de iniciar sesión". |
| FA-04 | Datos vacíos o formato inválido | `AUTH_014` — "La solicitud contiene datos inválidos o incompletos". |
| FA-05 | Demasiados intentos | `AUTH_013` — "Se recibieron demasiadas solicitudes. Inténtalo nuevamente en unos minutos". |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: doble factor de autenticación habilitado; la contraseña no se almacena en el cliente. |
| RNF-02 | Trazabilidad: el backend registra el resultado del login con correo enmascarado. |

---

# Dependencias

- AWS Cognito configurado con el cliente de aplicación y flujo `USER_PASSWORD_AUTH` (RT-01).

---

# Aclaraciones

- Contrato observado (Origen: HU/PDF): `POST /api/v1/auth/login`; request `{ email, password }`;
  response `AuthResponse { status, user, tokens, challenge }`.
- El campo correo usa `type=email` pero el formulario tiene `noValidate`; la
  validación de formato definitiva la realiza el backend.

---

# Fuera de alcance

- Inicio de sesión con proveedores externos (Google, SSO, etc.).
- Selección manual de canal MFA y verificación por SMS: el enum existe en backend
  pero el frontend actual solo expone OTP por correo.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Existe un límite de intentos fallidos con bloqueo temporal, o el control queda
   delegado a Cognito? (MEN-005 / bloqueo por intentos).
2. Con `DEC-004` (multi-rol), ¿cómo se refleja el conjunto de roles en el
   resultado del login (rol activo, selección, todos)? (Pendiente por definir).

---

# Prototipo

**Requerido:** Sí.

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-02-inicio-sesion.svg` — login con estados de
uso, error (`AUTH_005/006/008`) y desvíos a HU-01/HU-03. Reemplaza "M-02" del
informe. El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos.
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Reflejo del multi-rol en el login pendiente — no bloqueante para el comportamiento ya construido.)

## Resultado

**Estado DoR:** Cumple (documentación de funcionalidad ya construida).

**Pendientes:** 2 importantes (bloqueo por intentos; reflejo de multi-rol en login).
