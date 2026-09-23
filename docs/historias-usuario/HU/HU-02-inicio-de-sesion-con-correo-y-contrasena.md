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
| Dependencias | Proveedor de identidad configurado para el inicio de sesión (RT-01) |

> Reconstrucción documental según `hu-template.md` (`DEC-017`). Contenido de
> evidencia real; lo no confirmado se marca `Pendiente por definir`.

---

## Resumen ágil

Como **usuario registrado** necesito **iniciar sesión con mi correo y mi
contraseña**, para **acceder a la plataforma de forma segura y continuar mis
actividades según el rol asignado**.

## Contexto funcional

Es el punto de entrada a la plataforma. El usuario ingresa su correo y su
contraseña; el sistema verifica las credenciales. Según el resultado, el sistema
puede: (a) conceder el acceso directamente; (b) requerir el cambio de contraseña
obligatorio si es su primer ingreso (HU-01); o (c) solicitar un ingreso más seguro
en dos pasos mediante un código (HU-03). Módulo M1; actor: cualquier rol; resultado:
acceso concedido o continuación con el paso que corresponda. Origen: HU (PDF) / DOC.

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

**Qué se valida:** que el correo y la contraseña correspondan a una cuenta válida.
**Cuándo:** al enviar el formulario.
**Si cumple:** el sistema concede el acceso o continúa con el paso que corresponda
(cambio de contraseña obligatorio o ingreso en dos pasos).
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

## RC-01. Continuación según el resultado del ingreso

Según el resultado, el sistema: concede el acceso a la pantalla principal, o lleva
al cambio de contraseña obligatorio (HU-01), o a la verificación en dos pasos (HU-03).

---

# Criterios de aceptación

## CA-01. Acceso directo con credenciales válidas

Cuando el usuario ingresa credenciales válidas y no se requiere ningún paso
adicional, el sistema inicia la sesión y lo lleva a la pantalla principal.

## CA-02. Continuación al cambio obligatorio de contraseña

Cuando la cuenta requiere cambiar la contraseña (primer ingreso), el sistema lleva
al usuario a la pantalla de nueva contraseña (HU-01).

## CA-03. Continuación a la verificación en dos pasos

Cuando la cuenta usa verificación en dos pasos, el sistema lleva al usuario a la
pantalla de verificación para completar un ingreso más seguro con un código (HU-03).

## CA-04. Rechazo por credenciales inválidas

Cuando el correo o la contraseña son incorrectos, el sistema muestra `AUTH_005`
("Las credenciales ingresadas no son válidas") y no otorga acceso.

## CA-05. Rechazo por estado de la cuenta

Cuando la cuenta no está confirmada, muestra `AUTH_006`. Cuando la cuenta debe
restablecer la contraseña antes de ingresar, muestra `AUTH_008`.

---

# Escenarios de prueba

## CP-01 — Acceso directo válido

**Dado que** el usuario tiene credenciales válidas y no se requiere paso adicional
**Cuando** envía correo y contraseña correctos
**Entonces** el sistema inicia la sesión y muestra la pantalla principal.

## CP-02 — Desvío a OTP

**Dado que** la cuenta usa verificación en dos pasos
**Cuando** las credenciales son válidas
**Entonces** el sistema lleva al usuario a la pantalla de verificación (HU-03).

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
| RNF-01 | Seguridad: verificación en dos pasos habilitada; la contraseña no se almacena en el cliente. |
| RNF-02 | Trazabilidad: el sistema registra el resultado del ingreso con el correo enmascarado. |

---

# Dependencias

- Proveedor de identidad configurado para el inicio de sesión con correo y contraseña (RT-01).

---

# Aclaraciones

> Nota de lenguaje: el cuerpo usa lenguaje de negocio; los nombres técnicos del
> proveedor de identidad (AWS Cognito) se concentran aquí para backend/QA.

- **Nota técnica (backend/QA):** proveedor de identidad AWS Cognito, flujo
  `USER_PASSWORD_AUTH` (operación `InitiateAuth`). "Requerir cambio de contraseña" =
  `CHALLENGE_REQUIRED` + `NEW_PASSWORD_REQUIRED`; "verificación en dos pasos" =
  `EMAIL_OTP` (o `SMS_MFA`); "acceso concedido" = `AUTHENTICATED`. Ruta: `/auth/login`.
- **Contrato observado (Origen: HU/PDF):** `POST /api/v1/auth/login`; request
  `{ email, password }`; response `{ status, user, tokens, challenge }`.
- El campo correo usa `type=email` pero el formulario no valida el formato en el
  cliente; la validación definitiva la realiza el servidor.

---

# Fuera de alcance

- Inicio de sesión con proveedores externos (Google, SSO, etc.).
- Selección manual del canal de verificación y verificación por SMS: soportado en el
  backend pero la interfaz actual solo ofrece el código por correo.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿Existe un límite de intentos fallidos con bloqueo temporal, o el control queda
   delegado al proveedor de identidad? (MEN-005 / bloqueo por intentos).
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
