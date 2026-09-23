# HU-07 — Cierre de sesión y control de acceso a rutas protegidas

| Campo | Valor |
|---|---|
| Identificador | HU-07 |
| Nombre | Cierre de sesión y control de acceso a rutas protegidas |
| Módulo | M1 — Autenticación y gestión de roles |
| Actor | Usuario autenticado del sistema |
| Estado | Completada (desarrollo) · documentación reconstruida (`DEC-017`) |
| Prioridad | Alta |
| Versión | 1.0 |
| Fuente principal | HU (PDF) + DOC (`compira-context.md` §14) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-02 / HU-03 (sesión previamente establecida); AWS Cognito `globalSignOut` (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`).

---

## Resumen ágil

Como **usuario autenticado** necesito **cerrar mi sesión y mantener protegidas las
rutas internas**, para **evitar accesos no autorizados y conservar la seguridad de
mi información**.

## Contexto funcional

Mientras exista una sesión válida en el almacenamiento del navegador, el usuario
puede navegar por las rutas internas (`/`, `/users/register`, `/users/delete`). Al
cerrar sesión, el sistema limpia los datos locales de autenticación y notifica a
Cognito (`globalSignOut`). Sin sesión válida, cualquier ruta protegida redirige al
login. Módulo M1. Origen: HU (PDF) / DOC.

> Nota de trazabilidad: `/users/delete` corresponde a la capacidad de eliminación
> de usuarios (HU-09), hoy `Descartada` (`DEC-019`) con discrepancia abierta
> `IMP-012`. La ruta se documenta tal como figura en la evidencia; su vigencia
> depende de resolver `IMP-012`.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Cerrar sesión | Acción | N/A | — | N/A | N/A | N/A | Limpia la sesión local e invoca el cierre en Cognito. |
| Sesión almacenada | Datos en `sessionStorage` | Sí | Claves | N/A | `compira_access_token`, `compira_id_token`, `compira_refresh_token`, `compira_user` | No | Datos de sesión del navegador. |

---

# Validaciones funcionales

## VF-01. Existencia de sesión válida

**Qué se valida:** que exista una sesión válida en el almacenamiento del navegador.
**Cuándo:** al acceder a una ruta protegida.
**Si cumple:** permite el acceso a la vista protegida.
**Si no cumple:** redirige a la pantalla de inicio de sesión.

---

# Reglas de negocio

## RN-01. Almacenamiento en sessionStorage

La sesión se almacena en `sessionStorage`, por lo que se pierde al cerrar la
pestaña o el navegador.
Origen: HU (PDF).

---

# Reglas de comportamiento

## RC-01. Cierre local resiliente

Cuando el cierre en Cognito (`globalSignOut`) falla, el sistema mantiene la sesión
cerrada localmente de todas formas (no reexpone la sesión al usuario).

---

# Criterios de aceptación

## CA-01. Acceso a rutas protegidas con sesión activa

Cuando el usuario tiene una sesión válida, puede acceder a las vistas protegidas
mientras la sesión exista en el almacenamiento del navegador.

## CA-02. Redirección sin sesión válida

Cuando el usuario no tiene sesión válida e intenta abrir una ruta protegida, el
sistema lo redirige a la pantalla de inicio de sesión.

## CA-03. Cierre de sesión

Cuando el usuario cierra sesión, el sistema elimina las cuatro claves de
autenticación del `sessionStorage`, actualiza el estado a no autenticado y solicita
a Cognito el cierre global de la sesión.

## CA-04. Cierre local resiliente ante fallo del backend

Cuando el cierre en Cognito falla, el sistema mantiene la sesión cerrada localmente
de todas formas.

---

# Escenarios de prueba

## CP-01 — Cierre de sesión exitoso

**Dado que** el usuario tiene sesión activa
**Cuando** selecciona "Cerrar sesión"
**Entonces** el sistema elimina las cuatro claves del `sessionStorage`, marca no
autenticado y solicita `globalSignOut` a Cognito.

## CP-02 — Acceso sin sesión

**Dado que** no hay sesión válida
**Cuando** el usuario intenta abrir una ruta protegida
**Entonces** el sistema redirige al login.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | `globalSignOut` falla en Cognito | La sesión local ya fue limpiada; el usuario queda deslogueado igualmente. |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: al no usar `localStorage`, la sesión no persiste entre cierres del navegador. |

---

# Dependencias

- HU-02 / HU-03 (sesión previamente establecida).
- AWS Cognito `globalSignOut` (RT-01).

---

# Aclaraciones

- Contrato observado (Origen: HU/PDF): `POST /api/v1/auth/logout`; request
  `{ accessToken }`; response `204`. Protección de rutas del lado del cliente
  mediante `ProtectedRoute` (redirige a `/auth/login` si no hay usuario en contexto).
- La protección de rutas es del lado del cliente. La autorización real de cada
  operación protegida depende del `accessToken` enviado al backend — ligado a la
  deuda `DEC-010` (validación de token/rol en servidor).
- RECOMENDACIÓN — requiere aprobación: no existe refresco automático de tokens ni
  interceptor global de expiración durante la navegación (MEN-005). No es criterio
  de aceptación hasta aprobarse.

---

# Fuera de alcance

- Cierre de sesión en todos los dispositivos desde una pantalla de gestión.
- Expiración automática por inactividad.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Qué debe ocurrir cuando el `accessToken` expira durante la navegación
   (redirección automática al login vs refresco de token)? (MEN-005).
2. ¿Se requiere expiración de sesión por inactividad? (MEN-005).

---

# Prototipo

**Requerido:** Parcial (M-05: ubicación del control de cierre de sesión y estado
de redirección al login).

Descripción: control de cierre de sesión y comportamiento de redirección de rutas
protegidas.

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
- [ ] Sin supuestos funcionales críticos. (Manejo de expiración de token pendiente — no bloqueante para el comportamiento ya construido; ligado a `DEC-010`.)

## Resultado

**Estado DoR:** Cumple (documentación de funcionalidad ya construida).

**Pendientes:** 2 importantes (expiración/refresco de token; expiración por
inactividad).
