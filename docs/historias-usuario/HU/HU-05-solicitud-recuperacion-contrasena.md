# HU-05 — Solicitud de recuperación de contraseña por correo

| Campo | Valor |
|---|---|
| Identificador | HU-05 |
| Nombre | Solicitud de recuperación de contraseña por correo |
| Módulo | M1 — Autenticación y gestión de roles |
| Actor | Usuario registrado (Administrador, Coordinador o Colaborador) |
| Estado | Completada (desarrollo) · documentación reconstruida (`DEC-017`) |
| Prioridad | Alta |
| Versión | 1.0 |
| Fuente principal | HU (PDF) + DOC (`compira-context.md` §14) |
| Última actualización | 2026-09-21 |
| Dependencias | AWS Cognito con `forgotPassword` y SES configurado (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`).

---

## Resumen ágil

Como **usuario registrado** necesito **solicitar la recuperación de mi contraseña
desde la pantalla de acceso**, para **restablecer mi ingreso cuando olvido la
credencial actual**.

## Contexto funcional

Desde el enlace "Recuperar contraseña" del login, el usuario llega a
`/auth/password-recovery` (paso 1: "Recuperar contraseña"). Ingresa su correo; si
la cuenta existe, Cognito (`forgotPassword`) envía un código al destino
configurado y el sistema avanza al paso de confirmación (HU-06). Módulo M1.
Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Correo electrónico | Texto (email) | Sí | Correo | Pendiente por definir | Correo de la cuenta a recuperar | Sí | Correo al que se enviará el código de recuperación. |

---

# Validaciones funcionales

## VF-01. Correo presente y válido

**Qué se valida:** que el correo no esté vacío y tenga formato válido.
**Cuándo:** al enviar la solicitud.
**Si cumple:** se solicita el envío del código y se avanza al paso de confirmación.
**Si no cumple:** se informa el error y no se avanza.

---

# Reglas de negocio

Pendiente por definir (no se documentaron reglas de negocio específicas para esta
HU en la fuente más allá del comportamiento descrito).

---

# Reglas de comportamiento

## RC-01. Avance al paso de confirmación

Tras una solicitud exitosa, el sistema muestra el destino enmascarado y avanza al
paso de confirmación (HU-06).

---

# Criterios de aceptación

## CA-01. Envío exitoso del código

Cuando el usuario ingresa un correo asociado a una cuenta existente, el sistema
solicita el envío del código, muestra el destino enmascarado y avanza al paso de
confirmación.

## CA-02. Rechazo por correo inválido o vacío

Cuando el correo está vacío, tiene formato inválido o el servicio rechaza la
solicitud, el sistema informa el error y no avanza al paso de restablecimiento.

---

# Escenarios de prueba

## CP-01 — Solicitud exitosa

**Dado que** el usuario está en `/auth/password-recovery` (paso 1)
**Cuando** ingresa el correo de una cuenta existente
**Entonces** el sistema envía el código, muestra el destino enmascarado y avanza
al paso de confirmación (HU-06).

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Datos inválidos | `AUTH_014` — "La solicitud contiene datos inválidos o incompletos". |
| FA-02 | Demasiadas solicitudes | `AUTH_013`. |
| FA-03 | Error inesperado | "Ocurrió un error inesperado. Intenta nuevamente." (UI). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| — | Pendiente por definir (no documentados RNF específicos en la fuente para esta HU). |

---

# Dependencias

- AWS Cognito con `forgotPassword` habilitado y SES configurado (RT-01).

---

# Aclaraciones

- Contrato observado (Origen: HU/PDF): `POST /api/v1/auth/password-recovery`;
  request `{ email }`; response `{ codeDeliveryDetails }`.
- RECOMENDACIÓN — requiere aprobación (INC-06): por seguridad (evitar enumeración
  de usuarios) suele preferirse una respuesta genérica que no revele si el correo
  existe; hoy el resultado puede diferenciar cuenta existente de inexistente. A
  confirmar (MEN-005). No es criterio de aceptación hasta aprobarse.

---

# Fuera de alcance

- Recuperación mediante preguntas de seguridad u otros canales.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿La respuesta debe ser genérica para no revelar la existencia de la cuenta?
   (INC-06 / MEN-005).

---

# Prototipo

**Requerido:** Sí (M-04, paso 1).

Descripción: pantalla "Recuperar contraseña" (paso 1) con estados de uso,
validación y error.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos.
- [x] Validaciones definidas.
- [ ] Reglas de negocio definidas. (No se documentaron reglas propias en la fuente; el comportamiento sí está definido.)
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Respuesta genérica INC-06 pendiente — no bloqueante para el comportamiento ya construido.)

## Resultado

**Estado DoR:** Cumple (documentación de funcionalidad ya construida; el
comportamiento de enumeración es una mejora a decidir, no un bloqueo).

**Pendientes:** 1 importante (respuesta genérica, INC-06).
