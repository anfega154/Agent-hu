# Contexto funcional — COMPIRA

> Resumen funcional y técnico derivado del documento oficial
> `docs/proyecto/COMPIRA_TDG_Desarrollo_SW_InformeTecnico_V1.docx`.
> Este archivo es la referencia de contexto para trabajar HUs y features de COMPIRA.
> Ante cualquier duda o contradicción, la fuente de verdad es el documento oficial (ver sección 17).

---

## Cómo leer este documento (separación funcional / técnica)

El contenido está clasificado en tres bloques conceptuales. **No mezcles los
niveles**: una decisión o restricción técnica NO se convierte en requisito
funcional dentro de una HU.

**Bloque A — Contexto funcional (qué necesita hacer COMPIRA).**
Secciones 1–10 y 14–18. Es la base para redactar HU. Ejemplo funcional: "el
usuario debe autenticarse".

**Bloque B — Restricciones técnicas aprobadas (decisiones ya establecidas).**
Secciones 12 y 13 (stack implementado en TDG II y ADR registrados). Son
decisiones oficiales del equipo; trátalas como restricciones (`Origen: DEC/RT`),
no como requisitos funcionales inventados. Ejemplo: "AWS Cognito + OTP" es una
restricción técnica, no la necesidad funcional.

**Bloque C — Decisiones técnicas propuestas (aún NO aprobadas).**
Recomendaciones o pendientes que no deben tratarse como definitivos:
- Sección 11 (stack propuesto en TDG I) en lo que haya sido superado por la
  sección 12.
- ADR-06 (proveedor de nube general) sigue **abierto**.
- Deuda técnica de ADR-10 (validación de token en el servidor) está **pendiente**.

Regla: si un elemento del Bloque C aún no fue aprobado, refiérete a él como
`RECOMENDACIÓN — requiere aprobación` o `Pendiente por definir`, nunca como
requisito. Relaciona funcional y técnico, pero mantenlos conceptualmente separados.

---

## 1. Nombre del proyecto

**COMPIRA** — Plataforma web para centralizar flujos de trabajo en pymes.

- Etimología del nombre: contracción de *compito* (tarea, en italiano) + *azienda* (empresa, recortada). Busca sonar natural en español (cercano a "compañía"), amigable y accesible para pymes.
- Proyecto de Trabajo de Grado (TDG I y TDG II) — Ingeniería en Software, Tecnológico de Antioquia (Medellín, Colombia), 2026.
- Equipo de desarrollo: Norbey Alonso Mejía Cortés y Andrés Felipe Gañán Moreno.
- Directora (Product Owner): Mg. María Nelcy González Ramírez. Codirector (Product Owner): Mg. Juan David Tamayo Quintero.

---

## 2. Objetivo general

Desarrollar una plataforma web que **centralice la información y facilite la gestión de tareas diarias en las pymes**.

### Objetivos específicos

1. **Analizar** los sistemas de gestión actuales, identificando limitaciones y dificultades derivadas del uso de múltiples canales dispersos.
2. **Identificar** requisitos y módulos para la plataforma a partir del análisis del contexto, plasmándolos en el backlog y las Historias de Usuario (HU).
3. **Desarrollar** los módulos centrales siguiendo un flujo de trabajo ágil (Scrum + Kanban) que facilite la integración constante y progresiva de funcionalidades.
4. **Validar** la plataforma mediante pruebas funcionales y de usabilidad que verifiquen su estructura, funcionamiento y el cumplimiento de los objetivos.

---

## 3. Problema que busca resolver

Muchas pymes gestionan sus tareas y actividades mediante múltiples herramientas y canales desconectados entre sí:

- WhatsApp;
- correo electrónico;
- hojas de cálculo;
- herramientas de gestión de proyectos (Jira, Trello, Asana, Microsoft Project);
- aplicaciones de notas;
- documentos compartidos (Google Drive, Notion, Confluence, SharePoint);
- repositorios de código, herramientas de comunicación interna (Slack, Teams) y soporte al cliente (Zendesk, Freshdesk, Intercom).

Esta dispersión genera problemas como:

- pérdida de trazabilidad;
- dificultad para conocer responsables;
- dificultad para conocer el estado real de las tareas;
- retrasos en entregas y compromisos;
- duplicidad de información;
- pérdida de tiempo buscando información en varias plataformas;
- dificultad para realizar seguimiento;
- fragmentación del ciclo de vida de las tareas a medida que intervienen más actores;
- aumento de la carga operativa.

Las herramientas existentes en el mercado (Trello, Asana, Jira) están orientadas a proyectos complejos o empresas de mayor tamaño, requieren curva de aprendizaje, capacitación y costos (licencias y costos ocultos) que las pymes con recursos limitados no siempre pueden asumir. Aun las gratuitas atacan puntos aislados del ciclo de vida de la tarea, por lo que no eliminan la fragmentación sino que la desplazan.

**Contexto de mercado:** solo en Colombia, en 2023 se constituyeron **141.687 pymes** (principalmente en comercio, hotelería e industria), que operan con presupuestos tecnológicos limitados y sin personal dedicado de TI.

COMPIRA busca centralizar este proceso en una única plataforma accesible, intuitiva y de bajo costo de adopción.

---

## 4. Proceso funcional principal

El proceso principal del sistema es la **gestión centralizada del ciclo de vida de una tarea** y su trazabilidad completa (proceso TO-BE frente al flujo actual disperso AS-IS).

El flujo general comprende:

1. autenticación del usuario (según su rol);
2. creación de la tarea;
3. definición de información de la tarea (título, descripción, fechas);
4. asignación de responsables (uno o varios colaboradores);
5. notificación automática de la asignación;
6. ejecución de la actividad;
7. actualización de estado;
8. seguimiento en tiempo real desde el panel;
9. reasignación cuando aplique;
10. aprobación o cierre;
11. registro del histórico de cambios;
12. consulta de indicadores de productividad.

Principio clave: **ningún módulo funciona de forma aislada**; la acción de uno desencadena lo que ocurre en el siguiente, sin intervención manual (asignación → notificación → seguimiento → cierre → histórico → indicadores).

---

## 5. Actores

Los tres roles son la base de la gobernanza por permisos. Cada HU debe asociarse a uno de estos roles.

### 5.1 Administrador del sistema

Responsable de la configuración general de la organización. Único con permisos para crear, editar o eliminar cuentas y modificar configuraciones críticas.

Funciones (Gestión de roles, usuarios, informes y plataforma):

- gestionar usuarios (registrar, editar, remover);
- asignar y modificar roles;
- restablecer contraseñas;
- consultar reportes de productividad;
- consultar porcentaje de cumplimiento de tareas;
- consultar reporte de retraso de tareas;
- consultar carga de trabajo (tareas asignadas) por responsable;
- consultar tiempos de cierre de tareas;
- consultar ciclo e históricos de las tareas;
- configurar notificaciones;
- configurar zona horaria;
- monitorear el panel de equipos.

### 5.2 Coordinador de equipo (líder de proyecto)

Rol operativo. Supervisa el seguimiento de tareas de su equipo.

Funciones (Gestión de tareas de equipo, alertas de desempeño, panel de seguimiento):

- crear tareas;
- asignar y reasignar tareas;
- cancelar tareas;
- establecer fechas límite;
- realizar seguimiento y monitorear avance;
- gestionar alertas de desempeño;
- consultar el panel de seguimiento;
- aprobar actividades cuando aplique;
- cerrar actividades cuando aplique;
- gestionar la trazabilidad de las tareas.

**No** tiene permisos para: gestionar usuarios ni modificar configuraciones generales de la plataforma.

### 5.3 Colaborador (operador)

Responsable de ejecutar las tareas asignadas.

Funciones (Interacción y documentación de tareas):

- consultar tareas asignadas;
- recibir notificaciones;
- recibir recordatorios;
- actualizar el estado de las tareas (pendiente, en progreso, completada);
- registrar observaciones;
- documentar la ejecución de la tarea.

**No** puede: crear tareas ni modificar configuraciones generales.

---

## 6. Módulos funcionales

### M1. Autenticación y gestión de roles

- registro seguro de usuarios;
- inicio de sesión;
- asignación de permisos según rol;
- recuperación de contraseña;
- control de sesión;
- control de acceso basado en roles (RBAC).

> El módulo M1 determina qué módulos y qué opciones ve cada usuario al entrar. En la implementación actual, la autenticación se delega en **AWS Cognito** con verificación en dos pasos (OTP por correo) — ver secciones 12 y 14.

### M2. Gestión de tareas

- creación de tareas (título, descripción, fechas);
- asignación a uno o varios colaboradores;
- estados de tarea y actualización de estado;
- reasignación;
- cancelación;
- cierre;
- historial de cambios;
- trazabilidad.

### M3. Recordatorios automáticos

Notificaciones dentro de la plataforma (en tiempo real) cuando:

- una tarea es asignada;
- una tarea es reasignada;
- una tarea está próxima a vencer;
- una tarea está retrasada (alerta al coordinador).

### M4. Panel de seguimiento

Vista consolidada del equipo:

- estado de tareas;
- avance;
- tareas vencidas;
- tareas próximas a vencer;
- niveles de cumplimiento.

Filtros: por usuario/colaborador, estado y fecha. Debe actualizarse en tiempo real sin recarga completa de página.

### M5. Administración del sistema

Disponible solo para el Administrador:

- gestión de usuarios;
- gestión de roles;
- configuraciones generales (zona horaria, notificaciones);
- reportes de productividad del equipo.

---

## 7. Alcance explícitamente excluido

Fuera del alcance del proyecto:

- aplicación móvil nativa (solo acceso desde navegador con diseño responsivo);
- implementación productiva en empresas reales con datos en producción;
- diseño UI/UX avanzado más allá de prototipos funcionales;
- estrategias comerciales;
- lanzamiento comercial del producto.

Si durante el análisis aparece una funcionalidad que pudiera pertenecer a estos elementos, **no debe agregarse automáticamente al backlog**. Debe registrarse como:

> "Posible ampliación de alcance — requiere validación."

---

## 8. Principios del sistema

COMPIRA debe favorecer:

- **simplicidad de adopción** (operable desde la primera sesión, sin capacitación especializada ni onboarding complejo);
- **centralización** de la información en un único entorno;
- **trazabilidad** completa del ciclo de cada tarea;
- **gobernanza por roles** (cada actor ve y ejecuta únicamente lo que corresponde a su función);
- **seguimiento** del trabajo en tiempo real;
- reducción de la dispersión de información;
- bajo costo de adopción e implementación.

Diferenciadores frente a Trello / Asana / Jira: diseñado para pymes sin capacitación previa, bajo costo de adopción, roles diferenciados para contexto pyme e interfaz contextualizada.

---

## 9. Metodología

- Metodología **híbrida Scrum + Kanban**.
- **Scrum**: sprints de dos semanas, cada uno con un incremento funcional. Ceremonias: Sprint Planning, Daily Scrum, Sprint Review, Sprint Retrospective. Artefactos: Product Backlog, Sprint Backlog, Incremento.
- **Kanban**: tablero dentro de cada sprint con columnas **Por hacer / En progreso / En revisión / Terminado** y límite de trabajo en progreso (WIP).
- Roles del equipo:
  - Product Owner: Mg. María Nelcy González Ramírez y Mg. Juan David Tamayo Quintero.
  - Scrum Master: Norbey Alonso Mejía Cortés.
  - Developers: Norbey Alonso Mejía Cortés y Andrés Felipe Gañán Moreno.
- Total: **12 sprints** de 2 semanas, distribuidos en dos semestres (2026-1 / TG1 y 2026-2 / TG2), con un período complementario intermedio (junio–julio) para ajustes y configuración del entorno.

### Fases del proyecto

- **Fase 1 — Análisis y requisitos** (OE1): levantamiento de información, limitaciones actuales, requisitos funcionales/no funcionales, BPMN AS-IS.
- **Fase 2 — Diseño y modelado** (OE2): arquitectura, diagrama de clases y entidad-relación, Product Backlog con HU priorizadas, prototipos de interfaz, BPMN TO-BE.
- **Fase 3 — Desarrollo iterativo** (OE3): implementación de módulos en sprints de 2 semanas con incremento validado por el Product Owner.
- **Fase 4 — Pruebas y despliegue** (OE4): pruebas funcionales, de integración y de usabilidad; despliegue en cloud; manuales y resumen técnico.

---

## 10. Estándares de calidad aplicados

- **BPMN 2.0** (OMG / ISO/IEC 19510): modelado de procesos AS-IS y TO-BE.
- **ISO/IEC 25010:2023** (modelo de calidad del producto). Cuatro características priorizadas para COMPIRA:
  - **Usabilidad**: interfaces accesibles para pymes sin curva de aprendizaje.
  - **Seguridad**: control de acceso por roles mediante JWT.
  - **Fiabilidad**: integridad ACID de registros históricos (PostgreSQL).
  - **Mantenibilidad**: arquitectura desacoplada (Spring Boot + React.js).
- **ISO/IEC 12207:2017** (procesos del ciclo de vida del software).
- **ISO/IEC/IEEE 42010:2022** (descripción de arquitectura, base para los ADR).
- **Modelo C4** (Context, Containers, Components) para documentar la arquitectura; el nivel de código se omite.

---

## 11. Stack tecnológico propuesto (TDG I)

Arquitectura **cliente-servidor de tres capas**, comunicación exclusiva vía **API REST** con autenticación **JWT**.

- **Frontend**: React.js 18.x con TypeScript (renderizado concurrente, componentes reactivos y reutilizables, actualización en tiempo real del panel sin recarga).
- **Backend**: Java con Spring Boot 3.x (autoconfiguración, inyección de dependencias, Spring Security nativo). Java 17 LTS.
- **Base de datos**: PostgreSQL 16.x (RDBMS, cumplimiento ACID, integridad del historial).
- **Autenticación**: JWT sin estado (el token transporta el rol activo del usuario).
- **Despliegue**: nube (AWS, Azure o GCP), modelo PaaS.
- **Control de versiones**: GitHub (Git, pull requests, revisión de código).

---

## 12. Stack tecnológico implementado (TDG II)

Durante la implementación se tomaron decisiones que ajustan el stack propuesto. Este es el estado real y debe considerarse la referencia técnica vigente:

- **Backend reactivo no bloqueante**: Spring **WebFlux** + Project Reactor sobre **Java 21** (en lugar del Spring MVC bloqueante previsto). Toda la cadena debe ser no bloqueante; se verifica con BlockHound.
- **Persistencia reactiva**: **R2DBC** sobre PostgreSQL (`spring-boot-starter-data-r2dbc`, driver `r2dbc-postgresql`, pool `r2dbc-pool`), acceso con SQL explícito vía `DatabaseClient` y `TransactionalOperator`. Migraciones de esquema versionadas con **Liquibase** (DataSource JDBC dedicado solo para migraciones). Changelog maestro crea tablas `users`, `roles`, `user_roles` y siembra roles base.
- **Arquitectura del backend**: **Clean Architecture** con el andamiaje (scaffold) de **Bancolombia**, módulos Gradle: `domain/model`, `domain/usecase`, `applications/app-service`, `infrastructure` (entry-points y driven-adapters). Reglas de dependencia verificadas con **ArchUnit**.
- **Identidad y autenticación**: delegada en **AWS Cognito** vía SDK asíncrono. Cognito emite los tokens (acceso, identidad, refresco); el backend no genera ni firma JWT propios. Flujo `USER_PASSWORD_AUTH`, con MFA/OTP por correo (`EMAIL_OTP`) y cambio obligatorio de contraseña (`NEW_PASSWORD_REQUIRED`). Esto fija **AWS** como proveedor de nube para identidad.
  - Cognito usa **User Pools** (directorio de usuarios) e **Identity Pools** (credenciales temporales); soporta OAuth 2.0, OpenID Connect y SAML 2.0.
- **Frontend sin librería de estado ni de tablas dedicada**: React Context (`AuthProvider`, hook `useAuth`) para el estado de sesión, estado local por componente, y `fetch` nativo encapsulado en una capa de servicios por característica (`authApi`, `companyApi`). Sin Redux/Zustand/TanStack Query/Axios por ahora.
- **Calidad y CI**:
  - Backend: **SonarQube** + **JaCoCo**; el pipeline bloquea si la cobertura de línea baja de **80 %** o la de rama de **60 %**; construye imagen **Docker** de producción.
  - Frontend: **Vitest** con cobertura v8 (umbrales 80 % en líneas/funciones/sentencias, 60 % en ramas) y **oxlint** como analizador estático.
  - Existen exclusiones de cobertura (p. ej. módulo `companies` y configuración) que deben revisarse antes del cierre.

> Nota: el proveedor de nube general (ADR-06) quedó como decisión abierta al cierre de TG1 (criterios: contenedor Java, PostgreSQL gestionado y capa gratuita; a resolver antes del sprint 11). La línea de identidad ya quedó fijada en AWS por el uso de Cognito (ADR-10).

---

## 13. Decisiones técnicas (ADR)

Doce ADR registrados. Resumen operativo:

- **ADR-01** — React para el frontend (sobre Angular, Vue 3, Svelte). Requerido por M3 (tiempo real) y M4 (tablas/filtros).
- **ADR-02** — Java + Spring Boot para el backend (sobre Node/Express, Django, Laravel). Spring Security cubre JWT/RBAC.
- **ADR-03** — PostgreSQL (sobre MySQL, MongoDB, SQLite). Integridad ACID para el historial de M2.
- **ADR-04** — Cliente-servidor en tres capas con API REST (sobre monolito y microservicios).
- **ADR-05** — Autenticación JWT sin estado (sobre sesión con estado, OAuth2 externo).
- **ADR-06** — Despliegue en la nube PaaS; **proveedor pendiente** (criterios de cierre definidos).
- **ADR-07** — Backend reactivo no bloqueante con Spring WebFlux + Project Reactor (Java 21).
- **ADR-08** — Persistencia reactiva R2DBC sobre PostgreSQL + migraciones con Liquibase.
- **ADR-09** — Clean Architecture por capas/módulos con el andamiaje de Bancolombia (verificada con ArchUnit).
- **ADR-10** — Identidad delegada en AWS Cognito (tokens emitidos por el proveedor; fija AWS).
- **ADR-11** — Frontend sin librería de estado/tablas (React Context, hooks, fetch nativo).
- **ADR-12** — Puertas de calidad con cobertura mínima obligatoria en CI.

> Deuda técnica relevante (ADR-10): el backend aún **no valida en el servidor el token Bearer** ni aplica autorización por rol sobre los endpoints (la autorización efectiva recae en Cognito). Pendiente un filtro de validación de token y verificación de rol en la capa `reactive-web`.

---

## 14. Backlog — Historias de usuario (estado a TG1)

Las HU completadas hasta el corte corresponden al módulo M1 (autenticación). Cada HU debe tener criterios de aceptación con escenarios Cuando/Espero y una Definición de Terminado (DoD).

| ID | Historia | Sprint | Prioridad | SP | Estado | Criterio (resumen) |
|----|----------|--------|-----------|----|--------|--------------------|
| HU-01 | Cambio obligatorio de contraseña en el primer inicio de sesión | S1 | Alta | 5 | Completado | Requiere cambio de contraseña y activa la cuenta con la nueva credencial. |
| HU-02 | Inicio de sesión con correo y contraseña | S1 | Alta | 5 | Completado | Autentica credenciales válidas y determina el siguiente paso. |
| HU-03 | Verificación en dos pasos mediante código OTP por correo | S1 | Alta | 5 | Completado | Segundo factor con desafío `EMAIL_OTP` (código de 6 dígitos). |
| HU-04 | Reenvío del código OTP de acceso | S2 | Media | 2 | Completado | Permite reenviar el OTP tras cumplirse el tiempo de espera (120 s). |
| HU-05 | Solicitud de recuperación de contraseña por correo | S2 | Alta | 3 | Completado | Envía el código de recuperación al correo registrado. |
| HU-06 | Confirmación del restablecimiento de contraseña con código | S2 | Alta | 5 | Completado | Confirma el código y restablece la contraseña. |
| HU-07 | Cierre de sesión y control de acceso a rutas protegidas | S2 | Alta | 3 | Completado | Protege rutas internas y limpia la sesión al salir. |
| HU-08 | Registro de usuarios por parte del administrador | S3 | Alta | 5 | Completado | Crea usuarios con rol y contraseña temporal. |
| HU-09 | Eliminación de usuarios por correo | S3 | Alta | 3 | Completado | Elimina usuarios con confirmación previa. |

### Detalle de comportamiento del flujo de autenticación (M1)

Convenciones observadas en los criterios de aceptación de las HU de autenticación (útiles para nuevas HU del mismo módulo):

- **Rutas**: `/` (pantalla principal), `/auth/new-password` (cambio obligatorio), `/auth/verify` (verificación OTP), pantalla de inicio de sesión.
- **Respuestas de Cognito**: `AUTHENTICATED` (acceso concedido, se guardan tokens y datos), `CHALLENGE_REQUIRED` con `NEW_PASSWORD_REQUIRED`, `EMAIL_OTP` o `SMS_MFA`.
- **Política de contraseña**: longitud entre 10 y 128 caracteres, más la política de Cognito; la confirmación debe coincidir ("Las contraseñas no coinciden").
- **OTP**: código de 6 dígitos; botón deshabilitado hasta completar los 6 dígitos; contador de reenvío de 120 s en formato `mm:ss`; al reenviar se limpian los campos y se devuelve el foco al primero.
- **Códigos de error** (ejemplos usados): `AUTH_002` (contraseña no cumple política de Cognito), `AUTH_003` (código de confirmación inválido), `AUTH_004` (código expirado), `AUTH_005` (credenciales no válidas), `AUTH_006` / `AUTH_008` (cuenta no confirmada / requiere restablecer contraseña).
- **DoD estándar por HU**: (1) código revisado y aprobado (code review); (2) prueba funcional ejecutada y aprobada (caso documentado); (3) evidencia visual integrada en el diario de sprint; (4) criterios de aceptación verificados y marcados.

---

## 15. Plan de sprints (resumen)

**TG1 (2026-1)**
- S1–S2: análisis, requisitos funcionales/no funcionales, BPMN AS-IS, actores y roles.
- S3–S4: arquitectura, diagrama de clases y ER, Product Backlog, stack, wireframes, BPMN TO-BE.
- S5: consolidación del documento TG1 y sustentación parcial.
- Período complementario (jun–jul): incorporar retroalimentación, configurar entorno (GitHub, PostgreSQL, Spring Boot + React) y prueba de concepto del módulo de autenticación.

**TG2 (2026-2)**
- S6: módulo de autenticación (M1) + pruebas unitarias.
- S7: módulo de gestión de tareas (M2).
- S8: módulo de recordatorios (M3) + panel de seguimiento (M4).
- S9: módulo de administración (M5), integración y pruebas de integración.
- S10: pruebas funcionales por módulo y correcciones.
- S11: pruebas de usabilidad en pyme experimental y despliegue en cloud.
- S12: manuales de usuario e instalación, resumen técnico, documento final y sustentación.

---

## 16. Fundamento académico (antecedentes)

El diseño se apoya en literatura reciente (citada en el documento oficial):

- **Barrera-Cámara et al. (2018)** — BPM y notación BPMN para modelar el flujo AS-IS/TO-BE.
- **Salgado-García et al. (2024)** — transformación digital y competitividad; el reto no es el acceso técnico sino alinear la solución con la cultura organizacional (evitar resistencia con interfaces intuitivas).
- **Lozano-Montoya et al. (2024)** — viabilidad de Scrum/Kanban en equipos pequeños.
- **Bonilla-Acosta y Caballero-Espinel (2025)** — viabilidad técnica/económica de arquitectura REST de tres capas para pymes colombianas; contexto de 141.687 pymes creadas en 2023.
- **Guerrero-Calvache y Hernández (2024)** — factores de productividad: comunicación, asignación de responsabilidades y seguimiento del progreso concentran el **91,8 %** de representatividad; base de los indicadores del panel.
- **Fernández-Trujillo (2021)** — gobernanza organizacional (roles, reglas y toma de decisiones claras) para coordinación y confianza.

---

## 17. Fuente de verdad

Este documento es un resumen funcional y técnico derivado del documento oficial:

`docs/proyecto/COMPIRA_TDG_Desarrollo_SW_InformeTecnico_V1.docx`

Ante dudas, contradicciones o falta de información:

1. consultar el documento oficial;
2. no inventar una respuesta;
3. registrar la inconsistencia;
4. formular una pregunta pendiente.

---

## 18. Regla fundamental

No incorporar funcionalidad solamente porque parezca lógica para una plataforma de gestión de tareas.

Todo comportamiento debe provenir de:

1. la documentación del proyecto;
2. una decisión explícita del equipo (incluyendo los ADR);
3. una HU previamente aprobada.

En caso contrario debe tratarse como recomendación o pregunta pendiente.
