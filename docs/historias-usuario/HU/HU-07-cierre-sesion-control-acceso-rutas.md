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
| Dependencias | HU-02 / HU-03 (sesión previamente establecida); cierre de sesión en el proveedor de identidad (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`).

---

## Resumen ágil

Como **usuario autenticado** necesito **cerrar mi sesión y mantener protegidas las
rutas internas**, para **evitar accesos no autorizados y conservar la seguridad de
mi información**.

## Contexto funcional

Mientras exista una sesión activa en el navegador, el usuario puede navegar por las
pantallas internas de la plataforma. Al cerrar sesión, el sistema borra los datos de
la sesión en el navegador y cierra la sesión también en el proveedor de identidad.
Sin sesión activa, cualquier pantalla interna redirige al inicio de sesión. Módulo
M1. Origen: HU (PDF) / DOC.

> Nota de trazabilidad: entre las pantallas internas figura la de eliminación de
> usuarios (HU-09), hoy `Descartada` (`DEC-019`) con discrepancia abierta `IMP-012`.
> Su vigencia depende de resolver `IMP-012`.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Cerrar sesión | Acción | N/A | — | N/A | N/A | N/A | Borra la sesión del navegador y cierra la sesión en el proveedor de identidad. |
| Sesión del navegador | Datos de sesión | Sí | — | N/A | Datos de acceso e identidad del usuario | No | Se mantienen mientras la pestaña/navegador esté abierto (ver Aclaraciones para el detalle técnico). |

---

# Validaciones funcionales

## VF-01. Existencia de sesión activa

**Qué se valida:** que exista una sesión activa en el navegador.
**Cuándo:** al acceder a una pantalla interna.
**Si cumple:** permite el acceso a la pantalla.
**Si no cumple:** redirige a la pantalla de inicio de sesión.

---

# Reglas de negocio

## RN-01. La sesión no persiste entre cierres del navegador

La sesión se mantiene solo mientras la pestaña o el navegador estén abiertos; al
cerrarlos, se pierde y el usuario debe volver a iniciar sesión.
Origen: HU (PDF).

---

# Reglas de comportamiento

## RC-01. Cierre resiliente ante fallo

Cuando el cierre en el proveedor de identidad falla, el sistema mantiene la sesión
cerrada en el navegador de todas formas (no reexpone la sesión al usuario).

---

# Criterios de aceptación

## CA-01. Acceso a pantallas internas con sesión activa

Cuando el usuario tiene una sesión activa, puede acceder a las pantallas internas
mientras la sesión exista en el navegador.

## CA-02. Redirección sin sesión activa

Cuando el usuario no tiene sesión activa e intenta abrir una pantalla interna, el
sistema lo redirige a la pantalla de inicio de sesión.

## CA-03. Cierre de sesión

Cuando el usuario cierra sesión, el sistema borra los datos de la sesión en el
navegador, lo marca como no autenticado y cierra la sesión en el proveedor de
identidad.

## CA-04. Cierre resiliente ante fallo

Cuando el cierre en el proveedor de identidad falla, el sistema mantiene la sesión
cerrada en el navegador de todas formas.

---

# Escenarios de prueba

## CP-01 — Cierre de sesión exitoso

**Dado que** el usuario tiene sesión activa
**Cuando** selecciona "Cerrar sesión"
**Entonces** el sistema borra los datos de la sesión en el navegador, lo marca como
no autenticado y cierra la sesión en el proveedor de identidad.

## CP-02 — Acceso sin sesión

**Dado que** no hay sesión activa
**Cuando** el usuario intenta abrir una pantalla interna
**Entonces** el sistema redirige al inicio de sesión.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | El cierre en el proveedor de identidad falla | La sesión del navegador ya fue borrada; el usuario queda con la sesión cerrada igualmente. |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: la sesión no persiste entre cierres del navegador. |

---

# Dependencias

- HU-02 / HU-03 (sesión previamente establecida).
- Cierre de sesión habilitado en el proveedor de identidad (RT-01).

---

# Aclaraciones

> Nota de lenguaje: el cuerpo usa lenguaje de negocio; los detalles técnicos se
> concentran aquí para backend/QA.

- **Nota técnica (backend/QA):** la sesión del navegador se guarda en
  `sessionStorage` con las claves `compira_access_token`, `compira_id_token`,
  `compira_refresh_token`, `compira_user` (por eso no persiste entre cierres). El
  cierre en el proveedor de identidad (AWS Cognito) es `globalSignOut`. La protección
  de pantallas es del lado del cliente (`ProtectedRoute`, redirige a `/auth/login`).
- **Contrato observado (Origen: HU/PDF):** `POST /api/v1/auth/logout`; request
  `{ accessToken }`; response `204`.
- La autorización real de cada operación protegida depende del token enviado al
  servidor — ligado a la deuda `DEC-010` (validación de token/rol en servidor).
- RECOMENDACIÓN — requiere aprobación: no existe refresco automático de la sesión ni
  control global de expiración durante la navegación (MEN-005). No es criterio de
  aceptación hasta aprobarse.

---

# Fuera de alcance

- Cierre de sesión en todos los dispositivos desde una pantalla de gestión.
- Expiración automática por inactividad.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Qué debe ocurrir cuando la sesión expira durante la navegación (redirección
   automática al inicio de sesión vs renovación automática de la sesión)? (MEN-005).
2. ¿Se requiere expiración de sesión por inactividad? (MEN-005).

---

# Prototipo

**Requerido:** Parcial.

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-07-cierre-sesion-rutas.svg` — ubicación del
control de cierre de sesión y redirección al login sin sesión. Reemplaza "M-05" del
informe. El detalle del contenedor autenticado (AppShell) queda fuera de foco. El
diseño visual final es decisión de UX del equipo.

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
