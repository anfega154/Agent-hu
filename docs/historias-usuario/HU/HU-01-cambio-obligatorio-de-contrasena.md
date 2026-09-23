# HU-01 — Cambio obligatorio de contraseña en el primer inicio de sesión

| Campo | Valor |
|---|---|
| Identificador | HU-01 |
| Nombre | Cambio obligatorio de contraseña en el primer inicio de sesión |
| Módulo | M1 — Autenticación y gestión de roles |
| Actor | Usuario registrado (Administrador, Coordinador o Colaborador) que ingresa por primera vez con contraseña temporal |
| Estado | Completada (desarrollo) · documentación reconstruida (`DEC-017`) |
| Prioridad | Alta |
| Versión | 1.0 |
| Fuente principal | HU (PDF `COMPIRA_Historias_Usuario.pdf`) + DOC (`compira-context.md` §14) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-08 (registro con contraseña temporal); configuración de política de contraseñas en AWS Cognito (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`). No reabre
> desarrollo: el software está `Completada`. El contenido proviene de evidencia
> real del PDF y del contexto; lo no confirmado se marca `Pendiente por definir`.

---

## Resumen ágil

Como **usuario registrado que ingresa por primera vez con una contraseña temporal**
necesito **establecer mi propia contraseña personal antes de acceder**, para
**activar mi cuenta con una credencial segura y bajo mi control**.

## Contexto funcional

Cuando el Administrador crea un usuario (HU-08), la cuenta queda con una
contraseña temporal y en un estado que obliga a cambiarla. En el primer inicio de
sesión, AWS Cognito responde al login con `status = CHALLENGE_REQUIRED` y
`challengeName = NEW_PASSWORD_REQUIRED`. El sistema redirige al usuario a la
pantalla "Crea tu nueva contraseña" (`/auth/new-password`) y no le permite
continuar hasta definir una contraseña válida. Es el primer paso obligatorio del
ciclo de vida de una cuenta. Módulo M1; actor: cualquier rol en su primer ingreso;
resultado esperado: cuenta activada con credencial personal y acceso concedido (o
encadenamiento a un segundo desafío).

Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Nueva contraseña | Texto (password, con mostrar/ocultar) | Sí | Texto | 10–128 caracteres en la UI | Debe cumplir la política del pool de Cognito | Sí | Nueva contraseña personal del usuario. |
| Confirmar contraseña | Texto (password) | Sí | Texto | 10–128 | Debe coincidir exactamente con "Nueva contraseña" | Sí | Repetición para evitar errores de tecleo. |
| session | Dato de contexto (no editable) | Sí | Token de sesión del desafío | Pendiente por definir | Devuelto por el login | No | Sesión del desafío `NEW_PASSWORD_REQUIRED`; se transporta internamente, no la ingresa el usuario. |

Política de contraseña mostrada al usuario en pantalla (tooltip de criterios):
mínimo 10 caracteres, al menos una mayúscula, una minúscula, un número y un
carácter especial (`!@#$%^&*`). Nota: la política oficial exacta configurada en
Cognito está `Pendiente por definir` (ver Preguntas pendientes e INC-05).

---

# Validaciones funcionales

## VF-01. Longitud y coincidencia de contraseña

**Qué se valida:** que "Nueva contraseña" tenga entre 10 y 128 caracteres y que
coincida exactamente con "Confirmar contraseña".
**Cuándo:** en el cliente, antes de habilitar el envío.
**Si cumple:** se habilita el botón de envío.
**Si no cumple:** el botón permanece deshabilitado; si no coinciden, se muestra
"Las contraseñas no coinciden".

## VF-02. Política de contraseña de Cognito

**Qué se valida:** que la nueva contraseña cumpla la política configurada en el
pool de Cognito.
**Cuándo:** en el backend/Cognito, al enviar el cambio.
**Si cumple:** Cognito responde `AUTHENTICATED` (o encadena otro desafío).
**Si no cumple:** se muestra el mensaje `AUTH_002` y no se cambia la credencial.

## VF-03. Contexto válido de la pantalla

**Qué se valida:** que exista `session` y correo válidos en el contexto al abrir
`/auth/new-password`.
**Cuándo:** al cargar la pantalla.
**Si cumple:** se muestra el formulario.
**Si no cumple:** el sistema redirige a la pantalla de inicio de sesión.

---

# Reglas de negocio

## RN-01. Cambio obligatorio e ineludible

El cambio de contraseña en el primer ingreso es obligatorio; el usuario no puede
omitirlo ni acceder a la aplicación sin completarlo.
Origen: HU (PDF).

## RN-02. Cumplimiento de la política de Cognito

La contraseña definitiva debe cumplir la política configurada en el pool de
Cognito. El valor exacto de esa política es `Pendiente por definir`.
Origen: HU (PDF) / RT-01.

---

# Reglas de comportamiento

## RC-01. Redirección posterior al cambio

Tras un cambio exitoso, si Cognito responde `AUTHENTICATED`, el sistema guarda la
sesión (tokens y datos de usuario) y redirige a la pantalla principal (`/`).

## RC-02. Encadenamiento a segundo desafío

Si tras el cambio Cognito responde nuevamente `CHALLENGE_REQUIRED` con `EMAIL_OTP`
(o `SMS_MFA`), el sistema conduce a la pantalla de verificación (`/auth/verify`,
HU-03) con la nueva `session`, sin dar por finalizado el acceso.

---

# Criterios de aceptación

## CA-01. Redirección obligatoria al desafío de cambio

Cuando el usuario inicia sesión con contraseña temporal y el login responde
`challengeName = NEW_PASSWORD_REQUIRED`, el sistema lo redirige a la pantalla de
nueva contraseña y no permite acceder a ninguna ruta protegida hasta completar el
cambio.

## CA-02. Bloqueo del envío hasta cumplir longitud y coincidencia

El botón de envío permanece deshabilitado mientras la nueva contraseña no tenga
entre 10 y 128 caracteres o mientras no coincida con la confirmación. Si no
coinciden, se muestra "Las contraseñas no coinciden".

## CA-03. Cambio exitoso que completa el acceso

Cuando el usuario envía una contraseña que cumple la política de Cognito y este
responde `AUTHENTICATED`, el sistema guarda la sesión y lo redirige a `/`.

## CA-04. Cambio exitoso que encadena un segundo desafío

Cuando tras el cambio Cognito responde `CHALLENGE_REQUIRED` con `EMAIL_OTP` (o
`SMS_MFA`), el sistema conduce a `/auth/verify` con la nueva `session`, sin
finalizar el acceso.

## CA-05. Rechazo por política de contraseña no cumplida

Cuando la contraseña cumple la longitud pero no la política de Cognito, el sistema
muestra `AUTH_002` ("La contraseña no cumple con la política definida en Cognito")
y no cambia la credencial.

## CA-06. Acceso directo inválido a la pantalla

Cuando se intenta abrir `/auth/new-password` sin `session` y correo válidos en el
contexto, el sistema redirige a la pantalla de inicio de sesión.

---

# Escenarios de prueba

## CP-01 — Cambio exitoso con acceso directo

**Dado que** el usuario ingresa con contraseña temporal y Cognito responde
`NEW_PASSWORD_REQUIRED`
**Cuando** define y confirma una contraseña válida que cumple la política
**Entonces** Cognito responde `AUTHENTICATED`, el sistema guarda la sesión y
muestra la pantalla principal.

## CP-02 — Rechazo por política no cumplida

**Dado que** el usuario está en `/auth/new-password`
**Cuando** envía una contraseña de longitud válida pero que no cumple la política
de Cognito
**Entonces** el sistema muestra `AUTH_002` y no cambia la credencial.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Contraseñas no coinciden | Mantiene el formulario, no envía; mensaje "Las contraseñas no coinciden" (UI). |
| FA-02 | Contraseña no cumple política de Cognito | No cambia la credencial; `AUTH_002`. |
| FA-03 | Sesión de desafío expirada o inválida | Error de challenge; `AUTH_012` ("La solicitud del reto de autenticación no es válida"). |
| FA-04 | Error inesperado | Muestra mensaje genérico "Ocurrió un error inesperado. Intenta nuevamente." |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: la contraseña viaja solo por HTTPS; no se registra en logs (el backend enmascara correo y sesión). |
| RNF-02 | Trazabilidad: el backend registra inicio, éxito y fallo de la operación con datos enmascarados. |

---

# Dependencias

- HU-08 (registro de usuario con contraseña temporal, deja la cuenta en estado de cambio obligatorio).
- AWS Cognito con política de contraseñas configurada (RT-01).

---

# Aclaraciones

- Contrato funcional observado (Origen: HU/PDF): `POST /api/v1/auth/login/challenge`;
  request `{ email, session, challengeName: "NEW_PASSWORD_REQUIRED", newPassword }`;
  response `AuthResponse { status, user, tokens, challenge }`. El backend traduce el
  desafío a `RespondToAuthChallenge` de Cognito.
- RECOMENDACIÓN — requiere aprobación: alinear la validación del cliente con la
  política real de Cognito para evitar el error `AUTH_002` tras enviar. No es
  criterio de aceptación hasta aprobarse.
- INC-05 (ver PENDIENTES/MEN-005): la UI exige mínimo 10 y el backend valida
  mínimo 8; se recomienda unificar. No se decide aquí.

---

# Fuera de alcance

- Cambio de contraseña voluntario desde el perfil de un usuario ya autenticado.
- Expiración periódica de contraseñas.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Cuál es la política exacta de contraseñas configurada en Cognito (longitud
   mínima y clases de caracteres)? (MEN-005 / INC-05).
2. ¿La contraseña temporal tiene fecha de expiración? Si expira antes del primer
   ingreso, ¿qué mensaje debe ver el usuario? (MEN-005).

---

# Prototipo

**Requerido:** Sí (referido como M-01 en la fuente).

Descripción del prototipo requerido: pantalla "Crea tu nueva contraseña" con
estados de uso, validación (longitud/coincidencia) y error (`AUTH_002`).

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
- [ ] Sin supuestos funcionales críticos. (Política exacta de Cognito pendiente — no bloqueante para el comportamiento ya construido.)

## Resultado

**Estado DoR:** Cumple (para documentación de funcionalidad ya construida; los
pendientes son detalles de configuración, no bloquean el comportamiento).

**Pendientes:** 2 importantes (política de contraseña, expiración de temporal).
