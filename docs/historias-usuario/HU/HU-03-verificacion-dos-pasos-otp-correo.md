# HU-03 — Verificación en dos pasos mediante código OTP por correo

| Campo | Valor |
|---|---|
| Identificador | HU-03 |
| Nombre | Verificación en dos pasos mediante código OTP por correo |
| Módulo | M1 — Autenticación y gestión de roles |
| Actor | Usuario registrado (Administrador, Coordinador o Colaborador) |
| Estado | Completada (desarrollo) · documentación reconstruida (`DEC-017`) |
| Prioridad | Alta |
| Versión | 1.0 |
| Fuente principal | HU (PDF) + DOC (`compira-context.md` §14) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-02 (login) o HU-01 (cambio de contraseña) que originan el desafío; AWS Cognito con MFA por correo (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`).

---

## Resumen ágil

Como **usuario registrado** necesito **confirmar mi identidad con un código
enviado a mi correo**, para **completar el acceso con verificación en dos pasos y
proteger mi cuenta**.

## Contexto funcional

Tras el login (o tras el cambio obligatorio de contraseña), cuando Cognito
devuelve el desafío `EMAIL_OTP`, el sistema muestra la pantalla "Verifica tu
identidad" (`/auth/verify`). El usuario ingresa el código de 6 dígitos recibido
por correo para completar la autenticación y recibir los tokens. Módulo M1;
resultado: acceso concedido con tokens. Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Código OTP | 6 campos de un dígito (solo numéricos) | Sí | Numérico | Exactamente 6 dígitos | 0–9 por dígito | Sí | Código recibido por correo. Soporta autoavance, retroceso y pegado. |
| session | Dato de contexto | Sí | Token de sesión del desafío | Pendiente por definir | Devuelto por el paso previo | No | Sesión del desafío; no la ingresa el usuario. |
| Destino enmascarado | Texto informativo (solo lectura) | N/A | Texto | N/A | N/A | No | Correo destino del código (`codeDeliveryDetails.destination`). |

---

# Validaciones funcionales

## VF-01. Código completo

**Qué se valida:** que los 6 dígitos estén diligenciados.
**Cuándo:** en el cliente, antes de habilitar el envío.
**Si cumple:** se habilita el botón de verificación.
**Si no cumple:** el botón permanece deshabilitado.

## VF-02. Verificación del código contra Cognito

**Qué se valida:** que el código sea correcto y vigente.
**Cuándo:** al enviar.
**Si cumple:** Cognito responde `AUTHENTICATED` y se guardan los tokens.
**Si no cumple:** `AUTH_003` (inválido) o `AUTH_004` (expirado).

## VF-03. Contexto válido de la pantalla

**Qué se valida:** que existan `session` y correo válidos al abrir `/auth/verify`.
**Cuándo:** al cargar la pantalla.
**Si cumple:** se muestra el formulario.
**Si no cumple:** redirige a la pantalla de inicio de sesión.

---

# Reglas de negocio

## RN-01. Formato del OTP

El código OTP tiene exactamente 6 dígitos numéricos.
Origen: HU (PDF).

---

# Reglas de comportamiento

## RC-01. Limpieza y foco tras código inválido

Cuando el código es incorrecto, el sistema limpia los 6 campos y devuelve el foco
al primero, manteniendo bloqueado el acceso.

## RC-02. Acceso exitoso

Cuando Cognito responde `AUTHENTICATED`, el sistema guarda tokens y datos de
usuario y redirige a `/`.

---

# Criterios de aceptación

## CA-01. Verificación exitosa

Cuando el usuario ingresa correctamente el código de 6 dígitos y Cognito responde
`AUTHENTICATED`, el sistema guarda los tokens y datos de usuario y lo redirige a `/`.

## CA-02. Habilitación del envío solo con código completo

El botón de verificación permanece deshabilitado hasta que los 6 dígitos estén
diligenciados.

## CA-03. Código inválido

Cuando el código es incorrecto, el sistema muestra `AUTH_003`, limpia los 6 campos
y devuelve el foco al primero, manteniendo bloqueado el acceso.

## CA-04. Código expirado

Cuando el código venció, el sistema muestra `AUTH_004` y no completa la
autenticación.

## CA-05. Acceso directo inválido a la pantalla

Cuando se intenta abrir `/auth/verify` sin `session` y correo válidos, el sistema
redirige a la pantalla de inicio de sesión.

---

# Escenarios de prueba

## CP-01 — Verificación exitosa

**Dado que** Cognito emitió el desafío `EMAIL_OTP` y el usuario recibió el código
**Cuando** ingresa los 6 dígitos correctos
**Entonces** Cognito responde `AUTHENTICATED` y el sistema muestra `/`.

## CP-02 — Código inválido

**Dado que** el usuario está en `/auth/verify`
**Cuando** ingresa un código incorrecto
**Entonces** el sistema muestra `AUTH_003`, limpia los campos y reenfoca el primero.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Código incorrecto | `AUTH_003` — "El código de confirmación es inválido". |
| FA-02 | Código expirado | `AUTH_004` — "El código de confirmación expiró". |
| FA-03 | Falta el código en la solicitud | `AUTH_012` — "Debes enviar el código de verificación para completar el reto seleccionado". |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: el segundo factor protege el acceso incluso si la contraseña fue comprometida. |

---

# Dependencias

- HU-02 (login) o HU-01 (cambio de contraseña) que originan el desafío `EMAIL_OTP`.
- AWS Cognito con MFA por correo (SES) habilitado (RT-01).

---

# Aclaraciones

- Contrato observado (Origen: HU/PDF): `POST /api/v1/auth/login/challenge`; request
  `{ email, session, challengeName: "EMAIL_OTP", code }`; el backend traduce a
  `challengeResponses = { USERNAME, EMAIL_OTP_CODE }`.
- El destino mostrado proviene de `codeDeliveryDetails.destination` (correo
  enmascarado por Cognito); si no está disponible, se muestra el correo ingresado.

---

# Fuera de alcance

- Verificación por SMS (`SMS_MFA`) y selección de canal (`SELECT_MFA_TYPE`):
  soportados en el enum del backend pero no expuestos en el frontend actual.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Cuál es el tiempo de vigencia del código OTP configurado en Cognito? (MEN-005).

---

# Prototipo

**Requerido:** Sí (M-03 en la fuente).

Descripción: pantalla "Verifica tu identidad" con estados de uso, validación
(código incompleto) y error (`AUTH_003`, `AUTH_004`).

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
- [x] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** Cumple.

**Pendientes:** 1 importante (vigencia del OTP).
