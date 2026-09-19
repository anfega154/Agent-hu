# Glosario funcional — COMPIRA

Registra el **lenguaje ubicuo** del dominio. Objetivo: que todas las HU usen los
mismos términos para los mismos conceptos.

Reglas:

- No crear sinónimos para conceptos existentes sin justificación (p. ej. no
  alternar entre "Colaborador", "Operador", "Empleado" o "Usuario operativo"
  para el mismo actor).
- Registrar la fuente de cada término: `DOC / HU / DEC / RT / REC`.
- Si un término no está claramente definido en las fuentes, marcarlo como
  `Pendiente por definir` en su definición, sin inventar.

---

| Término | Definición | Fuente |
|---|---|---|
| COMPIRA | Plataforma web para centralizar la información y la gestión de tareas diarias en pymes. | DOC |
| Administrador del sistema | Actor responsable de la configuración general de la organización; único con permiso para gestionar cuentas, roles y configuraciones críticas. | DOC |
| Coordinador de equipo | Actor operativo (líder de proyecto) que crea, asigna, reasigna, cancela y da seguimiento a las tareas de su equipo. No gestiona usuarios ni configuración general. | DOC |
| Colaborador | Actor (operador) que ejecuta las tareas asignadas, actualiza su estado y documenta la ejecución. No crea tareas ni modifica configuración general. | DOC |
| Tarea | Unidad central de trabajo con título, descripción, fechas, responsables, estado, historial y trazabilidad, gestionada a lo largo de su ciclo de vida. | DOC |
| Estado de tarea | Situación de una tarea durante su ejecución (p. ej. pendiente, en progreso, completada). Valores exactos: Pendiente por definir. | DOC |
| Reasignación | Cambio del o los responsables de una tarea. | DOC |
| Recordatorio / Notificación | Aviso dentro de la plataforma ante eventos de tarea (asignación, reasignación, próximo vencimiento, retraso). | DOC |
| Panel de seguimiento | Vista consolidada del estado, avance, vencimientos y cumplimiento de las tareas del equipo. | DOC |
| Rol | Conjunto de permisos que determina qué puede ver y hacer un actor (gobernanza por roles / RBAC). | DOC |
| Módulo | Agrupación funcional del sistema (M1 Autenticación y roles, M2 Gestión de tareas, M3 Recordatorios, M4 Seguimiento, M5 Administración). | DOC |

<!-- Añadir nuevos términos del dominio aquí, con su fuente. No duplicar conceptos con sinónimos. -->
