# 02 — Matriz de cobertura

> Documento de la **Fase 4 (Matriz de cobertura)**. Relaciona
> **Objetivo → Módulo → Capacidad → Funcionalidad → HU** para verificar que toda
> funcionalidad de alcance quede asociada al menos a una HU, y que toda HU
> justifique qué parte del alcance cubre.
>
> Alineada con `01-inventario-hu.md` v4.0 (consolidación aprobada, DEC-024..DEC-028)
> y con `DECISIONES.md`. Objetivo raíz: **OE3 — Desarrollar los módulos centrales**.
>
> | Campo | Valor |
> |---|---|
> | Versión | 3.0 |
> | Última actualización | 2026-09-21 |
> | Estado | Propuesta consolidada — pendiente de aprobación |
>
> Cambios v3.0: numeración consolidada; se reflejan fusiones (HU-11→10, 23→24,
> 35→28, 37→29, 39→38), HU faltantes nuevas (HU-40, HU-41) y HU diferidas
> (HU-34, HU-36).

---

## Matriz principal

Columna **Conformidad doc.**: `OK-v1` = HU generada conforme a la plantilla en
Fase 6 (HU-01..HU-08); `RE` = pendiente de reconstrucción; `N/A` = HU nueva aún
por elaborar según plantilla.

| Objetivo | Módulo | Capacidad | Funcionalidad | HU | Conformidad doc. |
|---|---|---|---|---|---|
| OE3 | M1 | Autenticación | Inicio de sesión correo + contraseña | HU-02 | OK-v1 |
| OE3 | M1 | Autenticación | Verificación en dos pasos (OTP correo) | HU-03 | OK-v1 |
| OE3 | M1 | Autenticación | Reenvío de OTP | HU-04 | OK-v1 |
| OE3 | M1 | Activación de cuenta | Cambio obligatorio de contraseña primer ingreso | HU-01 | OK-v1 |
| OE3 | M1 | Recuperación de contraseña | Solicitud de recuperación por correo | HU-05 | OK-v1 |
| OE3 | M1 | Recuperación de contraseña | Confirmación de restablecimiento con código | HU-06 | OK-v1 |
| OE3 | M1 | Control de sesión | Cierre de sesión y protección de rutas | HU-07 | OK-v1 |
| OE3 | M1 | RBAC (efectivo en servidor) | Validación de token/rol en servidor | ⚠ Sin HU (deuda técnica ADR-10; `DEC-010`) | — |
| OE3 | M5 | Gestión de usuarios | Registrar usuario (con Equipo, `DEC-014`) | HU-08 | OK-v1.1 (extensión Equipo pendiente) |
| OE3 | M5 | Gestión de usuarios | Consultar / listar usuarios | HU-40 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M5 | Gestión de usuarios | Editar usuario, su rol y restablecer contraseña | HU-10 | OK-v1 (Borrador; DoR No cumple, IMP-001) |
| OE3 | M5 | Gestión de usuarios | Eliminar usuario | ⚠ HU-09 Descartada (`DEC-019`; hueco `IMP-012`) | — |
| OE3 | M2 | Creación de tarea | Crear tarea (título, descripción, fechas) | HU-12 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Asignación | Asignar a un responsable | HU-13 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Consulta de tareas | Consultar tareas asignadas | HU-14 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Actualización de estado | Actualizar estado (7 estados) | HU-15 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Observaciones / documentación | Registrar observaciones | HU-16 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Reasignación | Reasignar (con histórico) | HU-17 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Cancelación | Cancelar tarea | HU-18 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Aprobación / cierre | Aprobar / cerrar tarea | HU-19 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M2 | Historial / trazabilidad | Consultar historial de la tarea | HU-20 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M3 | Notificación de asignación/reasignación | Notificar evento de asignación | HU-21 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M3 | Alertas de vencimiento | Próxima a vencer / retrasada | HU-24 | OK-v1 (Borrador; DoR No cumple, BLOQ-009) |
| OE3 | M4 | Vista consolidada + filtros | Panel de seguimiento con filtros | HU-25 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M4 | Indicadores + carga de trabajo | Cumplimiento, vencidas/próximas/retrasadas, carga por responsable | HU-27 | OK-v1 (Borrador; DoR No cumple, BLOQ-009, IMP-021) |
| OE3 | M5 | Configuración de la organización | Zona horaria global + notificaciones generales | HU-28 | OK-v1 (Borrador; DoR No cumple, IMP-023) |
| OE3 | M5 | Reportes | Productividad y cumplimiento | HU-29 | OK-v1 (Borrador; DoR No cumple, IMP-021) |
| OE3 | M5 | Reportes | Retraso y tiempos de cierre | HU-38 | OK-v1 (Borrador; DoR No cumple, IMP-021) |
| OE3 | M5 | Gestión de equipos | Crear equipo | HU-31 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M5 | Gestión de equipos | Asignar / reasignar coordinador | HU-32 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M5 | Gestión de equipos | Reasignar colaborador a otro equipo | HU-33 | OK-v1 (Borrador; DoR No cumple, IMP-022) |
| OE3 | M5 | Gestión de equipos | Consultar / listar equipos y miembros | HU-41 | OK-v1 (Borrador; DoR No cumple) |
| OE3 | M5 | Reportes configurables (ampliación) | Configurar/guardar reporte desde plantilla | HU-34 (diferida) | N/A |
| OE3 | M3/M4 | Alertas de desempeño | Gestionar alertas de desempeño | HU-36 (diferida, `IMP-013`) | N/A |

---

## Verificación de cobertura por módulo

| Módulo | Capacidades | Cubiertas por HU | Vacíos / observaciones |
|---|---|---|---|
| M1 | Autenticación, sesión, recuperación, activación, RBAC | HU-01..HU-07 | RBAC efectivo en servidor: deuda técnica (`DEC-010`). HU-01..HU-08 en reconstrucción documental (RE). |
| M2 | Ciclo de vida de la tarea | HU-12..HU-20 | HU-15 es la de mayor esfuerzo (motor de 7 estados). |
| M3 | Notificaciones y alertas | HU-21, HU-24 | HU-22 y HU-23 fusionadas. HU-36 (desempeño) diferida. |
| M4 | Panel e indicadores | HU-25, HU-27 | HU-26 y HU-30 fusionadas. Carga de trabajo cubierta en HU-27 (`DEC-028`). |
| M5 | Usuarios, equipos, organización, reportes | HU-08, HU-10, HU-40, HU-41, HU-28, HU-29, HU-38, HU-31, HU-32, HU-33 | HU-11/HU-35/HU-37/HU-39 fusionadas; HU-34 diferida; eliminación de usuario descartada (hueco `IMP-012`). |

**Conclusión de cobertura:** toda capacidad de alcance queda asociada al menos a
una HU, salvo: (a) RBAC efectivo en servidor = deuda técnica (`DEC-010`); (b)
eliminación de usuario = descartada (`DEC-019`), hueco funcional consciente a
resolver en `IMP-012`. Se cerraron dos huecos detectados en la consolidación con
HU-40 (listar usuarios) y HU-41 (listar equipos). No hay HU huérfanas.

---

## Trazabilidad de reglas transversales → HU

| RT | Aplica a (HU) |
|---|---|
| RT-01 Autenticación e identidad | HU-01, HU-02, HU-03, HU-04, HU-05, HU-06, HU-08, HU-10 |
| RT-02 Autorización por rol (RBAC) | HU-07, HU-08, HU-10, HU-40, HU-41 (y transversal a todas; deuda `DEC-010`) |
| RT-03 Trazabilidad / historial | HU-17, HU-20, HU-29, HU-38 |
| RT-04 Actualización en tiempo real | HU-21, HU-24, HU-25, HU-27 |
| RT-05 Zona horaria | HU-24, HU-27, HU-28 |
| RT-06 Convenciones contraseña/OTP y errores | HU-01, HU-02, HU-03, HU-04, HU-06, HU-08, HU-10 |
| RT-07 Acceso responsivo por navegador | Transversal a todas |

---

## Trazabilidad de consolidación (fusiones y diferimientos)

| Cambio | HU origen → destino | Decisión |
|---|---|---|
| Restablecer contraseña admin absorbido en editar usuario | HU-11 → HU-10 | `DEC-024` |
| Alertas de vencimiento unificadas | HU-23 → HU-24 | `DEC-025` |
| Configuración de organización unificada | HU-35 → HU-28 (reabre `DEC-018(c)`) | `DEC-026` |
| Reportes reunidos | HU-37 → HU-29; HU-39 → HU-38 (reabre `DEC-023`) | `DEC-027` |
| Reporte de carga de trabajo no se crea | `IMP-014` resuelto (cubierto por HU-27) | `DEC-028` |
| HU faltantes añadidas | HU-40 (listar usuarios), HU-41 (listar equipos) | `DEC-028` |
| Diferidas por alcance/indefinición | HU-34, HU-36 | `DEC-027` (HU-34), `DEC-021`/`IMP-013` (HU-36) |

> Las RT son propuestas; su aprobación formal corresponde al equipo. No se
> inventan valores no documentados.
