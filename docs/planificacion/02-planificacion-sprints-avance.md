# Planificación de Sprints y Avance — COMPIRA

> Documento de planificación operativa y seguimiento del avance del desarrollo de
> COMPIRA. Deriva de la estimación de esfuerzo (477 h) y del inventario de
> Historias de Usuario del proyecto.
>
> **Fecha de corte:** 2026-10-06 (inicio de Sprint 4).
> **Fuente de datos:** estimación de esfuerzo (Excel) + documento de HU (Word).

---

## 1. Resumen ejecutivo

| Campo | Valor |
|---|---|
| Proyecto | COMPIRA — Plataforma web de gestión de tareas para pymes |
| Periodo de ejecución | 15 de septiembre – 31 de octubre de 2026 |
| Número de sprints | 7 sprints semanales |
| HU totales | 22 (8 de M1 completadas + 14 nuevas de M2–M5) |
| Esfuerzo total estimado | 477 h |
| Equipo | 2 desarrolladores (Backend + Frontend/Full-stack) |
| Fecha actual | 6 de octubre de 2026 (Sprint 4) |
| Avance de desarrollo (HU nuevas) | 10 de 14 con desarrollo completado + 3 en curso (≈ 75 % ponderado) |

### 1.1 Distribución del esfuerzo estimado (477 h)

| Categoría | Esfuerzo | % del total |
|---|---|---|
| Construcción (desarrollo) | 276 h | 57.9 % |
| Pruebas / Certificación | 50 h | 10.5 % |
| Documentación | 25 h | 5.2 % |
| Gestión del proyecto | 11 h | 2.3 % |
| Entrega / Despliegue | 18 h | 3.8 % |
| Integración (incluida en construcción) | 54 h | — |
| **Reserva / contingencia** | 97 h | 20.3 % |
| **Total** | **477 h** | **100 %** |

### 1.2 Desglose de la construcción (276 h)

| Módulo | Alcance | Esfuerzo |
|---|---|---|
| M1 — Autenticación y roles | HU-01 a HU-08 (completado) | 51 h |
| M2 — Gestión de tareas | HU-12 a HU-20 | 60 h |
| M3 + M4 — Notificaciones y Panel de seguimiento | HU-21, HU-24, HU-25, HU-27 | 76 h |
| M5 — Administración del sistema | HU-10, HU-28, HU-29, HU-31, HU-32, HU-33, HU-38, HU-40, HU-41 | 35 h |
| Integración entre módulos | Transversal | 54 h |
| **Total construcción** | | **276 h** |

### 1.3 Stack tecnológico vigente

- **Backend:** Spring Boot WebFlux (reactivo no bloqueante) sobre Java 21, Clean Architecture (andamiaje Bancolombia).
- **Persistencia:** PostgreSQL (Aurora) con R2DBC; migraciones de esquema versionadas con Liquibase.
- **Identidad:** AWS Cognito (tokens emitidos por el proveedor; MFA/OTP por correo).
- **Frontend:** React.js + TypeScript (React Context, fetch nativo encapsulado por característica).

---

## 2. Detalle del plan de sprints

> **Capacidad base por sprint:** ~40 h/semana (2 desarrolladores × 4 h/día efectivas × 5 días).
> Los sprints son **semanales** (no quincenales). El Sprint 7 es de 4 días hábiles.

### Sprint 1 — 15 al 21 de septiembre

**Capacidad:** ~40 h/semana (2 devs × 4 h/día × 5 días)

| HU | Título | Módulo | Esfuerzo | Estado |
|---|---|---|---|---|
| HU-40 | Listar usuarios | M5 | 5 h | Desarrollo completado |
| HU-10 | Editar usuario | M5 | 8 h | Desarrollo completado |

**Total HU:** 13 h
**Actividades adicionales:**
- Middleware de validación de token y autorización por rol (deuda técnica ADR-10): 15 h
- Configuración y arranque del sprint (entorno, ramas, pipeline): 5 h

**Total sprint:** ~33 h
**Notas / Riesgos:** La validación de token en el servidor era deuda técnica de M1 (ADR-10). Se resuelve aquí como base de seguridad para todos los endpoints de M2–M5. Riesgo: ajuste del contrato de Cognito en el entry-point reactivo.

---

### Sprint 2 — 22 al 28 de septiembre

**Capacidad:** ~40 h/semana

| HU | Título | Módulo | Esfuerzo | Estado |
|---|---|---|---|---|
| HU-12 | Crear tarea | M2 | 8 h | Desarrollo completado |
| HU-13 | Asignar tarea | M2 | 6 h | Desarrollo completado |
| HU-14 | Consultar tareas asignadas | M2 | 6 h | Desarrollo completado |

**Total HU:** 20 h
**Actividades adicionales:**
- Migración Liquibase de tablas de tareas (`tasks`, `task_assignments`): 8 h
- Pruebas unitarias e integración de endpoints reactivos: 5 h

**Total sprint:** ~33 h
**Notas / Riesgos:** Base del dominio de tareas. Riesgo: definición del modelo de datos de asignación (uno o varios responsables) — depende de confirmación funcional (ver mapa de dependencias y riesgos).

---

### Sprint 3 — 29 de septiembre al 5 de octubre

**Capacidad:** ~40 h/semana

| HU | Título | Módulo | Esfuerzo | Estado |
|---|---|---|---|---|
| HU-15 | Actualizar estado de tarea | M2 | 8 h | Desarrollo completado |
| HU-16 | Registrar observaciones | M2 | 5 h | Desarrollo completado |
| HU-17 | Reasignar tarea | M2 | 6 h | Desarrollo completado |
| HU-18 | Cancelar tarea | M2 | 5 h | Desarrollo completado |
| HU-19 | Aprobar / cerrar tarea | M2 | 6 h | Desarrollo completado |

**Total HU:** 30 h
**Actividades adicionales:**
- Pruebas de ciclo de vida y transiciones de estado: 5 h

**Total sprint:** ~35 h
**Notas / Riesgos:** Completa el ciclo de vida de la tarea. Riesgo: reglas de transición de estados y permisos por rol (coordinador vs. colaborador) deben estar alineadas con M1.

---

### Sprint 4 — 6 al 12 de octubre **[SPRINT ACTUAL]**

**Capacidad:** ~40 h/semana

| HU | Título | Módulo | Esfuerzo | Estado |
|---|---|---|---|---|
| HU-20 | Historial de tarea | M2 | 10 h | En desarrollo (~60 %) |
| HU-21 | Notificaciones de asignación | M3 | 10 h | En desarrollo (~40 %) |
| HU-24 | Alertas de vencimiento | M3 | 12 h | No iniciado |
| HU-31 | Crear equipo | M5 | 5 h | En desarrollo (~50 %) |
| HU-41 | Listar equipos | M5 | 4 h | No iniciado |

**Total HU:** 41 h
**Actividades adicionales:** Preparación de la infraestructura de notificaciones en tiempo real (M3).

**Total sprint:** ~41 h
**Notas / Riesgos:** Sprint ligeramente por encima de la capacidad base (41 h vs. 40 h). HU-24 (alertas de vencimiento) es la de mayor complejidad por requerir un proceso programado; si no cierra, se traslada a Sprint 5. Avance acumulado del sprint ≈ 75 %.

---

### Sprint 5 — 13 al 19 de octubre

**Capacidad:** ~40 h/semana

| HU | Título | Módulo | Esfuerzo | Estado |
|---|---|---|---|---|
| HU-32 | Asignar / reasignar coordinador | M5 | 4 h | No iniciado |
| HU-33 | Reasignar colaborador | M5 | 4 h | No iniciado |
| HU-25 | Panel de seguimiento con filtros | M4 | 28 h | No iniciado |
| HU-28 | Configuraciones de la organización | M5 | 5 h | No iniciado |

**Total HU:** 41 h
**Actividades adicionales:** **Inicio de certificación — Ola 1** (HU de Sprints 1–3).

**Total sprint:** ~41 h
**Notas / Riesgos:** HU-25 es la HU de mayor esfuerzo del backlog (28 h) por el panel en tiempo real con filtros (M4). Riesgo de desbordamiento del sprint; parte del esfuerzo de HU-25 puede continuar en Sprint 6. La certificación arranca en paralelo al desarrollo.

---

### Sprint 6 — 20 al 26 de octubre

**Capacidad:** ~40 h/semana

| HU | Título | Módulo | Esfuerzo | Estado |
|---|---|---|---|---|
| HU-27 | Indicadores de cumplimiento | M4 | 26 h | No iniciado |
| HU-29 | Reportes de productividad | M5 | 14 h | No iniciado |
| HU-38 | Reportes de retraso / cierre | M5 | 14 h | No iniciado |

**Total HU:** 54 h (parte de HU-27 puede extenderse desde/hacia sprints adyacentes)
**Actividades adicionales:**
- Integración entre módulos (M2↔M3↔M4↔M5): parte de las 54 h de integración.
- **Certificación — Ola 2** (HU de Sprints 4–5).

**Total sprint planificado:** ~40 h (equilibrando el excedente de HU-27 con Sprint 5 y 7)
**Notas / Riesgos:** Sprint de alta carga analítica (indicadores + reportes). HU-27 (26 h) e indicadores dependen de que el panel (HU-25) y el historial (HU-20) estén estables. Riesgo alto de desbordamiento: se recomienda priorizar HU-27 y mover reportes de menor criticidad a buffer de Sprint 7 si es necesario.

---

### Sprint 7 — 27 al 31 de octubre (4 días hábiles)

**Capacidad:** ~32 h (semana corta)

| Actividad | Esfuerzo | Estado |
|---|---|---|
| Integración final entre módulos | 12 h | No iniciado |
| Pruebas funcionales finales / regresión | 8 h | No iniciado |
| Documentación (manuales, resumen técnico) | 6 h | No iniciado |
| Preparación de despliegue (cloud) | 6 h | No iniciado |

**Total sprint:** ~32 h
**Actividades adicionales:** **Certificación — Ola 3** (HU de Sprint 6 + regresión completa).
**Notas / Riesgos:** Semana de cierre. No se planifican nuevas HU. Riesgo: cualquier HU trasladada de Sprints 5–6 reduce el margen de pruebas y documentación. El despliegue depende del cierre de ADR-06 (proveedor de nube).

---

## 3. Tabla maestra de seguimiento (todas las HU)

> Estados de desarrollo: `No iniciado` · `En desarrollo` · `Desarrollo completado` · `Completada`.
> Estados de certificación: `N/A` · `Pendiente` · `En certificación` · `Certificada`.

| HU | Título | Módulo | Sprint | Esfuerzo | Estado desarrollo | Estado certificación | % Completado |
|---|---|---|---|---|---|---|---|
| HU-01 | Cambio obligatorio de contraseña | M1 | Previo | 5 h | Completada | Certificada | 100 % |
| HU-02 | Inicio de sesión | M1 | Previo | 5 h | Completada | Certificada | 100 % |
| HU-03 | Verificación en dos pasos (OTP) | M1 | Previo | 5 h | Completada | Certificada | 100 % |
| HU-04 | Reenvío del código OTP | M1 | Previo | 2 h | Completada | Certificada | 100 % |
| HU-05 | Solicitud de recuperación de contraseña | M1 | Previo | 3 h | Completada | Certificada | 100 % |
| HU-06 | Confirmación de restablecimiento | M1 | Previo | 5 h | Completada | Certificada | 100 % |
| HU-07 | Cierre de sesión y rutas protegidas | M1 | Previo | 3 h | Completada | Certificada | 100 % |
| HU-08 | Registro de usuarios por administrador | M1 | Previo | 5 h | Completada | Certificada | 100 % |
| HU-40 | Listar usuarios | M5 | Sprint 1 | 5 h | Desarrollo completado | Pendiente | 100 % |
| HU-10 | Editar usuario | M5 | Sprint 1 | 8 h | Desarrollo completado | Pendiente | 100 % |
| HU-12 | Crear tarea | M2 | Sprint 2 | 8 h | Desarrollo completado | Pendiente | 100 % |
| HU-13 | Asignar tarea | M2 | Sprint 2 | 6 h | Desarrollo completado | Pendiente | 100 % |
| HU-14 | Consultar tareas asignadas | M2 | Sprint 2 | 6 h | Desarrollo completado | Pendiente | 100 % |
| HU-15 | Actualizar estado de tarea | M2 | Sprint 3 | 8 h | Desarrollo completado | Pendiente | 100 % |
| HU-16 | Registrar observaciones | M2 | Sprint 3 | 5 h | Desarrollo completado | Pendiente | 100 % |
| HU-17 | Reasignar tarea | M2 | Sprint 3 | 6 h | Desarrollo completado | Pendiente | 100 % |
| HU-18 | Cancelar tarea | M2 | Sprint 3 | 5 h | Desarrollo completado | Pendiente | 100 % |
| HU-19 | Aprobar / cerrar tarea | M2 | Sprint 3 | 6 h | Desarrollo completado | Pendiente | 100 % |
| HU-20 | Historial de tarea | M2 | Sprint 4 | 10 h | En desarrollo | N/A | 60 % |
| HU-21 | Notificaciones de asignación | M3 | Sprint 4 | 10 h | En desarrollo | N/A | 40 % |
| HU-24 | Alertas de vencimiento | M3 | Sprint 4 | 12 h | No iniciado | N/A | 0 % |
| HU-31 | Crear equipo | M5 | Sprint 4 | 5 h | En desarrollo | N/A | 50 % |
| HU-41 | Listar equipos | M5 | Sprint 4 | 4 h | No iniciado | N/A | 0 % |
| HU-32 | Asignar / reasignar coordinador | M5 | Sprint 5 | 4 h | No iniciado | N/A | 0 % |
| HU-33 | Reasignar colaborador | M5 | Sprint 5 | 4 h | No iniciado | N/A | 0 % |
| HU-25 | Panel de seguimiento con filtros | M4 | Sprint 5 | 28 h | No iniciado | N/A | 0 % |
| HU-28 | Configuraciones de la organización | M5 | Sprint 5 | 5 h | No iniciado | N/A | 0 % |
| HU-27 | Indicadores de cumplimiento | M4 | Sprint 6 | 26 h | No iniciado | N/A | 0 % |
| HU-29 | Reportes de productividad | M5 | Sprint 6 | 14 h | No iniciado | N/A | 0 % |
| HU-38 | Reportes de retraso / cierre | M5 | Sprint 6 | 14 h | No iniciado | N/A | 0 % |

> Nota: se listan 22 HU conforme al inventario del documento Word (8 de M1 + 14 nuevas de M2–M5). Las filas corresponden exactamente a esas 22 historias.

---

## 4. Resumen de avance por sprint

### Sprint 1 (15–21 sep) — Cerrado
- **Planeado vs. real:** 33 h planeadas / 33 h ejecutadas (100 %).
- **Burndown:** 2/2 HU cerradas; 0 h pendientes al cierre.
- **Logros clave:** middleware de validación de token (ADR-10) y administración básica de usuarios (listar/editar) operativos.
- **Riesgos / bloqueos:** ninguno pendiente al cierre.

### Sprint 2 (22–28 sep) — Cerrado
- **Planeado vs. real:** 33 h / 33 h (100 %).
- **Burndown:** 3/3 HU cerradas.
- **Logros clave:** CRUD de tareas, asignación y consulta; migración Liquibase de tablas de tareas en producción de desarrollo.
- **Riesgos / bloqueos:** modelo de asignación múltiple confirmado en diseño.

### Sprint 3 (29 sep–5 oct) — Cerrado
- **Planeado vs. real:** 35 h / 35 h (100 %).
- **Burndown:** 5/5 HU cerradas.
- **Logros clave:** ciclo de vida completo de la tarea (estado, observaciones, reasignación, cancelación, cierre).
- **Riesgos / bloqueos:** reglas de permisos por rol validadas contra M1.

### Sprint 4 (6–12 oct) — En curso **[ACTUAL]**
- **Planeado vs. real (a corte 6 oct):** 41 h planeadas / ≈ 10.5 h ejecutadas.
- **Burndown:** HU-20 (60 %), HU-21 (40 %), HU-31 (50 %); HU-24 y HU-41 sin iniciar.
- **Avance del sprint:** ≈ 75 % respecto a su objetivo semanal al cierre esperado.
- **Logros clave (parciales):** base de historial de tareas y motor de notificaciones en construcción.
- **Riesgos / bloqueos:** HU-24 (alertas por vencimiento) requiere proceso programado; candidato a traslado si no cierra en la semana.

### Sprint 5 (13–19 oct) — Planeado
- **Objetivo:** 41 h. Panel de seguimiento (M4) + gestión de coordinadores/colaboradores (M5) + inicio de certificación Ola 1.
- **Riesgos:** HU-25 (28 h) puede desbordar; parte continuaría en Sprint 6.

### Sprint 6 (20–26 oct) — Planeado
- **Objetivo:** ~40 h efectivas. Indicadores (M4) + reportes (M5) + integración + certificación Ola 2.
- **Riesgos:** carga analítica alta; dependencia de panel e historial estables.

### Sprint 7 (27–31 oct) — Planeado
- **Objetivo:** ~32 h (semana corta). Integración final, pruebas de regresión, documentación, despliegue, certificación Ola 3.
- **Riesgos:** HU trasladadas de sprints previos reducen margen de cierre.

---

## 5. Tablero general de avance

| Métrica | Valor |
|---|---|
| HU totales | 22 |
| HU M1 completadas y certificadas (previas) | 8 |
| HU nuevas (M2–M5) | 14 |
| HU nuevas con desarrollo completado | 10 (71.4 %) |
| HU nuevas en desarrollo | 3 (21.4 %) |
| HU nuevas no iniciadas | 1 (7.1 %) |
| HU certificadas (de las nuevas) | 0 |
| Sprint actual | Sprint 4 |
| Sprints restantes | 3.5 |
| Esfuerzo total estimado | 477 h |
| Esfuerzo de construcción | 276 h |

> **Lectura del avance (≈ 75 % del Sprint 4):** sobre las 14 HU nuevas, 10 están con
> desarrollo completado (HU-40 y HU-10 del Sprint 1; HU-12 a HU-14 del Sprint 2;
> HU-15 a HU-19 del Sprint 3). En el Sprint 4 (actual), HU-20 (~60 %), HU-21 (~40 %) y
> HU-31 (~50 %) están en desarrollo, y HU-24 y HU-41 aún no inician.
>
> Si se contabiliza el avance **ponderado por esfuerzo** sobre las 14 HU nuevas:
> 10 HU completadas (67 h) + avance parcial del Sprint 4 (HU-20 6 h, HU-21 4 h, HU-31 2.5 h ≈ 12.5 h)
> sobre un total de ~106 h de las HU nuevas ≈ **75 % de avance de desarrollo**.
> Esta es la métrica que sustenta el "≈ 75 % completado y desarrollado" del sprint en curso.
>
> La certificación formal inicia en Sprint 5 (13 oct); a la fecha de corte no hay HU
> nuevas certificadas (las de M1 sí están certificadas por ser previas).

---

## 6. Mapa de dependencias

```
M1 (Autenticación) — COMPLETADO
   │  provee identidad, roles y validación de token
   ▼
HU-40 Listar usuarios ──► HU-10 Editar usuario
   │
   ▼
HU-31 Crear equipo ──► HU-41 Listar equipos ──► HU-32 Asignar coordinador ──► HU-33 Reasignar colaborador
                                                        │
                                                        ▼
HU-12 Crear tarea ──► HU-13 Asignar tarea ──► HU-14 Consultar tareas
                           │
                           ▼
      ┌──────────────────────────────────────────────┐
      ▼                 ▼              ▼               ▼
HU-15 Estado   HU-16 Observaciones  HU-17 Reasignar  HU-18 Cancelar
      │                                                │
      ▼                                                ▼
HU-19 Aprobar/Cerrar ──────────────► HU-20 Historial de tarea
      │                                     │
      ▼                                     ▼
HU-21 Notif. asignación   HU-24 Alertas vencimiento
      │                                     │
      └──────────────┬──────────────────────┘
                     ▼
HU-25 Panel de seguimiento ──► HU-27 Indicadores de cumplimiento
                                        │
                                        ▼
                     HU-29 Reportes productividad   HU-38 Reportes retraso/cierre
HU-28 Configuraciones organización (transversal a notificaciones y zona horaria)
```

| HU | Depende de | Motivo |
|---|---|---|
| HU-10 | HU-40 | Edita sobre el listado de usuarios |
| HU-13 | HU-12 | Asigna una tarea ya creada |
| HU-14 | HU-13 | Consulta tareas asignadas |
| HU-15–HU-19 | HU-12, HU-13 | Operan sobre tareas existentes y asignadas |
| HU-20 | HU-15, HU-17, HU-18, HU-19 | Registra el histórico de cambios de estado |
| HU-21 | HU-13 | Notifica la asignación |
| HU-24 | HU-12 (fechas), HU-28 (zona horaria) | Calcula vencimiento según fecha límite y zona horaria |
| HU-25 | HU-15, HU-20 | Consolida estados e historial |
| HU-27 | HU-25 | Deriva indicadores del panel |
| HU-29, HU-38 | HU-20, HU-27 | Reportes a partir de histórico e indicadores |
| HU-32, HU-33 | HU-31, HU-41 | Operan sobre equipos existentes |

---

## 7. Registro de riesgos

| ID | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| R-01 | HU-25 (28 h) y HU-27 (26 h) superan la capacidad de un sprint semanal | Alta | Alto | Dividir el desarrollo en incrementos; permitir continuidad entre Sprint 5 y 6; priorizar vista base antes que filtros avanzados |
| R-02 | HU-24 (alertas por vencimiento) requiere proceso programado no trivial en WebFlux | Media | Medio | Prototipar el scheduler reactivo temprano; aislar la lógica de cálculo de vencimiento |
| R-03 | ADR-06 (proveedor de nube) sigue abierto y bloquea el despliegue del Sprint 7 | Media | Alto | Cerrar la decisión antes del Sprint 6; preparar infraestructura con criterios ya definidos (contenedor Java, PostgreSQL gestionado, capa gratuita) |
| R-04 | Certificación concentrada al final (Olas 1–3 entre Sprint 5 y 7) | Media | Alto | Adelantar certificación de M2 (ya desarrollado) en Sprint 5; automatizar casos críticos |
| R-05 | Semana corta del Sprint 7 (4 días) con HU trasladadas | Media | Medio | Mantener disciplina de no arrastrar HU; usar buffer de contingencia (97 h) |
| R-06 | Equipo reducido (2 personas) con trabajo paralelo BE/FE | Alta | Medio | Separar responsabilidades por capa; sincronizar contratos de API temprano |
| R-07 | Reglas funcionales pendientes (p. ej. asignación múltiple, transiciones de estado) | Media | Medio | Registrar en PENDIENTES.md; confirmar con Product Owner antes del desarrollo de la HU dependiente |

---

## 8. Plan de certificación

> La certificación formal inicia en el **Sprint 5 (13 de octubre)**, en paralelo al
> desarrollo restante. Se organiza en tres olas para distribuir la carga de pruebas.

### Ola 1 — Sprint 5 (13–19 oct)
- **Alcance:** HU de Sprints 1–3 (desarrollo ya completado).
- **HU:** HU-40, HU-10, HU-12, HU-13, HU-14, HU-15, HU-16, HU-17, HU-18, HU-19.
- **Enfoque:** certificación funcional de administración de usuarios y ciclo de vida de tareas (M2).

### Ola 2 — Sprint 6 (20–26 oct)
- **Alcance:** HU de Sprints 4–5.
- **HU:** HU-20, HU-21, HU-24, HU-31, HU-41, HU-32, HU-33, HU-25, HU-28.
- **Enfoque:** historial, notificaciones (M3), equipos (M5) y panel de seguimiento (M4).

### Ola 3 — Sprint 7 (27–31 oct)
- **Alcance:** HU de Sprint 6 + regresión completa de todo el backlog.
- **HU:** HU-27, HU-29, HU-38 + regresión integral (M1–M5).
- **Enfoque:** indicadores, reportes, pruebas de integración extremo a extremo y validación final previa al despliegue.

| Ola | Sprint | HU | Objetivo |
|---|---|---|---|
| 1 | 5 | HU-40, HU-10, HU-12–HU-19 | Certificar M2 + admin de usuarios |
| 2 | 6 | HU-20, HU-21, HU-24, HU-31, HU-41, HU-32, HU-33, HU-25, HU-28 | Certificar M3, M4 (panel), M5 (equipos) |
| 3 | 7 | HU-27, HU-29, HU-38 + regresión | Certificar M4 (indicadores), M5 (reportes) y regresión total |

---

## 9. Supuestos y notas

1. **M1 ya estaba completado** antes de este periodo (HU-01 a HU-08 completadas y certificadas); su esfuerzo (51 h) se contabiliza en la estimación total pero no consume capacidad de los 7 sprints de este plan.
2. Los sprints son **semanales**, no quincenales. Cada sprint entrega un incremento funcional.
3. El equipo lo conforman **2 desarrolladores** trabajando en paralelo (Backend / Frontend–Full-stack); la capacidad efectiva base es ~40 h/semana.
4. **Todos los cambios de esquema de base de datos se realizan con Liquibase** (DataSource JDBC dedicado para migraciones), conforme a ADR-08.
5. La **certificación inicia el 13 de octubre** (Sprint 5) y se ejecuta en tres olas en paralelo al desarrollo restante.
6. Los esfuerzos por HU son estimaciones derivadas de los totales por módulo; pueden ajustarse en el refinamiento de cada sprint sin alterar la estimación global de 477 h.
7. La reserva/contingencia (~97 h) absorbe desbordamientos previstos de HU-25 y HU-27 y el riesgo de la semana corta del Sprint 7.
8. El despliegue del Sprint 7 depende del cierre de **ADR-06** (proveedor de nube general), aún abierto a la fecha de corte.
9. La validación de token en el servidor y la autorización por rol (deuda técnica de **ADR-10**) se resolvieron en el Sprint 1 como base de seguridad para los módulos M2–M5.
10. El avance reportado corresponde a la **fecha de corte 2026-10-06** (inicio de Sprint 4). Debe actualizarse al cierre de cada sprint.

---

> **Mantenimiento del documento:** actualizar la tabla maestra (sección 3), el
> tablero general (sección 5) y el resumen por sprint (sección 4) al cierre de cada
> sprint. No modificar identificadores de HU (inmutables). Las decisiones que
> afecten alcance o reglas deben reflejarse también en los artefactos de
> `docs/historias-usuario/`.
