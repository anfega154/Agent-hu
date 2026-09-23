# 00 — Análisis funcional global de COMPIRA

> Documento de la **Fase 1 (Análisis del proyecto)** del flujo
> `Analizar → Proponer → Revisar → Aprobar → Generar → Validar`.
>
> Contiene el mapa funcional del proyecto y la **revisión estructural de las HU
> existentes** (sección 14). **No** desarrolla Historias de Usuario completas.
> Toda funcionalidad proviene de las fuentes documentales; lo que no tiene
> evidencia se marca como `Pendiente por definir` y se detalla en `PENDIENTES.md`.
>
> Fuentes base: `.kiro/steering/compira-context.md` (resumen del documento oficial
> `docs/proyecto/COMPIRA_TDG_Desarrollo_SW_InformeTecnico_V1.docx`),
> `docs/proyecto/COMPIRA_Historias_Usuario.pdf` (HU-01..HU-09 ya elaboradas),
> `hu-rules.md`, `hu-template.md`, `GLOSARIO.md`, `DECISIONES.md`.
>
> | Campo | Valor |
> |---|---|
> | Versión | 2.0 |
> | Última actualización | 2026-09-19 |
> | Estado | Borrador para validación del equipo / Product Owner |
>
> Cambios v2.0: se incorpora la revisión estructural de HU-01..HU-09 contra la
> plantilla oficial (§14) y se distingue **estado de desarrollo** (software) de
> **conformidad documental** (ajuste a la plantilla).

---

## 1. Objetivos (marco de trazabilidad)

Los objetivos específicos de COMPIRA se usan como raíz de la matriz de cobertura
(`02-matriz-cobertura.md`). Origen: DOC.

| ID | Objetivo | Naturaleza |
|---|---|---|
| OG | Desarrollar una plataforma web que centralice la información y facilite la gestión de tareas diarias en las pymes. | Objetivo general |
| OE1 | Analizar los sistemas de gestión actuales e identificar limitaciones de los canales dispersos. | Análisis (no genera software directamente) |
| OE2 | Identificar requisitos y módulos, plasmándolos en el backlog y las HU. | Análisis / backlog |
| OE3 | Desarrollar los módulos centrales bajo flujo ágil (Scrum + Kanban). | Construcción de software |
| OE4 | Validar la plataforma mediante pruebas funcionales y de usabilidad. | Validación / QA |

> Nota de alcance: OE1 y OE2 son actividades del proyecto, no funcionalidades del
> producto. El software entregable (HU) se concentra en **OE3**; OE4 aporta
> criterios de aceptación y validación, no capacidades nuevas. Los cinco módulos
> funcionales (M1–M5) son la materialización de OE3.

---

## 2. Problema que resuelve (contexto funcional)

Las pymes gestionan tareas en canales dispersos (WhatsApp, correo, hojas de
cálculo, herramientas varias), lo que genera pérdida de trazabilidad, dificultad
para conocer responsables y estado real, retrasos, duplicidad y carga operativa.
COMPIRA centraliza el **ciclo de vida de la tarea** en una única plataforma
accesible, intuitiva y de bajo costo de adopción. Origen: DOC.

Principio funcional clave: **ningún módulo funciona aislado**; una acción
desencadena la siguiente sin intervención manual
(asignación → notificación → seguimiento → cierre → histórico → indicadores).
Origen: DOC.

---

## 3. Mapa de actores

Tres roles son la base de la gobernanza por permisos (RBAC). Cada HU debe
asociarse a uno de estos actores. Origen: DOC. Terminología alineada con
`GLOSARIO.md` (no usar sinónimos).

| Actor | Descripción | Responsabilidad / interacción principal |
|---|---|---|
| Administrador del sistema | Configuración general de la organización; único con permiso para gestionar cuentas, roles y configuraciones críticas. | Gestión de usuarios, roles, configuraciones, reportes de productividad, monitoreo del panel. |
| Coordinador de equipo | Actor operativo (líder de proyecto) que supervisa el trabajo de su equipo. | Crear/asignar/reasignar/cancelar tareas, fechas límite, seguimiento, alertas de desempeño, aprobación/cierre cuando aplique. |
| Colaborador | Operador que ejecuta las tareas asignadas. | Consultar tareas, recibir notificaciones/recordatorios, actualizar estado, registrar observaciones, documentar ejecución. |

> Nota de terminología: el PDF de HU usa "usuario registrado del sistema
> (administrador, coordinador o colaborador)" y los códigos de rol
> `ADMINISTRATOR`, `COORDINATOR`, `COLLABORATOR`. Se mantiene la terminología del
> GLOSARIO; los códigos técnicos se registran como equivalencia (ver §14 y
> GLOSARIO pendiente de ampliar).

Actores no humanos / técnicos identificados en las fuentes:

| Actor | Tipo | Rol funcional |
|---|---|---|
| Sistema COMPIRA | Proceso automático | Encadena asignación → notificación → seguimiento → histórico → indicadores; genera recordatorios y alertas. Origen: DOC. |
| AWS Cognito | Servicio externo de identidad | Autentica, emite tokens (acceso, identidad, refresco), gestiona MFA/OTP y cambio obligatorio de contraseña. Origen: DEC/RT (contexto §12; ADR-10) y HU-01..HU-09. |

Fronteras de permisos explícitas (Origen: DOC):

- El Coordinador **no** gestiona usuarios ni configuración general.
- El Colaborador **no** crea tareas ni modifica configuración general.

---

## 4. Mapa de módulos

Origen: DOC (sección 6 del contexto).

| Módulo | Nombre | Propósito funcional | Actor(es) principal(es) |
|---|---|---|---|
| M1 | Autenticación y gestión de roles | Acceso seguro, sesión, recuperación de contraseña, RBAC (qué ve/hace cada rol). | Todos / Administrador (gestión). |
| M2 | Gestión de tareas | Ciclo de vida de la tarea: creación, asignación, estados, reasignación, cancelación, cierre, historial, trazabilidad. | Coordinador, Colaborador. |
| M3 | Recordatorios automáticos | Notificaciones en plataforma ante eventos de tarea (asignación, reasignación, próximo vencimiento, retraso). | Sistema → Colaborador / Coordinador. |
| M4 | Panel de seguimiento | Vista consolidada en tiempo real: estados, avance, vencidas, próximas a vencer, cumplimiento, con filtros. | Coordinador, Administrador. |
| M5 | Administración del sistema | Gestión de usuarios y roles, configuraciones generales (zona horaria, notificaciones), reportes de productividad. | Administrador. |

---

## 5. Capacidades funcionales por módulo

Descomposición **Módulo → Capacidad → Funcionalidad**. Todo lo listado proviene
de DOC salvo indicación. Para M1, el detalle proviene además del PDF de HU
(Origen: HU). Cuando un detalle no está definido, se marca `Pendiente por definir`.

### M1 — Autenticación y gestión de roles

| Capacidad | Funcionalidades identificadas | Origen | Observaciones |
|---|---|---|---|
| C1.1 Autenticación | Inicio de sesión con correo y contraseña; verificación en dos pasos (OTP por correo); reenvío de OTP. | DOC / HU (HU-02, HU-03, HU-04) | Flujo `USER_PASSWORD_AUTH`; segundo factor `EMAIL_OTP`, código 6 dígitos; reenvío a 120 s. Origen: HU. |
| C1.2 Activación de cuenta | Cambio obligatorio de contraseña en el primer inicio de sesión. | DOC / HU (HU-01) | Reto `NEW_PASSWORD_REQUIRED`; contraseña UI 10–128, backend valida 8–128 (inconsistencia, ver INC-05). |
| C1.3 Recuperación de contraseña | Solicitud de recuperación por correo; confirmación de restablecimiento con código. | DOC / HU (HU-05, HU-06) | `forgotPassword` / `confirmForgotPassword`. |
| C1.4 Control de sesión | Cierre de sesión; protección de rutas internas. | DOC / HU (HU-07) | Sesión en `sessionStorage`; protección de rutas del lado cliente; `globalSignOut`. |
| C1.5 Control de acceso por rol (RBAC) | Determinar qué módulos/opciones ve y ejecuta cada rol. | DOC | Deuda técnica ADR-10 y preguntas pendientes del PDF: el backend no confirma validación de rol `ADMINISTRATOR` en operaciones de administración. |

### M2 — Gestión de tareas

| Capacidad | Funcionalidades identificadas | Origen | Observaciones |
|---|---|---|---|
| C2.1 Creación de tarea | Crear tarea con título, descripción y fechas. | DOC | Reglas exactas de campos: Pendiente por definir. |
| C2.2 Asignación | Asignar tarea a uno o varios colaboradores. | DOC | Asignación múltiple confirmada por DOC. |
| C2.3 Reasignación | Cambiar el/los responsables de una tarea. | DOC | |
| C2.4 Consulta de tareas | Colaborador consulta sus tareas asignadas. | DOC | |
| C2.5 Actualización de estado | Actualizar estado (pendiente, en progreso, completada). | DOC | Conjunto exacto y transiciones: Pendiente por definir. |
| C2.6 Observaciones / documentación | Registrar observaciones y documentar la ejecución. | DOC | Formato/adjuntos: Pendiente por definir. |
| C2.7 Cancelación | Cancelar una tarea (Coordinador). | DOC | |
| C2.8 Aprobación | Aprobar actividad cuando aplique (Coordinador). | DOC | "Cuando aplique": condición no definida. Pendiente. |
| C2.9 Cierre | Cerrar actividad cuando aplique (Coordinador). | DOC | Diferencia funcional completar/cerrar: Pendiente. |
| C2.10 Historial / trazabilidad | Registrar el histórico de cambios de la tarea. | DOC | Persistencia con integridad ACID (M2). |

### M3 — Recordatorios automáticos

| Capacidad | Funcionalidades identificadas | Origen | Observaciones |
|---|---|---|---|
| C3.1 Notificación de asignación | Avisar al colaborador cuando se le asigna una tarea. | DOC | |
| C3.2 Notificación de reasignación | Avisar cuando una tarea es reasignada. | DOC | |
| C3.3 Aviso de próximo vencimiento | Avisar cuando una tarea está próxima a vencer. | DOC | Umbral de "próximo": Pendiente por definir. |
| C3.4 Alerta de retraso | Alertar al coordinador cuando una tarea está retrasada. | DOC | |

> Alcance de M3: las notificaciones son **dentro de la plataforma / en tiempo
> real**. Notificaciones por correo/push externas: Pendiente / posible fuera de
> alcance. Origen: DOC.

### M4 — Panel de seguimiento

| Capacidad | Funcionalidades identificadas | Origen | Observaciones |
|---|---|---|---|
| C4.1 Vista consolidada | Estado, avance y niveles de cumplimiento del equipo. | DOC | Debe actualizarse en tiempo real sin recarga completa. |
| C4.2 Tareas vencidas | Visualizar tareas vencidas. | DOC | |
| C4.3 Tareas próximas a vencer | Visualizar tareas próximas a vencer. | DOC | |
| C4.4 Filtros | Filtrar por usuario/colaborador, estado y fecha. | DOC | |
| C4.5 Carga de trabajo | Consultar tareas asignadas por responsable. | DOC (funciones del Administrador) | Solapa con reportes de M5; ver dependencias e INC-02. |
| C4.6 Indicadores de cumplimiento | Niveles/porcentaje de cumplimiento. | DOC | Fórmulas de cálculo: Pendiente por definir. |

### M5 — Administración del sistema

| Capacidad | Funcionalidades identificadas | Origen | Observaciones |
|---|---|---|---|
| C5.1 Gestión de usuarios | Registrar, editar y remover usuarios; restablecer contraseñas. | DOC / HU (HU-08 registrar, HU-09 remover) | Editar usuario: fuera de alcance explícito en HU-08; no hay HU. Restablecer por admin: no hay HU (distinto de HU-05/06). |
| C5.2 Gestión de roles | Asignar y modificar roles. | DOC | HU-08 asigna rol al crear; modificar rol de usuario existente: no hay HU. |
| C5.3 Configuración de notificaciones | Configurar notificaciones. | DOC | Parámetros configurables: Pendiente por definir. |
| C5.4 Configuración de zona horaria | Configurar zona horaria. | DOC | Alcance (organización vs usuario): Pendiente. |
| C5.5 Reportes de productividad | Cumplimiento, retraso, carga de trabajo, tiempos de cierre, ciclo e histórico. | DOC | Definiciones de cálculo: Pendiente. |

---

## 6. Funcionalidades identificadas (consolidado)

Lista consolidada de funcionalidades candidatas a HU (la granularidad final se
decide en el inventario). Origen DOC salvo nota.

- M1: cambio obligatorio de contraseña; login correo+contraseña; OTP por correo;
  reenvío de OTP; solicitud de recuperación; confirmación de restablecimiento;
  cierre de sesión y protección de rutas; aplicación de permisos por rol (RBAC).
- M2: crear tarea; asignar a uno o varios; reasignar; consultar tareas asignadas;
  actualizar estado; registrar observaciones/documentar; cancelar; aprobar (cuando
  aplique); cerrar (cuando aplique); historial de cambios.
- M3: notificar asignación; notificar reasignación; avisar próximo vencimiento;
  alertar retraso.
- M4: vista consolidada; tareas vencidas; próximas a vencer; filtros; carga de
  trabajo por responsable; indicadores de cumplimiento.
- M5: registrar usuario; editar usuario; remover usuario; restablecer contraseña;
  asignar/modificar roles; configurar notificaciones; configurar zona horaria;
  reportes de productividad.

---

## 7. Reglas transversales conocidas (RT)

Reglas aplicables a múltiples HU. Candidatas a RT; su aprobación formal como RT
corresponde al equipo (`DECISIONES.md`). No se inventan valores no documentados.

| RT (propuesta) | Descripción | Origen | Estado |
|---|---|---|---|
| RT-01 Autenticación e identidad | El acceso se autentica mediante AWS Cognito (tokens emitidos por Cognito; el backend no genera JWT propios). Flujo `USER_PASSWORD_AUTH`, MFA `EMAIL_OTP`, cambio obligatorio `NEW_PASSWORD_REQUIRED`. | DEC/RT (contexto §12, ADR-10) / HU | Restricción técnica aprobada. |
| RT-02 Autorización por rol (RBAC) | Cada actor ve y ejecuta únicamente lo que corresponde a su rol. | DOC | Deuda técnica: validación de token/rol en servidor pendiente (ADR-10; preguntas HU-08/HU-09). |
| RT-03 Trazabilidad / historial | El ciclo de vida de la tarea se registra con historial de cambios e integridad ACID. | DOC | Alcance de auditoría: Pendiente (también para operaciones de usuario, ver HU-09). |
| RT-04 Actualización en tiempo real | El panel y las notificaciones se actualizan en tiempo real sin recarga completa. | DOC | Mecanismo técnico: fuera de la HU. |
| RT-05 Zona horaria | Las fechas se interpretan según la zona horaria configurada. | DOC | Alcance (global/usuario) y formato: Pendiente. |
| RT-06 Convenciones de contraseña/OTP y errores | Contraseña 10–128 en UI (backend 8–128, ver INC-05); OTP de 6 dígitos; reenvío a 120 s; catálogo de errores `AUTH_001..AUTH_014`. | HU | Observado en HU M1; unificación de mínimo de contraseña pendiente. |
| RT-07 Acceso responsivo por navegador | Acceso solo vía navegador con diseño responsivo (sin app móvil nativa). | DOC | Restricción de alcance. |

> Recomendación (REC): formalizar RT-06 con el **catálogo de códigos de error
> `AUTH_00x`** ya usado en las HU. Requiere aprobación.

---

## 8. Dependencias funcionales

| Dependencia | Descripción | Impacto |
|---|---|---|
| M2, M3, M4, M5 → M1 | Toda funcionalidad requiere autenticación y control de acceso por rol. | Bloqueante de ejecución. |
| M3 → M2 | Los recordatorios se disparan por eventos del ciclo de la tarea. | M3 no tiene sentido sin M2. |
| M4 → M2 | El panel consolida estados, avance y vencimientos de las tareas. | M4 depende de datos de M2. |
| M4 → M3 | Alertas de retraso/próximo vencimiento se relacionan con la lógica de avisos. | Coordinación de umbrales. |
| M5 (reportes) → M2 | Los reportes se calculan sobre tareas y su historial. | Requiere datos de M2. |
| M4.5 ↔ M5.5 | "Carga de trabajo por responsable" aparece en M4 y en M5. | Posible solapamiento (INC-02). |
| M1 (identidad) → AWS Cognito | La identidad depende de Cognito (fija AWS como proveedor de identidad). | Restricción técnica; ADR-06 abierto. |
| HU-01 → HU-08 | El cambio obligatorio de contraseña depende del registro con contraseña temporal. | Cadena de activación de cuenta. Origen: HU. |
| HU-03 → HU-02/HU-01 | El OTP se origina en el login o en el cambio de contraseña. | Origen: HU. |
| HU-04 → HU-03 | El reenvío de OTP depende de la pantalla de verificación. | Origen: HU. |
| HU-06 → HU-05 | La confirmación de restablecimiento depende de la solicitud. | Origen: HU. |

No se inventan dependencias adicionales.

---

## 9. Contradicciones e inconsistencias encontradas

Registradas, no resueltas (regla de contradicciones). Cada una genera pendiente.

| ID | Descripción | Fuentes en tensión | Tratamiento |
|---|---|---|---|
| INC-01 | Autenticación: stack propuesto (TDG I / ADR-05) define JWT propio del backend; stack implementado (TDG II / ADR-10) delega en Cognito y no firma JWT propios. | Contexto §11/ADR-05 vs §12/ADR-10 + HU | Prevalece la decisión posterior (Cognito) por prioridad de fuentes; se documenta. |
| INC-02 | "Carga de trabajo por responsable" figura como reporte del Administrador (M5) y como vista del panel (M4). | DOC (Administrador) vs DOC (M4) | No se decide; pendiente P-06. |
| INC-03 | RBAC: el diseño exige RBAC (RT-02), pero no se confirma validación de token/rol en el servidor (deuda ADR-10; preguntas HU-08/HU-09). | Diseño vs estado implementado | Pendiente P-14. |
| INC-04 | Proveedor de nube: ADR-06 abierto, pero la identidad ya fija AWS por Cognito (ADR-10). | ADR-06 vs ADR-10 | Decisión de infraestructura pendiente. |
| INC-05 | Longitud mínima de contraseña: la UI exige 10 y el backend valida 8. | HU-01 / HU-06 (Aclaraciones) | Pendiente P-19; unificar mínimo. |
| INC-06 | Recuperación de contraseña: hoy el resultado puede diferenciar cuenta existente de inexistente, lo que permite enumeración de usuarios; la práctica de seguridad sugiere respuesta genérica. | HU-05 (Aclaraciones) | Pendiente P-20; decisión de comportamiento. |

---

## 10. Información faltante (resumen)

Detalle y clasificación en `PENDIENTES.md`. Vacíos detectados (no se inventan
respuestas):

- Conjunto exacto y transiciones válidas de **estados de tarea** (M2).
- Condiciones de **aprobación** y diferencia **completar/cerrar** (M2).
- Reglas de **campos de la tarea** (obligatoriedad, formatos, fechas).
- Reglas de **observaciones/documentación** (M2).
- **Umbrales** de "próximo a vencer" y "retraso" (M3/M4).
- **Fórmulas** de indicadores, tiempos de cierre y reportes (M4/M5).
- Ubicación de **carga de trabajo** (M4 vs M5) — INC-02.
- Parámetros de **notificaciones** y alcance de **zona horaria** (M5).
- Alcance de **auditoría/historial** (M2 y operaciones de usuario).
- Existencia de HU para **editar usuario** y **modificar rol** (no hay HU).
- Detalles de identidad pendientes del PDF: política exacta de contraseñas de
  Cognito, vigencia de OTP/código de recuperación, bloqueo por intentos, manejo
  de expiración/refresco de token, generación/validez de contraseña temporal.

---

## 11. Notas de alcance (fuera de alcance confirmado)

Origen: DOC (§7). No convertir en HU:

- Aplicación móvil nativa (solo navegador responsivo).
- Implementación productiva con datos reales.
- Diseño UI/UX avanzado más allá de prototipos funcionales.
- Estrategias comerciales y lanzamiento comercial.

Fuera de alcance declarado en las HU del PDF (Origen: HU): login con proveedores
externos/SSO; verificación por SMS y selección de canal MFA; edición de usuarios
(HU-08); soft delete y eliminación masiva (HU-09); expiración de contraseña
periódica y cambio voluntario desde perfil (HU-01).

Cualquier funcionalidad que roce estos límites se marca como
`POSIBLE AMPLIACIÓN DE ALCANCE — requiere validación`.

---

## 12. Recomendaciones del agente (no son requisitos)

> RECOMENDACIÓN — requiere aprobación.

- REC-01: Definir un catálogo único de estados de tarea y transiciones antes de M2.
- REC-02: Decidir dónde vive "carga de trabajo por responsable" (INC-02).
- REC-03: Priorizar la deuda técnica de validación de token/rol en servidor (ADR-10).
- REC-04: Crear HU explícitas para "editar usuario" y "asignar/modificar roles" (M5).
- REC-05: Formalizar el catálogo de errores `AUTH_00x` como parte de RT-06.
- REC-06: Unificar el mínimo de contraseña UI/backend (INC-05).
- REC-07: Definir respuesta genérica en recuperación para evitar enumeración (INC-06).

---

## 13. Próximo paso del flujo

`Analizar (este documento) → Proponer inventario (01) y matriz (02) → Revisar con
el equipo → Aprobar → Re-elaborar HU M1 según plantilla + generar HU nuevas
(Fase 6)`. No se generan HU completas en esta fase.

---

## 14. Revisión estructural de las HU existentes (HU-01 a HU-09)

Origen del insumo: `docs/proyecto/COMPIRA_Historias_Usuario.pdf`.

### 14.1 Distinción clave: desarrollo vs documentación

Se distinguen dos estados independientes para cada HU existente:

- **Estado de desarrollo** (software): `Completada`. Lo confirma el documento
  oficial (rutas, pantallas, contratos y casos observables en el repositorio).
  **No se modifica**: sería incorrecto degradar el estado de una funcionalidad ya
  construida.
- **Conformidad documental** (ajuste a `hu-template.md`): **No conforme**. La HU,
  tal como está redactada en el PDF, no sigue la plantilla oficial de COMPIRA.

La instrucción del equipo es revisar la estructura y, si está mal formulada,
**volver a crear la HU** y dejarla en el inventario como pendiente de
re-elaboración. Por eso, en `01-inventario-hu.md`, HU-01..HU-09 conservan su
`Estado de desarrollo = Completada` pero se marcan con
`Conformidad documental = No conforme` y una tarea de re-elaboración pendiente.

### 14.2 Hallazgos de no conformidad con `hu-template.md`

Aplican, en distinto grado, a las nueve HU:

| Hallazgo | Descripción | Exigido por la plantilla |
|---|---|---|
| H-01 Ficha de identificación incompleta | Las HU usan encabezado "Sprint / Prioridad / SP / Estado" y "Como/Quiero/Para", pero **no** incluyen la tabla de identificación oficial (Identificador, Nombre, Módulo, Actor, Estado, Prioridad, Versión, Fuente principal, Última actualización, Dependencias). | Plantilla, bloque "HU-XXX" + tabla de campos. |
| H-02 Falta "Resumen ágil" separado del "Contexto funcional" | Mezclan la fórmula ágil en tabla con el contexto; la plantilla los separa como apartados. | Plantilla, "Resumen ágil" y "Contexto funcional". |
| H-03 Sin sección "Validaciones funcionales (VF-01…)" | Las validaciones están embebidas en los CA y en "Campos"; no existe la sección VF con nomenclatura. | Plantilla, "Validaciones funcionales". |
| H-04 Numeración no conforme | Usan "CA 1", "RN 1" en vez de `CA-01`, `RN-01`. Faltan `RC-`, `CP-`, `FA-`, `RNF-`, `VF-`. | `hu-rules.md` §47 (nomenclatura). |
| H-05 Reglas de comportamiento no explícitas | Comportamientos (limpiar campos, reenfocar, refrescar) aparecen dentro de CA; no hay sección `RC-`. | Plantilla, "Reglas de comportamiento". |
| H-06 Escenarios de prueba (Gherkin) ausentes | No hay `CP-01` en formato Dado/Cuando/Entonces; los CA están en prosa. | Plantilla, "Escenarios de prueba". |
| H-07 Escenarios alternativos/error sin nomenclatura `FA-` | Existe tabla "Escenarios de error" pero sin IDs `FA-01` ni columnas de la plantilla (código HTTP, reintento, impacto). | Plantilla, "Escenarios alternativos / error". |
| H-08 RNF sin nomenclatura `RNF-` | Seguridad/trazabilidad se citan en prosa; sin IDs `RNF-01`. | Plantilla, "Requisitos no funcionales". |
| H-09 Sin sección "Definition of Ready" | Incluyen DoD (Definition of Done) pero no el DoR exigido por la plantilla. | Plantilla, "Definition of Ready". |
| H-10 "Fuente principal" y "Origen" no registrados | No se etiqueta el origen (DOC/HU/DEC/RT/REC) de reglas ni la fuente principal de la HU. | `hu-rules.md` §2 bis (trazabilidad de origen). |
| H-11 Trazabilidad interna incompleta | Sin matriz Necesidad → Regla → CA → CP dentro de la HU cuando la complejidad lo amerita. | `hu-rules.md` §27/§48. |

### 14.3 Fortalezas a conservar en la re-elaboración

Las HU del PDF tienen contenido valioso que **debe preservarse** al reescribir:

- CA verificables y observables (condición/acción/resultado), con mensajes y
  códigos `AUTH_00x`.
- Campos con tipo, control, obligatoriedad y descripción.
- Contratos funcionales de API (endpoint, request, response) por HU.
- Escenarios de error con mensajes reales.
- Preguntas pendientes por HU (se consolidan en `PENDIENTES.md`).
- Separación explícita de recomendaciones ("no es criterio hasta aprobación").

### 14.4 Conclusión de la revisión

Las nueve HU están **funcionalmente sólidas pero documentalmente no conformes**
con la plantilla oficial. Acción acordada: **re-elaborarlas** (Fase 6) para
cumplir `hu-template.md`, conservando su contenido funcional y su estado de
desarrollo `Completada`. Hasta re-elaborarlas, en el inventario figuran con
conformidad documental `No conforme` y una HU-tarea de re-elaboración pendiente.
No se reescriben en esta fase (solo análisis e inventario).
