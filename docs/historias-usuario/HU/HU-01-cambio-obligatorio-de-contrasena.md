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
| Dependencias | HU-08 (registro con contraseña temporal); política de seguridad de contraseñas (RT-01) |

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
contraseña temporal y marcada para que el usuario la cambie. En el primer inicio de
sesión, el sistema detecta que la cuenta requiere cambiar la contraseña, lleva al
usuario a la pantalla "Crea tu nueva contraseña" y no le permite continuar hasta que
defina una contraseña válida. Es el primer paso obligatorio del ciclo de vida de una
cuenta. Módulo M1; actor: cualquier rol en su primer ingreso; resultado esperado:
cuenta activada con una credencial personal y acceso concedido (o, si la cuenta usa
verificación en dos pasos, a continuación se solicita un ingreso más seguro con un
código; ver HU-03).

Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Nueva contraseña | Texto (con mostrar/ocultar) | Sí | Texto | 10–128 caracteres en la interfaz | Debe cumplir la política de seguridad de contraseñas | Sí | Nueva contraseña personal del usuario. |
| Confirmar contraseña | Texto | Sí | Texto | 10–128 | Debe coincidir exactamente con "Nueva contraseña" | Sí | Repetición para evitar errores de tecleo. |
| Identificador de la sesión de cambio | Dato interno (no editable) | Sí | — | Pendiente por definir | Generado al iniciar sesión | No | Vincula el cambio con el intento de ingreso; se maneja internamente, el usuario no lo ve ni lo ingresa. |

Política de contraseña mostrada al usuario en pantalla (guía de criterios):
mínimo 10 caracteres, al menos una mayúscula, una minúscula, un número y un
carácter especial (`!@#$%^&*`). Nota: la política oficial exacta de seguridad está
`Pendiente por definir` (ver Preguntas pendientes e INC-05).

---

# Validaciones funcionales

## VF-01. Longitud y coincidencia de contraseña

**Qué se valida:** que "Nueva contraseña" tenga entre 10 y 128 caracteres y que
coincida exactamente con "Confirmar contraseña".
**Cuándo:** en el cliente, antes de habilitar el envío.
**Si cumple:** se habilita el botón de envío.
**Si no cumple:** el botón permanece deshabilitado; si no coinciden, se muestra
"Las contraseñas no coinciden".

## VF-02. Política de seguridad de la contraseña

**Qué se valida:** que la nueva contraseña cumpla la política de seguridad de
contraseñas.
**Cuándo:** al enviar el cambio.
**Si cumple:** el cambio se acepta y se concede el acceso (o se solicita el ingreso
en dos pasos, si aplica).
**Si no cumple:** se muestra el mensaje `AUTH_002` y no se cambia la contraseña.

## VF-03. Acceso válido a la pantalla

**Qué se valida:** que la pantalla de nueva contraseña se abra dentro de un intento
de ingreso válido (con la sesión de cambio y el correo asociados).
**Cuándo:** al cargar la pantalla.
**Si cumple:** se muestra el formulario.
**Si no cumple:** el sistema redirige a la pantalla de inicio de sesión.

---

# Reglas de negocio

## RN-01. Cambio obligatorio e ineludible

El cambio de contraseña en el primer ingreso es obligatorio; el usuario no puede
omitirlo ni acceder a la aplicación sin completarlo.
Origen: HU (PDF).

## RN-02. Cumplimiento de la política de seguridad

La contraseña definitiva debe cumplir la política de seguridad de contraseñas. El
valor exacto de esa política es `Pendiente por definir`.
Origen: HU (PDF) / RT-01.

---

# Reglas de comportamiento

## RC-01. Acceso tras el cambio

Tras un cambio exitoso, si no se requiere un paso adicional, el sistema inicia la
sesión del usuario y lo lleva a la pantalla principal.

## RC-02. Ingreso en dos pasos tras el cambio

Si la cuenta usa verificación en dos pasos, tras el cambio el sistema solicita un
ingreso más seguro mediante un código enviado al usuario (pantalla de verificación,
HU-03), sin dar por finalizado el acceso todavía.

---

# Criterios de aceptación

## CA-01. Redirección obligatoria al cambio de contraseña

Cuando el usuario inicia sesión con una contraseña temporal y el sistema detecta que
la cuenta requiere cambiar la contraseña, lo lleva a la pantalla de nueva contraseña
y no permite acceder a ninguna pantalla interna hasta completar el cambio.

## CA-02. Bloqueo del envío hasta cumplir longitud y coincidencia

El botón de envío permanece deshabilitado mientras la nueva contraseña no tenga
entre 10 y 128 caracteres o mientras no coincida con la confirmación. Si no
coinciden, se muestra "Las contraseñas no coinciden".

## CA-03. Cambio exitoso que completa el acceso

Cuando el usuario envía una contraseña que cumple la política de seguridad y no se
requiere un paso adicional, el sistema inicia la sesión y lo lleva a la pantalla
principal.

## CA-04. Cambio exitoso que continúa con el ingreso en dos pasos

Cuando la cuenta usa verificación en dos pasos, tras el cambio el sistema lleva al
usuario a la pantalla de verificación (HU-03) para completar un ingreso más seguro,
sin finalizar el acceso todavía.

## CA-05. Rechazo por política de seguridad no cumplida

Cuando la contraseña cumple la longitud pero no la política de seguridad, el sistema
muestra `AUTH_002` ("La contraseña no cumple con la política de seguridad definida")
y no cambia la contraseña.

## CA-06. Acceso directo inválido a la pantalla

Cuando se intenta abrir la pantalla de nueva contraseña fuera de un intento de
ingreso válido (sin la sesión de cambio y el correo asociados), el sistema redirige
a la pantalla de inicio de sesión.

---

# Escenarios de prueba

## CP-01 — Cambio exitoso con acceso directo

**Dado que** el usuario ingresa con una contraseña temporal y la cuenta requiere
cambiarla
**Cuando** define y confirma una contraseña válida que cumple la política de seguridad
**Entonces** el sistema inicia la sesión y muestra la pantalla principal.

## CP-02 — Rechazo por política no cumplida

**Dado que** el usuario está en la pantalla de nueva contraseña
**Cuando** envía una contraseña de longitud válida pero que no cumple la política de
seguridad
**Entonces** el sistema muestra `AUTH_002` y no cambia la contraseña.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Contraseñas no coinciden | Mantiene el formulario, no envía; mensaje "Las contraseñas no coinciden" (UI). |
| FA-02 | Contraseña no cumple la política de seguridad | No cambia la contraseña; `AUTH_002`. |
| FA-03 | La sesión de cambio expiró o no es válida | Error de la operación; `AUTH_012` ("La solicitud de autenticación no es válida"). |
| FA-04 | Error inesperado | Muestra mensaje genérico "Ocurrió un error inesperado. Intenta nuevamente." |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: la contraseña viaja solo por HTTPS; no se registra en logs (el backend enmascara correo y sesión). |
| RNF-02 | Trazabilidad: el backend registra inicio, éxito y fallo de la operación con datos enmascarados. |

---

# Dependencias

- HU-08 (registro de usuario con contraseña temporal, deja la cuenta marcada para cambio obligatorio).
- Política de seguridad de contraseñas configurada (RT-01).

---

# Aclaraciones

> Nota de lenguaje: el cuerpo de esta HU usa lenguaje de negocio. Los nombres
> técnicos del proveedor de identidad (AWS Cognito) se concentran aquí, para
> referencia de backend/QA, sin contaminar la especificación funcional.

- **Nota técnica (backend/QA):** el proveedor de identidad es AWS Cognito. El estado
  de "cuenta requiere cambiar la contraseña" corresponde a la respuesta de login
  `status = CHALLENGE_REQUIRED` con `challengeName = NEW_PASSWORD_REQUIRED`. El
  "ingreso en dos pasos" corresponde a `EMAIL_OTP` (o `SMS_MFA`). La ruta de la
  pantalla es `/auth/new-password`.
- **Contrato observado (Origen: HU/PDF):** `POST /api/v1/auth/login/challenge`;
  request `{ email, session, challengeName: "NEW_PASSWORD_REQUIRED", newPassword }`;
  response `{ status, user, tokens, challenge }`. El backend lo traduce a
  `RespondToAuthChallenge` de Cognito. El campo funcional "Identificador de la sesión
  de cambio" es `session`.
- RECOMENDACIÓN — requiere aprobación: alinear la validación del cliente con la
  política real de seguridad para evitar el error `AUTH_002` tras enviar. No es
  criterio de aceptación hasta aprobarse.
- INC-05 (ver PENDIENTES/MEN-005): la interfaz exige mínimo 10 y el backend valida
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

**Requerido:** Sí.

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-01-nueva-contrasena.svg` — pantalla "Crea tu
nueva contraseña" con estados de uso, validación (longitud/coincidencia) y error
(`AUTH_002`). Reemplaza la referencia "M-01" del informe (no existía como archivo).
El diseño visual final es decisión de UX del equipo.

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
