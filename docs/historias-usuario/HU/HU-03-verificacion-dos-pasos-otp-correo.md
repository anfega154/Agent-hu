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
| Dependencias | HU-02 (login) o HU-01 (cambio de contraseña) que solicitan el ingreso en dos pasos; verificación por correo habilitada (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`).

---

## Resumen ágil

Como **usuario registrado** necesito **confirmar mi identidad con un código
enviado a mi correo**, para **completar el acceso con verificación en dos pasos y
proteger mi cuenta**.

## Contexto funcional

Tras el inicio de sesión (o tras el cambio obligatorio de contraseña), cuando la
cuenta usa verificación en dos pasos, el sistema muestra la pantalla "Verifica tu
identidad". El usuario ingresa el código de 6 dígitos recibido por correo para
completar un ingreso más seguro y obtener el acceso. Módulo M1;
resultado: acceso concedido con tokens. Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Código OTP | 6 campos de un dígito (solo numéricos) | Sí | Numérico | Exactamente 6 dígitos | 0–9 por dígito | Sí | Código recibido por correo. Soporta autoavance, retroceso y pegado. |
| Identificador de la sesión de ingreso | Dato interno | Sí | — | Pendiente por definir | Generado en el paso previo | No | Vincula la verificación con el intento de ingreso; no lo ingresa el usuario. |
| Correo destino (enmascarado) | Texto informativo (solo lectura) | N/A | Texto | N/A | N/A | No | Muestra, parcialmente oculto, el correo al que se envió el código. |

---

# Validaciones funcionales

## VF-01. Código completo

**Qué se valida:** que los 6 dígitos estén diligenciados.
**Cuándo:** en el cliente, antes de habilitar el envío.
**Si cumple:** se habilita el botón de verificación.
**Si no cumple:** el botón permanece deshabilitado.

## VF-02. Verificación del código

**Qué se valida:** que el código ingresado sea correcto y esté vigente.
**Cuándo:** al enviar.
**Si cumple:** el sistema concede el acceso.
**Si no cumple:** `AUTH_003` (inválido) o `AUTH_004` (expirado).

## VF-03. Acceso válido a la pantalla

**Qué se valida:** que la pantalla de verificación se abra dentro de un intento de
ingreso válido (con la sesión de ingreso y el correo asociados).
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

Cuando el código es correcto, el sistema concede el acceso, inicia la sesión y lleva
al usuario a la pantalla principal.

---

# Criterios de aceptación

## CA-01. Verificación exitosa

Cuando el usuario ingresa correctamente el código de 6 dígitos, el sistema concede
el acceso, inicia la sesión y lo lleva a la pantalla principal.

## CA-02. Habilitación del envío solo con código completo

El botón de verificación permanece deshabilitado hasta que los 6 dígitos estén
diligenciados.

## CA-03. Código inválido

Cuando el código es incorrecto, el sistema muestra `AUTH_003`, limpia los 6 campos
y devuelve el foco al primero, manteniendo bloqueado el acceso.

## CA-04. Código expirado

Cuando el código venció, el sistema muestra `AUTH_004` y no concede el acceso.

## CA-05. Acceso directo inválido a la pantalla

Cuando se intenta abrir la pantalla de verificación fuera de un intento de ingreso
válido (sin la sesión de ingreso y el correo asociados), el sistema redirige a la
pantalla de inicio de sesión.

---

# Escenarios de prueba

## CP-01 — Verificación exitosa

**Dado que** la cuenta solicitó verificación en dos pasos y el usuario recibió el código
**Cuando** ingresa los 6 dígitos correctos
**Entonces** el sistema concede el acceso y muestra la pantalla principal.

## CP-02 — Código inválido

**Dado que** el usuario está en la pantalla de verificación
**Cuando** ingresa un código incorrecto
**Entonces** el sistema muestra `AUTH_003`, limpia los campos y reenfoca el primero.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Código incorrecto | `AUTH_003` — "El código de confirmación es inválido". |
| FA-02 | Código expirado | `AUTH_004` — "El código de confirmación expiró". |
| FA-03 | Falta el código en la solicitud | `AUTH_012` — "Debes enviar el código de verificación para completar el ingreso en dos pasos". |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: el segundo factor protege el acceso incluso si la contraseña fue comprometida. |

---

# Dependencias

- HU-02 (login) o HU-01 (cambio de contraseña) que solicitan el ingreso en dos pasos.
- Verificación por correo habilitada (RT-01).

---

# Aclaraciones

> Nota de lenguaje: el cuerpo usa lenguaje de negocio; los nombres técnicos del
> proveedor de identidad (AWS Cognito) se concentran aquí para backend/QA.

- **Nota técnica (backend/QA):** el "ingreso en dos pasos" corresponde al desafío de
  Cognito `EMAIL_OTP`; "acceso concedido" = `AUTHENTICATED`. Ruta: `/auth/verify`.
- **Contrato observado (Origen: HU/PDF):** `POST /api/v1/auth/login/challenge`;
  request `{ email, session, challengeName: "EMAIL_OTP", code }`; el backend lo
  traduce a `challengeResponses = { USERNAME, EMAIL_OTP_CODE }`. El campo funcional
  "Identificador de la sesión de ingreso" es `session`.
- El correo destino mostrado proviene de `codeDeliveryDetails.destination` (correo
  enmascarado por el proveedor); si no está disponible, se muestra el correo ingresado.

---

# Fuera de alcance

- Verificación por SMS y selección de canal de verificación: soportado en el backend
  pero no expuesto en la interfaz actual.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Cuál es el tiempo de vigencia del código OTP configurado en Cognito? (MEN-005).

---

# Prototipo

**Requerido:** Sí.

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-03-verificacion-otp.svg` — "Verifica tu
identidad" con estados de uso, código inválido (`AUTH_003`) y expirado (`AUTH_004`).
Reemplaza "M-03" del informe. El diseño visual final es decisión de UX del equipo.

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
