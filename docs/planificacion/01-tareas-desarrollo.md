# COMPIRA — Documento de Tareas de Desarrollo

| Campo | Valor |
|---|---|
| Proyecto | COMPIRA |
| Documento | Documento de Tareas de Desarrollo |
| Fecha | 2026-10-06 |
| Período | 15 de septiembre – 31 de octubre de 2026 |
| Total de Historias de Usuario | 22 (8 completadas en M1 + 14 nuevas en M2–M5) |
| Esfuerzo total estimado | 477 h |

> **Origen de datos:** estimación consolidada (hoja de cálculo) e inventario de HU (documento Word).
> Las tareas listadas son el desglose técnico derivado de las HU aprobadas; no modifican el alcance
> funcional. Toda tarea técnica es trazable a su HU correspondiente.

---

## 1. Restricciones técnicas (stack vigente — TDG II)

Todas las tareas de este documento deben respetar las decisiones técnicas aprobadas (`Origen: DEC/RT`):

- **Backend:** Spring Boot **WebFlux** + Project Reactor sobre **Java 21**. Toda la cadena debe ser
  no bloqueante (`Mono`/`Flux`); se verifica con BlockHound.
- **Persistencia:** **R2DBC** sobre **PostgreSQL (Aurora)**. Acceso con SQL explícito vía
  `DatabaseClient` y `TransactionalOperator`. **No** se usa JPA/Hibernate.
- **Migraciones de esquema:** **Liquibase** (DataSource JDBC dedicado solo para migraciones).
  Cada cambio de esquema requiere un changeset con su `rollback` correspondiente.
- **Arquitectura:** **Clean Architecture** con el andamiaje de **Bancolombia**
  (`domain/model`, `domain/usecase`, `applications/app-service`, `infrastructure`).
  Reglas de dependencia verificadas con ArchUnit.
- **Identidad / Auth:** **AWS Cognito** (tokens emitidos por el proveedor). Deuda técnica ADR-10:
  el backend aún no valida el token Bearer ni aplica autorización por rol en el servidor
  (abordado en el Sprint 1 como middleware fundacional).
- **Frontend:** **React.js + TypeScript**, estado con **React Context**, `fetch` nativo encapsulado
  en una capa de servicios por característica. Sin Redux/Zustand/TanStack Query/Axios.
- **Calidad:** Backend SonarQube + JaCoCo (línea ≥ 80 %, rama ≥ 60 %), imagen Docker de producción.
  Frontend Vitest (cobertura v8, 80 % líneas/funciones/sentencias, 60 % ramas) + oxlint.

---

## 2. Resumen de distribución del esfuerzo

| Bloque | Horas |
|---|---|
| Construcción | 276 h |
| — M1 Autenticación | 51 h |
| — M2 Gestión de tareas | 60 h |
| — M3 Recordatorios + M4 Panel | 76 h |
| — M5 Administración | 35 h |
| — Integración | 54 h |
| Pruebas | 50 h |
| Documentación | 25 h |
| Gestión | 11 h |
| Entrega / despliegue | 18 h |
| **Total** | **477 h** |

---

## 3. Plan de sprints (semanal)

| Sprint | Fechas | Alcance | HU |
|---|---|---|---|
| Sprint 1 | 15–21 sep | Fundación + Admin de usuarios (M5) | HU-40, HU-10 + Middleware validación de token |
| Sprint 2 | 22–28 sep | Núcleo de tareas (M2) | HU-12, HU-13, HU-14 |
| Sprint 3 | 29 sep – 5 oct | Ciclo de vida de tareas (M2) | HU-15, HU-16, HU-17, HU-18, HU-19 |
| **Sprint 4** | **6–12 oct** | **Histórico + Notificaciones + Equipos (ACTUAL ~75 %)** | HU-20, HU-21, HU-24, HU-31, HU-41 |
| Sprint 5 | 13–19 oct | Equipos + Panel (M5/M4) + inicio Certificación | HU-32, HU-33, HU-25, HU-28 |
| Sprint 6 | 20–26 oct | Indicadores + Reportes + Integración + Certificación | HU-27, HU-29, HU-38 |
| Sprint 7 | 27–31 oct | Integración final, pruebas, documentación, despliegue | — |

### 3.1 Estado de avance al 2026-10-06 (Sprint 4 — ~75 %)

| HU | Sprint | Estado |
|---|---|---|
| HU-01 … HU-08 | — (M1) | Completada |
| HU-40 | 1 | Completada |
| HU-10 | 1 | Completada |
| HU-12 | 2 | Completada |
| HU-13 | 2 | Completada |
| HU-14 | 2 | Completada |
| HU-15 | 3 | Completada |
| HU-16 | 3 | Completada |
| HU-17 | 3 | Completada |
| HU-18 | 3 | Completada |
| HU-19 | 3 | Completada |
| HU-20 | 4 | Desarrollo completado |
| HU-21 | 4 | En desarrollo |
| HU-24 | 4 | No iniciada |
| HU-31 | 4 | No iniciada |
| HU-41 | 4 | No iniciada |
| HU-32, HU-33, HU-25, HU-28 | 5 | No iniciada |
| HU-27, HU-29, HU-38 | 6 | No iniciada |

> Progreso Sprint 4: de las 14 HU nuevas, **11 con desarrollo completado** (HU-40, HU-10, HU-12 a
> HU-20) ≈ 75 %. HU-21 en curso; HU-24, HU-31 y HU-41 pendientes de iniciar.

---

# Módulo M1 — Autenticación y gestión de roles (COMPLETADO)

Las HU del módulo M1 fueron implementadas y validadas en TDG II (51 h de construcción).
Se incluyen como referencia; **no requieren tareas adicionales de desarrollo**.

| HU | Nombre | Estado | Nota técnica |
|---|---|---|---|
| HU-01 | Cambio obligatorio de contraseña en el primer inicio de sesión | Completada | Reto Cognito `NEW_PASSWORD_REQUIRED`; ruta `/auth/new-password`. |
| HU-02 | Inicio de sesión con correo y contraseña | Completada | Flujo `USER_PASSWORD_AUTH`; determina siguiente paso. |
| HU-03 | Verificación en dos pasos mediante código OTP por correo | Completada | Reto `EMAIL_OTP` (6 dígitos); ruta `/auth/verify`. |
| HU-04 | Reenvío del código OTP de acceso | Completada | Reenvío tras 120 s; contador `mm:ss`. |
| HU-05 | Solicitud de recuperación de contraseña por correo | Completada | Envía código de recuperación al correo registrado. |
| HU-06 | Confirmación del restablecimiento de contraseña con código | Completada | Confirma código y restablece credencial. |
| HU-07 | Cierre de sesión y control de acceso a rutas protegidas | Completada | Protege rutas internas; limpia sesión y tokens. |
| HU-08 | Registro de usuarios por parte del administrador | Completada | Crea usuario con rol y contraseña temporal. |

**DoD aplicado por HU de M1:** code review aprobado, prueba funcional ejecutada y documentada,
evidencia visual en el diario de sprint, criterios de aceptación verificados.

> **Deuda técnica pendiente (ADR-10):** validación del token Bearer y autorización por rol en el
> servidor. Se aborda como middleware fundacional en el **Sprint 1** (ver sección fundacional).

---

# Tarea fundacional — Middleware de validación de token y autorización por rol

**Sprint:** Sprint 1 (15–21 sep)
**Módulo:** Transversal (RT — seguridad)
**Esfuerzo estimado:** incluido en construcción M5 / integración
**Estado:** Completada
**Dependencias:** AWS Cognito (ADR-10), módulo M1

### Tareas de Backend
- [x] Implementar `WebFilter` reactivo en la capa `reactive-web` que extraiga el token `Bearer` del header `Authorization`.
- [x] Validar la firma y vigencia del token contra el JWKS del User Pool de Cognito (descarga y cacheo de llaves públicas).
- [x] Extraer `sub`, `email` y `cognito:groups` (rol activo) y poblar el `SecurityContext` reactivo.
- [x] Implementar autorización por rol (`Administrador`, `Coordinador`, `Colaborador`) mediante anotaciones/configuración de rutas.
- [x] Manejo de errores: `401` token ausente/inválido/expirado, `403` rol insuficiente, con cuerpo de error estándar.
- [x] Pruebas de integración del filtro con tokens válidos, expirados y de rol insuficiente; verificación con BlockHound.

---

# Módulo M5 — Administración (parte Sprint 1)

---

## HU-40 — Listar usuarios

**Sprint:** Sprint 1 (15–21 sep)
**Módulo:** M5
**Esfuerzo estimado:** 5h
**Estado:** Completada
**Dependencias:** Middleware de validación de token, HU-08

### Tareas de Base de Datos / Liquibase
- [x] Verificar índices de consulta sobre `users(email)` y `users(role)` para filtrado/paginación.
- [x] Liquibase changelog: changeset `add-index-users-email-role` con índices `idx_users_email`, `idx_users_role`.
- [x] Rollback: `dropIndex` de ambos índices.

### Tareas de Backend
- [x] Endpoint `GET /api/v1/users` con paginación (`page`, `size`) y filtros opcionales (`role`, `search`).
- [x] UseCase `ListUsersUseCase` que retorna `Flux<User>` consultando vía `DatabaseClient` con SQL explícito.
- [x] Validación de parámetros de paginación (rango de `size`, `page >= 0`).
- [x] Manejo de errores: `400` parámetros inválidos; respuesta vacía controlada cuando no hay resultados.
- [x] Pruebas de integración del endpoint con datos sembrados; verificación no bloqueante.

### Tareas de Frontend
- [x] Página `UsersListPage` con tabla de usuarios (columnas: correo, rol, estado).
- [x] Controles de filtro por rol y búsqueda por correo; paginación.
- [x] Integración con `userApi.listUsers()` (fetch nativo en capa de servicios).
- [x] Estados de carga, error y vacío ("No hay usuarios registrados").
- [x] Pruebas de componente con Vitest (render de tabla, filtro, estado vacío).

---

## HU-10 — Editar usuario

**Sprint:** Sprint 1 (15–21 sep)
**Módulo:** M5
**Esfuerzo estimado:** 8h
**Estado:** Completada
**Dependencias:** HU-40, AWS Cognito, Middleware de validación de token

### Tareas de Base de Datos / Liquibase
- [x] Confirmar columnas editables en `users` (`display_name`, `role`, `status`) y columna de auditoría `updated_at`.
- [x] Liquibase changelog: changeset `add-users-updated-at` con columna `updated_at TIMESTAMP`.
- [x] Rollback: `dropColumn updated_at`.

### Tareas de Backend
- [x] Endpoint `PUT /api/v1/users/{id}` para actualizar nombre y rol del usuario.
- [x] UseCase `UpdateUserUseCase` que persiste con `DatabaseClient` dentro de `TransactionalOperator`.
- [x] Sincronizar el rol en Cognito (actualizar `cognito:groups` del usuario vía SDK asíncrono).
- [x] Validaciones: existencia del usuario, rol permitido, inmutabilidad del correo.
- [x] Manejo de errores: `404` usuario inexistente, `400` rol inválido, `409` conflicto de concurrencia.
- [x] Pruebas de integración del flujo update + sincronización Cognito (mock SDK).

### Tareas de Frontend
- [x] Página/modal `EditUserForm` con campos: nombre (editable), correo (solo lectura), rol (select).
- [x] Validación de formulario: nombre requerido, rol dentro de valores permitidos.
- [x] Integración con `userApi.updateUser(id, payload)`.
- [x] Estados de carga, error y confirmación de éxito; refresco de la lista sin recarga completa.
- [x] Pruebas de componente con Vitest (validación de formulario, llamada al servicio).

---

# Módulo M2 — Gestión de tareas

---

## HU-12 — Crear tarea

**Sprint:** Sprint 2 (22–28 sep)
**Módulo:** M2
**Esfuerzo estimado:** 8h
**Estado:** Completada
**Dependencias:** Middleware de validación de token, HU-40

### Tareas de Base de Datos / Liquibase
- [x] Crear tabla `tasks` (`id UUID PK`, `title`, `description`, `status`, `start_date`, `due_date`, `created_by`, `created_at`, `updated_at`).
- [x] Liquibase changelog: changeset `create-table-tasks` con PK, defaults y `status` inicial `PENDIENTE`.
- [x] Índices `idx_tasks_status`, `idx_tasks_due_date`, `idx_tasks_created_by`.
- [x] Rollback: `dropTable tasks` y `dropIndex` asociados.

### Tareas de Backend
- [x] Endpoint `POST /api/v1/tasks` para crear tarea (título, descripción, fechas).
- [x] UseCase `CreateTaskUseCase` que inserta vía `DatabaseClient` dentro de `TransactionalOperator` y retorna `Mono<Task>`.
- [x] Validaciones: título obligatorio, `due_date >= start_date`, estado inicial `PENDIENTE`.
- [x] Manejo de errores: `400` datos inválidos; `403` si el actor no es Coordinador/Administrador.
- [x] Pruebas de integración de creación con fechas válidas e inválidas; verificación no bloqueante.

### Tareas de Frontend
- [x] Página `CreateTaskPage` con formulario (título, descripción, fecha inicio, fecha límite).
- [x] Validación de formulario: título requerido, coherencia de fechas.
- [x] Integración con `taskApi.createTask(payload)`.
- [x] Estados de carga, error y éxito; redirección/refresco del listado tras crear.
- [x] Pruebas de componente con Vitest (validación de fechas, envío del formulario).

---

## HU-13 — Asignar tarea

**Sprint:** Sprint 2 (22–28 sep)
**Módulo:** M2
**Esfuerzo estimado:** 6h
**Estado:** Completada
**Dependencias:** HU-12, HU-40

### Tareas de Base de Datos / Liquibase
- [x] Crear tabla de relación `task_assignments` (`task_id FK`, `user_id FK`, `assigned_at`, `assigned_by`) para soportar múltiples responsables.
- [x] Liquibase changelog: changeset `create-table-task-assignments` con PK compuesta (`task_id`, `user_id`) y FKs a `tasks` y `users`.
- [x] Índice `idx_assignments_user` sobre `user_id`.
- [x] Rollback: `dropTable task_assignments`.

### Tareas de Backend
- [x] Endpoint `POST /api/v1/tasks/{id}/assignments` para asignar uno o varios colaboradores.
- [x] UseCase `AssignTaskUseCase` que inserta asignaciones vía `DatabaseClient` dentro de `TransactionalOperator`.
- [x] Validaciones: existencia de la tarea, existencia de usuarios, rol Colaborador de los asignados, no duplicar asignación.
- [x] Manejo de errores: `404` tarea/usuario inexistente, `409` asignación duplicada.
- [x] Pruebas de integración de asignación simple y múltiple.

### Tareas de Frontend
- [x] Componente `AssignTaskModal` con selector múltiple de colaboradores.
- [x] Validación: al menos un colaborador seleccionado.
- [x] Integración con `taskApi.assignTask(taskId, userIds)`.
- [x] Estados de carga, error y éxito; actualización de la vista de la tarea sin recarga.
- [x] Pruebas de componente con Vitest (selección múltiple, llamada al servicio).

---

## HU-14 — Consultar tareas asignadas

**Sprint:** Sprint 2 (22–28 sep)
**Módulo:** M2
**Esfuerzo estimado:** 6h
**Estado:** Completada
**Dependencias:** HU-13

### Tareas de Base de Datos / Liquibase
- [x] Verificar índice `idx_assignments_user` para consulta eficiente por colaborador.
- [x] Liquibase changelog: sin cambios de esquema nuevos (reutiliza `task_assignments`); documentar dependencia.
- [x] Rollback: N/A.

### Tareas de Backend
- [x] Endpoint `GET /api/v1/tasks/assigned` que retorna las tareas del usuario autenticado (`Flux<Task>`).
- [x] UseCase `ListAssignedTasksUseCase` con JOIN explícito `tasks` ⨝ `task_assignments` vía `DatabaseClient`.
- [x] Filtros opcionales por estado y rango de fechas; paginación.
- [x] Manejo de errores: `400` filtros inválidos; respuesta vacía controlada.
- [x] Pruebas de integración con usuario con y sin tareas asignadas.

### Tareas de Frontend
- [x] Página `MyTasksPage` con listado de tareas asignadas al usuario.
- [x] Filtros por estado y fecha; indicadores visuales de estado.
- [x] Integración con `taskApi.listAssignedTasks(filters)`.
- [x] Estados de carga, error y vacío ("No tienes tareas asignadas").
- [x] Pruebas de componente con Vitest (render de lista, filtro por estado, estado vacío).

---

## HU-15 — Actualizar estado de tarea

**Sprint:** Sprint 3 (29 sep – 5 oct)
**Módulo:** M2
**Esfuerzo estimado:** 8h
**Estado:** Completada
**Dependencias:** HU-14

### Tareas de Base de Datos / Liquibase
- [x] Confirmar dominio de `status` en `tasks` (`PENDIENTE`, `EN_PROGRESO`, `COMPLETADA`, `CANCELADA`, `CERRADA`).
- [x] Liquibase changelog: changeset `add-check-tasks-status` con restricción CHECK sobre valores de estado.
- [x] Rollback: `dropCheckConstraint` del estado.

### Tareas de Backend
- [x] Endpoint `PATCH /api/v1/tasks/{id}/status` para transición de estado.
- [x] UseCase `UpdateTaskStatusUseCase` que aplica la máquina de estados y actualiza `updated_at` vía `TransactionalOperator`.
- [x] Validaciones: transición permitida (p. ej. no pasar de `CANCELADA` a `EN_PROGRESO`), actor asignado a la tarea.
- [x] Manejo de errores: `404` tarea inexistente, `409` transición no permitida, `403` actor no autorizado.
- [x] Pruebas de integración de transiciones válidas e inválidas.

### Tareas de Frontend
- [x] Control de cambio de estado en la vista de detalle de tarea (select o botones de acción).
- [x] Deshabilitar transiciones no permitidas según el estado actual.
- [x] Integración con `taskApi.updateStatus(taskId, status)`.
- [x] Estados de carga, error y éxito; refresco del estado mostrado sin recarga.
- [x] Pruebas de componente con Vitest (transiciones habilitadas/deshabilitadas).

---

## HU-16 — Registrar observaciones

**Sprint:** Sprint 3 (29 sep – 5 oct)
**Módulo:** M2
**Esfuerzo estimado:** 5h
**Estado:** Completada
**Dependencias:** HU-15

### Tareas de Base de Datos / Liquibase
- [x] Crear tabla `task_observations` (`id UUID PK`, `task_id FK`, `author_id`, `content`, `created_at`).
- [x] Liquibase changelog: changeset `create-table-task-observations` con FK a `tasks` e índice `idx_observations_task`.
- [x] Rollback: `dropTable task_observations`.

### Tareas de Backend
- [x] Endpoint `POST /api/v1/tasks/{id}/observations` para registrar observación.
- [x] UseCase `AddObservationUseCase` que inserta vía `DatabaseClient` y retorna `Mono<Observation>`.
- [x] Validaciones: contenido no vacío, tarea existente, actor asignado o Coordinador.
- [x] Manejo de errores: `400` contenido vacío, `404` tarea inexistente.
- [x] Pruebas de integración de registro de observación.

### Tareas de Frontend
- [x] Componente `ObservationsPanel` en el detalle de tarea con lista cronológica y formulario de nueva observación.
- [x] Validación: contenido requerido.
- [x] Integración con `taskApi.addObservation(taskId, content)` y `taskApi.listObservations(taskId)`.
- [x] Estados de carga, error y vacío ("Sin observaciones").
- [x] Pruebas de componente con Vitest (envío de observación, render de lista).

---

## HU-17 — Reasignar tarea

**Sprint:** Sprint 3 (29 sep – 5 oct)
**Módulo:** M2
**Esfuerzo estimado:** 6h
**Estado:** Completada
**Dependencias:** HU-13

### Tareas de Base de Datos / Liquibase
- [x] Reutilizar `task_assignments`; registrar fecha/autor de reasignación en columnas `assigned_at`/`assigned_by`.
- [x] Liquibase changelog: changeset `add-assignments-reassign-audit` si se requieren columnas adicionales de auditoría; documentar rollback.
- [x] Rollback: `dropColumn` de columnas de auditoría añadidas.

### Tareas de Backend
- [x] Endpoint `PUT /api/v1/tasks/{id}/assignments` para reemplazar los responsables de la tarea.
- [x] UseCase `ReassignTaskUseCase` que elimina asignaciones previas e inserta las nuevas en una sola transacción (`TransactionalOperator`).
- [x] Validaciones: tarea no cerrada/cancelada, usuarios válidos y con rol Colaborador.
- [x] Manejo de errores: `404` tarea/usuario inexistente, `409` estado no permite reasignación.
- [x] Pruebas de integración de reasignación.

### Tareas de Frontend
- [x] Reutilizar `AssignTaskModal` en modo reasignación mostrando los responsables actuales.
- [x] Validación: al menos un colaborador tras la reasignación.
- [x] Integración con `taskApi.reassignTask(taskId, userIds)`.
- [x] Estados de carga, error y éxito; refresco de responsables sin recarga.
- [x] Pruebas de componente con Vitest (precarga de responsables, reasignación).

---

## HU-18 — Cancelar tarea

**Sprint:** Sprint 3 (29 sep – 5 oct)
**Módulo:** M2
**Esfuerzo estimado:** 5h
**Estado:** Completada
**Dependencias:** HU-15

### Tareas de Base de Datos / Liquibase
- [x] Confirmar soporte del estado `CANCELADA` en la restricción CHECK de `tasks` (de HU-15).
- [x] Liquibase changelog: sin cambios nuevos de esquema; documentar dependencia del CHECK existente.
- [x] Rollback: N/A.

### Tareas de Backend
- [x] Endpoint `PATCH /api/v1/tasks/{id}/cancel` para cancelar la tarea.
- [x] UseCase `CancelTaskUseCase` que cambia el estado a `CANCELADA` y registra motivo en observaciones vía `TransactionalOperator`.
- [x] Validaciones: tarea no cerrada, actor Coordinador/Administrador.
- [x] Manejo de errores: `409` tarea ya cerrada, `403` actor no autorizado.
- [x] Pruebas de integración de cancelación.

### Tareas de Frontend
- [x] Acción "Cancelar tarea" con diálogo de confirmación y campo de motivo.
- [x] Validación: confirmación explícita del usuario.
- [x] Integración con `taskApi.cancelTask(taskId, reason)`.
- [x] Estados de carga, error y éxito; actualización del estado mostrado.
- [x] Pruebas de componente con Vitest (confirmación, llamada al servicio).

---

## HU-19 — Aprobar / cerrar tarea

**Sprint:** Sprint 3 (29 sep – 5 oct)
**Módulo:** M2
**Esfuerzo estimado:** 6h
**Estado:** Completada
**Dependencias:** HU-15

### Tareas de Base de Datos / Liquibase
- [x] Confirmar estados `COMPLETADA` y `CERRADA` en la restricción CHECK de `tasks`.
- [x] Liquibase changelog: changeset `add-tasks-closed-at` con columna `closed_at TIMESTAMP` y `closed_by`.
- [x] Rollback: `dropColumn closed_at`, `dropColumn closed_by`.

### Tareas de Backend
- [x] Endpoint `PATCH /api/v1/tasks/{id}/close` para aprobar/cerrar la tarea.
- [x] UseCase `CloseTaskUseCase` que valida que la tarea esté `COMPLETADA` antes de cerrar; registra `closed_at`/`closed_by`.
- [x] Validaciones: solo Coordinador/Administrador puede cerrar; estado origen válido.
- [x] Manejo de errores: `409` estado no permite cierre, `403` actor no autorizado.
- [x] Pruebas de integración de cierre válido e inválido.

### Tareas de Frontend
- [x] Acción "Aprobar y cerrar" visible para Coordinador/Administrador en el detalle de tarea.
- [x] Validación: solo habilitada cuando la tarea está `COMPLETADA`.
- [x] Integración con `taskApi.closeTask(taskId)`.
- [x] Estados de carga, error y éxito; refresco del estado a `CERRADA`.
- [x] Pruebas de componente con Vitest (habilitación condicional, cierre).

---

# Sprint 4 (ACTUAL · 6–12 oct) — Histórico, Notificaciones y Equipos

---

## HU-20 — Histórico de tarea

**Sprint:** Sprint 4 (6–12 oct)
**Módulo:** M2
**Esfuerzo estimado:** 10h
**Estado:** Desarrollo completado
**Dependencias:** HU-15, HU-16, HU-17, HU-18, HU-19

### Tareas de Base de Datos / Liquibase
- [x] Crear tabla `task_history` (`id UUID PK`, `task_id FK`, `event_type`, `previous_value`, `new_value`, `changed_by`, `changed_at`).
- [x] Liquibase changelog: changeset `create-table-task-history` con FK a `tasks` e índice `idx_history_task_changed_at`.
- [x] Rollback: `dropTable task_history`.

### Tareas de Backend
- [x] Registrar eventos de histórico (creación, cambio de estado, reasignación, cancelación, cierre) desde los UseCases existentes dentro de la misma transacción.
- [x] Endpoint `GET /api/v1/tasks/{id}/history` que retorna el histórico ordenado (`Flux<HistoryEntry>`).
- [x] UseCase `GetTaskHistoryUseCase` con consulta ordenada por `changed_at` vía `DatabaseClient`.
- [x] Manejo de errores: `404` tarea inexistente; respuesta vacía controlada.
- [x] Pruebas de integración verificando que cada operación genera su entrada de histórico.

### Tareas de Frontend
- [x] Componente `TaskHistoryTimeline` en el detalle de tarea con línea de tiempo de eventos.
- [x] Formato legible por evento (quién, qué cambió, cuándo).
- [x] Integración con `taskApi.getTaskHistory(taskId)`.
- [x] Estados de carga, error y vacío ("Sin cambios registrados").
- [x] Pruebas de componente con Vitest (render de timeline, estado vacío).

---

## HU-21 — Notificaciones de asignación

**Sprint:** Sprint 4 (6–12 oct)
**Módulo:** M3
**Esfuerzo estimado:** 10h
**Estado:** En desarrollo
**Dependencias:** HU-13, HU-17

### Tareas de Base de Datos / Liquibase
- [x] Crear tabla `notifications` (`id UUID PK`, `recipient_id`, `type`, `title`, `message`, `related_task_id`, `read`, `created_at`).
- [x] Liquibase changelog: changeset `create-table-notifications` con índice `idx_notifications_recipient_read`.
- [x] Rollback: `dropTable notifications`.

### Tareas de Backend
- [x] Generar notificación de tipo `TASK_ASSIGNED` al asignar/reasignar una tarea (invocado desde `AssignTaskUseCase`/`ReassignTaskUseCase` en la misma transacción).
- [ ] Endpoint `GET /api/v1/notifications` (del usuario autenticado) y `PATCH /api/v1/notifications/{id}/read`.
- [ ] UseCase `ListNotificationsUseCase` y `MarkNotificationReadUseCase` vía `DatabaseClient`.
- [ ] Entrega en tiempo real mediante stream reactivo (`GET /api/v1/notifications/stream`, `Flux<ServerSentEvent>`).
- [ ] Manejo de errores: `404` notificación inexistente; control de propiedad (solo el destinatario).
- [ ] Pruebas de integración de generación, listado, marcado como leída y stream SSE.

### Tareas de Frontend
- [x] Componente `NotificationBell` con contador de no leídas en la barra superior.
- [ ] Panel desplegable `NotificationsPanel` con lista y acción "marcar como leída".
- [ ] Suscripción al stream SSE para actualización en tiempo real (sin recarga).
- [ ] Integración con `notificationApi.list()`, `notificationApi.markRead(id)` y `notificationApi.stream()`.
- [ ] Estados de carga, error y vacío ("Sin notificaciones").
- [ ] Pruebas de componente con Vitest (contador, marcado como leída).

---

## HU-24 — Alertas de vencimiento

**Sprint:** Sprint 4 (6–12 oct)
**Módulo:** M3
**Esfuerzo estimado:** 12h
**Estado:** No iniciada
**Dependencias:** HU-12, HU-21

### Tareas de Base de Datos / Liquibase
- [ ] Reutilizar `notifications` con tipos `TASK_DUE_SOON` y `TASK_OVERDUE`; agregar columna `due_evaluated_at` en `tasks` para evitar reevaluaciones.
- [ ] Liquibase changelog: changeset `add-tasks-due-evaluated-at` con columna `due_evaluated_at TIMESTAMP`.
- [ ] Rollback: `dropColumn due_evaluated_at`.

### Tareas de Backend
- [ ] Implementar proceso programado reactivo (scheduler) que evalúe tareas próximas a vencer y vencidas.
- [ ] UseCase `EvaluateDueTasksUseCase` que consulta tareas por `due_date` vía `DatabaseClient` y genera notificaciones `TASK_DUE_SOON` (a responsables) y `TASK_OVERDUE` (al coordinador).
- [ ] Idempotencia: no duplicar alertas usando `due_evaluated_at`.
- [ ] Validaciones y manejo de errores del job; logging/trazabilidad de la ejecución.
- [ ] Pruebas de integración del scheduler con tareas en distintos estados de vencimiento.

### Tareas de Frontend
- [ ] Mostrar alertas de vencimiento en `NotificationsPanel` diferenciando próximas a vencer y vencidas.
- [ ] Indicador visual en el listado de tareas para tareas vencidas / próximas a vencer.
- [ ] Integración vía `notificationApi` (reutiliza stream SSE de HU-21).
- [ ] Estados de carga, error y vacío.
- [ ] Pruebas de componente con Vitest (render diferenciado de tipos de alerta).

---

## HU-31 — Crear equipo

**Sprint:** Sprint 4 (6–12 oct)
**Módulo:** M5
**Esfuerzo estimado:** 5h
**Estado:** No iniciada
**Dependencias:** HU-40, Middleware de validación de token

### Tareas de Base de Datos / Liquibase
- [ ] Crear tabla `teams` (`id UUID PK`, `name`, `description`, `coordinator_id`, `created_at`, `updated_at`).
- [ ] Crear tabla de relación `team_members` (`team_id FK`, `user_id FK`, `joined_at`).
- [ ] Liquibase changelog: changesets `create-table-teams` y `create-table-team-members` con FKs a `users` e índices `idx_teams_coordinator`, `idx_team_members_user`.
- [ ] Rollback: `dropTable team_members`, `dropTable teams`.

### Tareas de Backend
- [ ] Endpoint `POST /api/v1/teams` para crear equipo (nombre, descripción, coordinador).
- [ ] UseCase `CreateTeamUseCase` que inserta vía `DatabaseClient` dentro de `TransactionalOperator`.
- [ ] Validaciones: nombre único, coordinador con rol Coordinador, unicidad de nombre por organización.
- [ ] Manejo de errores: `400` datos inválidos, `409` nombre duplicado, `403` actor no Administrador.
- [ ] Pruebas de integración de creación de equipo.

### Tareas de Frontend
- [ ] Página `CreateTeamPage` con formulario (nombre, descripción, selector de coordinador).
- [ ] Validación: nombre requerido, coordinador seleccionado.
- [ ] Integración con `teamApi.createTeam(payload)`.
- [ ] Estados de carga, error y éxito; redirección al listado de equipos.
- [ ] Pruebas de componente con Vitest (validación, envío del formulario).

---

## HU-41 — Listar equipos

**Sprint:** Sprint 4 (6–12 oct)
**Módulo:** M5
**Esfuerzo estimado:** 4h
**Estado:** No iniciada
**Dependencias:** HU-31

### Tareas de Base de Datos / Liquibase
- [ ] Verificar índices sobre `teams(coordinator_id)` y `teams(name)` para listado/búsqueda.
- [ ] Liquibase changelog: changeset `add-index-teams-name` con índice `idx_teams_name`.
- [ ] Rollback: `dropIndex idx_teams_name`.

### Tareas de Backend
- [ ] Endpoint `GET /api/v1/teams` con paginación y filtro por nombre.
- [ ] UseCase `ListTeamsUseCase` que retorna `Flux<Team>` con conteo de miembros (JOIN explícito con `team_members`) vía `DatabaseClient`.
- [ ] Validación de parámetros de paginación.
- [ ] Manejo de errores: `400` parámetros inválidos; respuesta vacía controlada.
- [ ] Pruebas de integración del listado con y sin equipos.

### Tareas de Frontend
- [ ] Página `TeamsListPage` con tabla de equipos (nombre, coordinador, nº de miembros).
- [ ] Filtro por nombre; paginación.
- [ ] Integración con `teamApi.listTeams(filters)`.
- [ ] Estados de carga, error y vacío ("No hay equipos registrados").
- [ ] Pruebas de componente con Vitest (render de tabla, filtro, estado vacío).

---

# Sprint 5 (13–19 oct) — Equipos, Panel e inicio de Certificación

> A partir de este sprint se incorporan tareas de certificación.

---

## HU-32 — Asignar / reasignar coordinador

**Sprint:** Sprint 5 (13–19 oct)
**Módulo:** M5
**Esfuerzo estimado:** 4h
**Estado:** No iniciada
**Dependencias:** HU-31, HU-41

### Tareas de Base de Datos / Liquibase
- [ ] Reutilizar `teams.coordinator_id`; agregar columna de auditoría `coordinator_assigned_at`.
- [ ] Liquibase changelog: changeset `add-teams-coordinator-audit` con columna `coordinator_assigned_at TIMESTAMP`.
- [ ] Rollback: `dropColumn coordinator_assigned_at`.

### Tareas de Backend
- [ ] Endpoint `PATCH /api/v1/teams/{id}/coordinator` para asignar/reasignar coordinador.
- [ ] UseCase `AssignCoordinatorUseCase` que actualiza `coordinator_id` vía `TransactionalOperator`.
- [ ] Validaciones: usuario con rol Coordinador, equipo existente.
- [ ] Manejo de errores: `404` equipo/usuario inexistente, `400` rol inválido, `403` actor no Administrador.
- [ ] Pruebas de integración de asignación y reasignación de coordinador.

### Tareas de Frontend
- [ ] Acción "Asignar coordinador" en el detalle del equipo con selector de usuarios Coordinador.
- [ ] Validación: coordinador seleccionado.
- [ ] Integración con `teamApi.assignCoordinator(teamId, userId)`.
- [ ] Estados de carga, error y éxito; refresco del coordinador mostrado.
- [ ] Pruebas de componente con Vitest (selección, reasignación).

### Tareas de Certificación
- [ ] Preparar casos de prueba: asignación inicial, reasignación, usuario sin rol Coordinador.
- [ ] Validación funcional del flujo contra los criterios de aceptación de la HU.
- [ ] Validación de integración con el listado y detalle de equipos.

---

## HU-33 — Reasignar colaborador

**Sprint:** Sprint 5 (13–19 oct)
**Módulo:** M5
**Esfuerzo estimado:** 4h
**Estado:** No iniciada
**Dependencias:** HU-31

### Tareas de Base de Datos / Liquibase
- [ ] Reutilizar `team_members`; soportar movimiento de colaborador entre equipos (eliminar + insertar).
- [ ] Liquibase changelog: sin cambios de esquema nuevos; documentar dependencia de `team_members`.
- [ ] Rollback: N/A.

### Tareas de Backend
- [ ] Endpoint `PUT /api/v1/teams/{id}/members` para actualizar los colaboradores del equipo.
- [ ] UseCase `ReassignMemberUseCase` que ajusta la membresía en una sola transacción (`TransactionalOperator`).
- [ ] Validaciones: usuarios con rol Colaborador, no duplicar membresía, equipo existente.
- [ ] Manejo de errores: `404` equipo/usuario inexistente, `409` membresía duplicada.
- [ ] Pruebas de integración de reasignación de miembros.

### Tareas de Frontend
- [ ] Componente `TeamMembersEditor` con selector múltiple de colaboradores en el detalle del equipo.
- [ ] Validación: coherencia de la lista de miembros.
- [ ] Integración con `teamApi.updateMembers(teamId, userIds)`.
- [ ] Estados de carga, error y éxito; refresco de la membresía sin recarga.
- [ ] Pruebas de componente con Vitest (edición de miembros).

### Tareas de Certificación
- [ ] Preparar casos de prueba: agregar, quitar y mover colaborador entre equipos.
- [ ] Validación funcional contra criterios de aceptación.
- [ ] Validación de integración con la lista de equipos y el conteo de miembros.

---

## HU-25 — Panel de seguimiento con filtros

**Sprint:** Sprint 5 (13–19 oct)
**Módulo:** M4
**Esfuerzo estimado:** 28h
**Estado:** No iniciada
**Dependencias:** HU-14, HU-15, HU-20

### Tareas de Base de Datos / Liquibase
- [ ] Verificar/crear índices de agregación: `idx_tasks_status`, `idx_tasks_due_date`, `idx_assignments_user`.
- [ ] Liquibase changelog: changeset `add-indexes-dashboard` con índices compuestos para consultas del panel.
- [ ] Rollback: `dropIndex` de los índices añadidos.

### Tareas de Backend
- [ ] Endpoint `GET /api/v1/dashboard/tasks` con filtros por usuario/colaborador, estado y rango de fechas.
- [ ] UseCase `GetDashboardUseCase` con consultas agregadas (conteos por estado, vencidas, próximas a vencer) vía `DatabaseClient` y SQL explícito.
- [ ] Endpoint `GET /api/v1/dashboard/stream` (`Flux<ServerSentEvent>`) para actualización en tiempo real del panel.
- [ ] Validaciones de filtros; manejo de errores `400`.
- [ ] Pruebas de integración de agregaciones y filtros combinados; verificación no bloqueante.

### Tareas de Frontend
- [ ] Página `DashboardPage` con tarjetas resumen (totales, por estado, vencidas, próximas a vencer).
- [ ] Panel de filtros (colaborador, estado, rango de fechas) con aplicación reactiva.
- [ ] Tabla de tareas filtrada; actualización en tiempo real vía SSE sin recarga.
- [ ] Integración con `dashboardApi.getTasks(filters)` y `dashboardApi.stream()`.
- [ ] Estados de carga, error y vacío ("No hay datos para los filtros seleccionados").
- [ ] Pruebas de componente con Vitest (filtros, render de tarjetas, estado vacío).

### Tareas de Certificación
- [ ] Preparar casos de prueba: filtros individuales y combinados, panel sin datos, actualización en tiempo real.
- [ ] Validación funcional contra criterios de aceptación de la HU.
- [ ] Validación de integración con los módulos M2 (tareas) y M5 (equipos/usuarios).

---

## HU-28 — Configuración de la organización

**Sprint:** Sprint 5 (13–19 oct)
**Módulo:** M5
**Esfuerzo estimado:** 5h
**Estado:** No iniciada
**Dependencias:** Middleware de validación de token

### Tareas de Base de Datos / Liquibase
- [ ] Crear tabla `org_settings` (`id UUID PK`, `timezone`, `notifications_enabled`, `updated_at`, `updated_by`).
- [ ] Liquibase changelog: changeset `create-table-org-settings` con fila por defecto (zona horaria `America/Bogota`, notificaciones habilitadas).
- [ ] Rollback: `dropTable org_settings`.

### Tareas de Backend
- [ ] Endpoints `GET /api/v1/settings` y `PUT /api/v1/settings` para consultar y actualizar la configuración.
- [ ] UseCase `GetSettingsUseCase` y `UpdateSettingsUseCase` vía `DatabaseClient`/`TransactionalOperator`.
- [ ] Validaciones: zona horaria válida (IANA), solo Administrador puede modificar.
- [ ] Manejo de errores: `400` zona horaria inválida, `403` actor no Administrador.
- [ ] Pruebas de integración de lectura y actualización de configuración.

### Tareas de Frontend
- [ ] Página `OrgSettingsPage` con formulario (zona horaria, activar/desactivar notificaciones).
- [ ] Validación: zona horaria seleccionada de un catálogo válido.
- [ ] Integración con `settingsApi.get()` y `settingsApi.update(payload)`.
- [ ] Estados de carga, error y confirmación de guardado.
- [ ] Pruebas de componente con Vitest (carga de configuración, guardado).

### Tareas de Certificación
- [ ] Preparar casos de prueba: actualización válida, zona horaria inválida, acceso de rol no Administrador.
- [ ] Validación funcional contra criterios de aceptación.
- [ ] Validación de integración con los módulos que consumen la zona horaria (fechas de tareas, alertas).

---

# Sprint 6 (20–26 oct) — Indicadores, Reportes, Integración y Certificación

---

## HU-27 — Indicadores de cumplimiento

**Sprint:** Sprint 6 (20–26 oct)
**Módulo:** M4
**Esfuerzo estimado:** 26h
**Estado:** No iniciada
**Dependencias:** HU-25, HU-20

### Tareas de Base de Datos / Liquibase
- [ ] Evaluar vistas de agregación o consultas optimizadas para indicadores (% cumplimiento, retraso, tiempos de cierre).
- [ ] Liquibase changelog: changeset `create-view-task-metrics` con vista `v_task_metrics` (o índices de soporte si se calcula en consulta).
- [ ] Rollback: `dropView v_task_metrics` / `dropIndex` asociados.

### Tareas de Backend
- [ ] Endpoint `GET /api/v1/indicators/compliance` con filtros por equipo/colaborador y período.
- [ ] UseCase `GetComplianceIndicatorsUseCase` que calcula: % de cumplimiento, tareas con retraso, tiempos promedio de cierre, carga por responsable, vía `DatabaseClient` con SQL de agregación.
- [ ] Validaciones de filtros/período; manejo de errores `400`.
- [ ] Pruebas de integración de los cálculos con datos sembrados representativos.
- [ ] Verificación no bloqueante (BlockHound) de las consultas de agregación.

### Tareas de Frontend
- [ ] Página `IndicatorsPage` con gráficos (cumplimiento, retraso, tiempos de cierre, carga por responsable).
- [ ] Filtros por equipo/colaborador y período.
- [ ] Integración con `indicatorApi.getCompliance(filters)`.
- [ ] Estados de carga, error y vacío ("Sin datos suficientes para calcular indicadores").
- [ ] Pruebas de componente con Vitest (render de gráficos, filtros, estado vacío).

### Tareas de Certificación
- [ ] Preparar casos de prueba: cálculo con datos conocidos, filtros por período, equipo sin datos.
- [ ] Validación funcional de los indicadores contra valores esperados.
- [ ] Validación de integración con el panel (HU-25) y el histórico (HU-20).

---

## HU-29 — Reportes de productividad

**Sprint:** Sprint 6 (20–26 oct)
**Módulo:** M5
**Esfuerzo estimado:** 14h
**Estado:** No iniciada
**Dependencias:** HU-27, HU-20

### Tareas de Base de Datos / Liquibase
- [ ] Reutilizar vista/consultas de métricas (HU-27); evaluar índices adicionales para rangos de fecha.
- [ ] Liquibase changelog: changeset `add-indexes-productivity-reports` si se requieren índices por fecha; documentar rollback.
- [ ] Rollback: `dropIndex` de los índices añadidos.

### Tareas de Backend
- [ ] Endpoint `GET /api/v1/reports/productivity` con filtros por período, equipo y colaborador.
- [ ] UseCase `GetProductivityReportUseCase` que agrega tareas completadas, cumplimiento y carga por responsable vía `DatabaseClient`.
- [ ] Endpoint de exportación `GET /api/v1/reports/productivity/export` (CSV) generado de forma reactiva (streaming).
- [ ] Validaciones de filtros; manejo de errores `400`.
- [ ] Pruebas de integración del reporte y de la exportación.

### Tareas de Frontend
- [ ] Página `ProductivityReportPage` con tabla/gráfico y filtros por período/equipo/colaborador.
- [ ] Acción "Exportar CSV".
- [ ] Integración con `reportApi.getProductivity(filters)` y `reportApi.exportProductivity(filters)`.
- [ ] Estados de carga, error y vacío.
- [ ] Pruebas de componente con Vitest (filtros, exportación, estado vacío).

### Tareas de Certificación
- [ ] Preparar casos de prueba: reporte por período, exportación CSV, filtros combinados.
- [ ] Validación funcional contra criterios de aceptación.
- [ ] Validación de integración con indicadores (HU-27) y datos de tareas (M2).

---

## HU-38 — Reportes de retraso y cierre

**Sprint:** Sprint 6 (20–26 oct)
**Módulo:** M5
**Esfuerzo estimado:** 14h
**Estado:** No iniciada
**Dependencias:** HU-27, HU-19, HU-20

### Tareas de Base de Datos / Liquibase
- [ ] Reutilizar métricas de retraso y `closed_at` (de HU-19); evaluar índices por `closed_at` y `due_date`.
- [ ] Liquibase changelog: changeset `add-indexes-delay-closure-reports` con índices de soporte; documentar rollback.
- [ ] Rollback: `dropIndex` de los índices añadidos.

### Tareas de Backend
- [ ] Endpoint `GET /api/v1/reports/delays` (tareas con retraso) y `GET /api/v1/reports/closures` (tiempos de cierre).
- [ ] UseCase `GetDelayReportUseCase` y `GetClosureReportUseCase` con cálculos de retraso y tiempo de cierre vía `DatabaseClient`.
- [ ] Exportación CSV reactiva de ambos reportes.
- [ ] Validaciones de filtros; manejo de errores `400`.
- [ ] Pruebas de integración de ambos reportes y sus exportaciones.

### Tareas de Frontend
- [ ] Página `DelayClosureReportPage` con dos vistas (retraso y cierre), filtros y exportación CSV.
- [ ] Integración con `reportApi.getDelays(filters)` y `reportApi.getClosures(filters)`.
- [ ] Estados de carga, error y vacío.
- [ ] Pruebas de componente con Vitest (cambio de vista, filtros, exportación).

### Tareas de Certificación
- [ ] Preparar casos de prueba: tareas con y sin retraso, cálculo de tiempo de cierre, exportaciones.
- [ ] Validación funcional contra criterios de aceptación.
- [ ] Validación de integración con el cierre de tareas (HU-19) y el histórico (HU-20).

---

# Sprint 7 (27–31 oct) — Integración final, pruebas, documentación y despliegue

No introduce nuevas HU. Consolida el incremento y prepara la entrega (54 h de integración,
50 h de pruebas, 25 h de documentación y 18 h de entrega distribuidas en el período).

### Tareas de Integración
- [ ] Verificar integración extremo a extremo de los flujos M2 → M3 → M4 (asignación → notificación → panel/indicadores).
- [ ] Validar la cadena no bloqueante completa con BlockHound en los flujos integrados.
- [ ] Revisar y eliminar exclusiones de cobertura pendientes (módulo `companies` y configuración).
- [ ] Verificar reglas de dependencia de Clean Architecture con ArchUnit en todos los módulos.

### Tareas de Pruebas
- [ ] Ejecutar pruebas funcionales por módulo (M2–M5) y registrar casos documentados.
- [ ] Pruebas de integración entre módulos.
- [ ] Pruebas de usabilidad en la pyme experimental (según plan S11).
- [ ] Confirmar puertas de calidad en CI: backend línea ≥ 80 % / rama ≥ 60 % (JaCoCo/SonarQube); frontend Vitest (80 %/60 %) + oxlint.

### Tareas de Documentación
- [ ] Actualizar documentación OpenAPI/Swagger de todos los endpoints con ejemplos completos de request/response y errores.
- [ ] Generar diagramas de flujo (Mermaid) de los procesos de tareas, notificaciones y panel.
- [ ] Actualizar manuales de usuario e instalación.
- [ ] Consolidar el resumen técnico del incremento.

### Tareas de Entrega / Despliegue
- [ ] Construir imagen Docker de producción del backend.
- [ ] Ejecutar migraciones Liquibase contra el entorno destino.
- [ ] Desplegar backend y frontend en la nube (proveedor general pendiente — ADR-06; identidad fijada en AWS por Cognito).
- [ ] Verificación post-despliegue (smoke tests) de los flujos críticos.

---

## 4. Trazabilidad resumen (HU → Sprint → Esfuerzo)

| HU | Módulo | Sprint | Esfuerzo | Estado |
|---|---|---|---|---|
| HU-01…HU-08 | M1 | — | 51 h (total M1) | Completada |
| HU-40 | M5 | 1 | 5 h | Completada |
| HU-10 | M5 | 1 | 8 h | Completada |
| HU-12 | M2 | 2 | 8 h | Completada |
| HU-13 | M2 | 2 | 6 h | Completada |
| HU-14 | M2 | 2 | 6 h | Completada |
| HU-15 | M2 | 3 | 8 h | Completada |
| HU-16 | M2 | 3 | 5 h | Completada |
| HU-17 | M2 | 3 | 6 h | Completada |
| HU-18 | M2 | 3 | 5 h | Completada |
| HU-19 | M2 | 3 | 6 h | Completada |
| HU-20 | M2 | 4 | 10 h | Desarrollo completado |
| HU-21 | M3 | 4 | 10 h | En desarrollo |
| HU-24 | M3 | 4 | 12 h | No iniciada |
| HU-31 | M5 | 4 | 5 h | No iniciada |
| HU-41 | M5 | 4 | 4 h | No iniciada |
| HU-32 | M5 | 5 | 4 h | No iniciada |
| HU-33 | M5 | 5 | 4 h | No iniciada |
| HU-25 | M4 | 5 | 28 h | No iniciada |
| HU-28 | M5 | 5 | 5 h | No iniciada |
| HU-27 | M4 | 6 | 26 h | No iniciada |
| HU-29 | M5 | 6 | 14 h | No iniciada |
| HU-38 | M5 | 6 | 14 h | No iniciada |

> **Nota de alcance:** este documento desglosa tareas técnicas trazables a las HU aprobadas. Los
> nombres de tablas, endpoints, columnas y componentes son el diseño técnico propuesto para la
> implementación bajo el stack vigente; cuando una HU aún no haya definido un detalle funcional,
> dicho detalle debe confirmarse con la HU correspondiente (`Pendiente por definir`) y no asumirse
> desde este desglose.
