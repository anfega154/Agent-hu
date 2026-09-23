# HU-04 — Reenvío del código OTP de acceso

| Campo | Valor |
|---|---|
| Identificador | HU-04 |
| Nombre | Reenvío del código OTP de acceso |
| Módulo | M1 — Autenticación y gestión de roles |
| Actor | Usuario registrado (Administrador, Coordinador o Colaborador) |
| Estado | Completada (desarrollo) · documentación reconstruida (`DEC-017`) |
| Prioridad | Media |
| Versión | 1.0 |
| Fuente principal | HU (PDF) + DOC (`compira-context.md` §14) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-03 (pantalla de verificación) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`).

---

## Resumen ágil

Como **usuario registrado** necesito **solicitar el reenvío del código OTP cuando
no lo reciba**, para **continuar el proceso de autenticación sin reiniciar todo el
ingreso**.

## Contexto funcional

En la pantalla de verificación (`/auth/verify`), si el usuario no recibe el
código, puede solicitar uno nuevo una vez transcurrido el tiempo de espera.
Mientras el contador está activo se muestra el tiempo restante; al llegar a cero
aparece la opción "Reenviar código". Módulo M1. Origen: HU (PDF) / DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Reenviar código | Acción (botón) | N/A | — | N/A | N/A | N/A | Disponible solo cuando el contador llega a 0. |
| Contador de espera | Texto informativo (mm:ss) | N/A | mm:ss | Valor inicial 120 s (configurable por `VITE_OTP_RESEND_COOLDOWN_SECONDS`) | N/A | No | Cuenta regresiva antes de habilitar el reenvío. |

---

# Validaciones funcionales

## VF-01. Disponibilidad del reenvío

**Qué se valida:** que el contador de espera haya llegado a 0.
**Cuándo:** durante la permanencia en `/auth/verify`.
**Si cumple:** se habilita "Reenviar código".
**Si no cumple:** la opción no está disponible; se muestra el tiempo restante en mm:ss.

---

# Reglas de negocio

## RN-01. Tiempo de espera del cliente

El tiempo de espera del cliente antes de permitir un nuevo reenvío es de 120
segundos por defecto, configurable por variable de entorno.
Origen: HU (PDF).

---

# Reglas de comportamiento

## RC-01. Reinicio tras reenvío

Al solicitar el reenvío, el sistema reinicia el contador a 120 segundos, limpia
los campos del código y devuelve el foco al primero.

## RC-02. Conservación del flujo ante rechazo

Si Cognito rechaza el reenvío, el sistema conserva el flujo y muestra el error sin
autenticar la sesión.

---

# Criterios de aceptación

## CA-01. Reenvío disponible tras el tiempo de espera

Cuando el contador llega a 0 y el usuario selecciona "Reenviar código", el sistema
solicita un nuevo código, reinicia el contador a 120 segundos, limpia los campos y
devuelve el foco al primero.

## CA-02. Reenvío bloqueado durante el tiempo de espera

Mientras el contador es mayor que 0, la opción de reenvío no está disponible y se
muestra el tiempo restante en formato mm:ss.

## CA-03. Rechazo del servicio

Cuando Cognito rechaza el reenvío (por ejemplo, por exceso de solicitudes), el
sistema conserva el flujo y muestra el mensaje de error correspondiente sin
autenticar la sesión.

---

# Escenarios de prueba

## CP-01 — Reenvío exitoso

**Dado que** el contador llegó a 0 en `/auth/verify`
**Cuando** el usuario selecciona "Reenviar código"
**Entonces** el sistema solicita un nuevo código, reinicia el contador a 120 s,
limpia los campos y reenfoca el primero.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Demasiadas solicitudes | `AUTH_013` — "Se recibieron demasiadas solicitudes. Inténtalo nuevamente en unos minutos". |
| FA-02 | Usuario no encontrado | `AUTH_007` — "No se encontró una cuenta asociada al usuario enviado". |
| FA-03 | Error genérico | "No se pudo reenviar el código." (UI). |

---

# Requisitos no funcionales

Documentar únicamente cuando hayan sido definidos.

| ID | Requisito |
|---|---|
| — | Pendiente por definir (no se documentaron RNF específicos en la fuente para esta HU). |

---

# Dependencias

- HU-03 (pantalla de verificación).

---

# Aclaraciones

- Contrato observado (Origen: HU/PDF): `POST /api/v1/auth/login/resend-code`;
  request `{ email }`; response `{ codeDeliveryDetails }`.
- El temporizador de 120 s es una salvaguarda del cliente; el backend no aplica
  cooldown propio, solo el límite de Cognito.
- RECOMENDACIÓN — requiere aprobación: el cooldown vive solo en el cliente; al
  recargar la página el contador se reinicia a 120 s. Evaluar si es aceptable o si
  debe persistirse (MEN-005). No es criterio de aceptación hasta aprobarse.

---

# Fuera de alcance

- Reenvío por SMS.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Debe persistirse el contador de reenvío tras recargar la página? ¿Existe un
   número máximo de reenvíos? (MEN-005).

---

# Prototipo

**Requerido:** Sí (representado dentro de M-03: estados "esperando" y "reenvío
disponible").

Descripción: variación de la pantalla de verificación con el contador activo y con
el reenvío habilitado.

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

**Pendientes:** 1 importante (persistencia del contador / máximo de reenvíos).
