# HU-06 — Confirmación del restablecimiento de contraseña con código

| Campo | Valor |
|---|---|
| Identificador | HU-06 |
| Nombre | Confirmación del restablecimiento de contraseña con código |
| Módulo | M1 — Autenticación y gestión de roles |
| Actor | Usuario registrado (Administrador, Coordinador o Colaborador) |
| Estado | Completada (desarrollo) · documentación reconstruida (`DEC-017`) |
| Prioridad | Alta |
| Versión | 1.0 |
| Fuente principal | HU (PDF) + DOC (`compira-context.md` §14) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-05 (solicitud del código); AWS Cognito `confirmForgotPassword` (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`).

---

## Resumen ágil

Como **usuario registrado** necesito **confirmar el restablecimiento de mi
contraseña con el código recibido y una nueva contraseña**, para **volver a
ingresar con una credencial segura y vigente**.

## Contexto funcional

En el paso 2 de `/auth/password-recovery` ("Restablece tu contraseña"), el usuario
ingresa el código de 6 dígitos recibido, la nueva contraseña y su confirmación. Al
confirmar (`confirmForgotPassword`), Cognito restablece la contraseña y el sistema
muestra el paso 3 ("Contraseña actualizada") con la opción de volver al inicio de
sesión. Módulo M1. Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Código de verificación | Texto numérico (máx. 6 dígitos) | Sí | Numérico | 6 dígitos | 0–9 | Sí | Código recibido por correo (HU-05). |
| Nueva contraseña | Texto (password, con mostrar/ocultar) | Sí | Texto | 10–128 | Debe cumplir la política de Cognito | Sí | Nueva contraseña. |
| Confirmar contraseña | Texto (password) | Sí | Texto | 10–128 | Debe coincidir con "Nueva contraseña" | Sí | Repetición de la nueva contraseña. |

Política mostrada: mínimo 10 caracteres, una mayúscula, una minúscula, un número y
un carácter especial (`!@#$%^&*`). Política exacta de Cognito: Pendiente por
definir (INC-05).

---

# Validaciones funcionales

## VF-01. Formato de código y contraseña

**Qué se valida:** que el código no esté vacío, la contraseña tenga 10–128
caracteres y coincida con la confirmación.
**Cuándo:** en el cliente, antes de habilitar el envío.
**Si cumple:** se habilita el envío.
**Si no cumple:** el envío permanece deshabilitado; si no coinciden, "Las
contraseñas no coinciden".

## VF-02. Validez del código

**Qué se valida:** que el código sea correcto y vigente.
**Cuándo:** al confirmar.
**Si cumple:** se restablece la contraseña.
**Si no cumple:** `AUTH_003` (incorrecto) o `AUTH_004` (expirado); no cambia la contraseña.

## VF-03. Política de contraseña

**Qué se valida:** que la nueva contraseña cumpla la política de Cognito.
**Cuándo:** al confirmar.
**Si cumple:** se restablece la contraseña.
**Si no cumple:** `AUTH_002`; no realiza el cambio.

---

# Reglas de negocio

Pendiente por definir (no se documentaron reglas de negocio propias más allá del
comportamiento y la política de contraseña compartida con HU-01).

---

# Reglas de comportamiento

## RC-01. Confirmación y retorno

Tras un restablecimiento exitoso, el sistema muestra la pantalla de confirmación
(paso 3) con la opción de regresar al inicio de sesión.

---

# Criterios de aceptación

## CA-01. Restablecimiento exitoso

Cuando el usuario ingresa un código válido junto con una nueva contraseña que
cumple la política y coincide con su confirmación, el sistema restablece la
contraseña y muestra la pantalla de confirmación para regresar al inicio de sesión.

## CA-02. Bloqueo del envío por reglas de formato

El envío permanece deshabilitado mientras el código esté vacío, la contraseña no
tenga 10–128 caracteres o no coincida con la confirmación (se muestra "Las
contraseñas no coinciden").

## CA-03. Código incorrecto o expirado

Cuando el código es incorrecto (`AUTH_003`) o expiró (`AUTH_004`), el sistema
informa el motivo y no cambia la contraseña actual.

## CA-04. Contraseña que no cumple la política

Cuando la nueva contraseña no cumple la política de Cognito, el sistema muestra
`AUTH_002` y no realiza el cambio.

---

# Escenarios de prueba

## CP-01 — Restablecimiento exitoso

**Dado que** el usuario recibió el código (HU-05) y está en el paso 2
**Cuando** ingresa un código válido y una contraseña que cumple la política
**Entonces** Cognito restablece la contraseña y el sistema muestra el paso 3.

## CP-02 — Código expirado

**Dado que** el usuario está en el paso 2
**Cuando** ingresa un código expirado
**Entonces** el sistema muestra `AUTH_004` y no cambia la contraseña.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Código inválido | `AUTH_003` — "El código de confirmación es inválido". |
| FA-02 | Código expirado | `AUTH_004` — "El código de confirmación expiró". |
| FA-03 | Contraseña no cumple política | `AUTH_002` — "La contraseña no cumple con la política definida en Cognito". |
| FA-04 | Contraseñas no coinciden | "Las contraseñas no coinciden" (UI). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| — | Pendiente por definir (no documentados RNF específicos en la fuente para esta HU). |

---

# Dependencias

- HU-05 (solicitud del código).
- AWS Cognito `confirmForgotPassword` (RT-01).

---

# Aclaraciones

- Contrato observado (Origen: HU/PDF): `POST /api/v1/auth/password-recovery/confirm`;
  request `{ email, confirmationCode, newPassword }`; response `204`.
- INC-05 (MEN-005): política mínima de contraseña difiere entre UI (10) y backend (8).

---

# Fuera de alcance

- Reutilización de contraseñas anteriores / historial de contraseñas (depende de la
  configuración de Cognito).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Cognito está configurado para impedir reutilizar contraseñas recientes?
   (MEN-005).

---

# Prototipo

**Requerido:** Sí (M-04, pasos 2 y 3).

Descripción: pantalla "Restablece tu contraseña" (paso 2) y "Contraseña
actualizada" (paso 3), con estados de uso, validación y error.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos.
- [x] Validaciones definidas.
- [ ] Reglas de negocio definidas. (Comparte la política de contraseña con HU-01; sin reglas propias adicionales documentadas.)
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [x] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** Cumple.

**Pendientes:** 1 importante (reutilización de contraseñas).
