# PENDIENTES — COMPIRA

> Registro de **lo que todavía no sabemos** (vacíos, ambigüedades y
> contradicciones sin resolver). Distinto de `DECISIONES.md` (lo ya definido).
>
> Reglas:
> - No se inventan respuestas. Cada pendiente queda como pregunta abierta.
> - Al resolverse un pendiente que constituya una decisión funcional relevante,
>   se registra en `DECISIONES.md` (DEC-XXX) y se actualizan HU, inventario,
>   matriz y glosario en el mismo paso.
> - Nomenclatura alineada con `DECISIONES.md`: `BLOQ-xxx` (bloqueante),
>   `IMP-xxx` (importante), `MEN-xxx` (menor).
>
> | Campo | Valor |
> |---|---|
> | Versión | 3.0 |
> | Última actualización | 2026-09-21 |
>
> Cambios v3.0: alineado con la consolidación aprobada (DEC-024..DEC-028). Se
> cierra `IMP-014` (→ `DEC-028`); se ajusta `IMP-001` (reset absorbido en HU-10);
> se mantienen abiertos `IMP-012` y `IMP-013`. Se conserva la nomenclatura de
> `DECISIONES.md`.

---

## Bloqueantes

Impiden desarrollar correctamente las HU asociadas. Ninguna HU asociada puede
pasar a `Lista para desarrollo` hasta resolverse.

| ID | Pendiente | Módulo / HU | Estado |
|---|---|---|---|
| BLOQ-008 | **Deuda de seguridad `DEC-010`:** el backend no valida el token Bearer ni aplica autorización por rol/equipo en el servidor. Es RNF bloqueante para llevar cualquier HU de M2–M5 a `Lista para desarrollo`. | Transversal M2–M5 | Abierto |
| BLOQ-009 | **Umbral de "próxima a vencer" (HU-24):** cuánto antes del vencimiento debe alertarse al Colaborador. `DEC-012` resolvió solo el "retraso" (inmediato); el umbral de "próximo" no está definido. Sin él, el escenario CA-02/VF-02 de HU-24 no es construible. | M3/M4 / HU-24, HU-27 | Abierto |

> Nota: los bloqueantes originales de análisis de dominio (BLOQ-001, BLOQ-002,
> BLOQ-004..BLOQ-007) fueron **resueltos** por DEC-001..DEC-016 (ver
> `DECISIONES.md`). BLOQ-003 resuelto (ubicación de HU-10/HU-11 en M5).

---

## Importantes

No bloquean el inicio, pero deben resolverse para completar las HU.

| ID | Pendiente | Módulo / HU | Estado |
|---|---|---|---|
| IMP-001 | Alcance exacto del **restablecimiento de contraseña por el administrador**, ahora absorbido como acción dentro de HU-10 (`DEC-024`): ¿dispara flujo de Cognito (correo/OTP) o fija contraseña temporal como en HU-08? El detalle define el escenario de aceptación dentro de HU-10. | M5 / HU-10 | Abierto (reasignado a HU-10) |
| IMP-002 | Condición exacta bajo la cual una tarea `Completada` requiere aprobación para pasar a `Cerrada`. `DEC-007` la hace obligatoria siempre; queda el detalle de qué valida el Coordinador al aprobar. | M2 / HU-19 | Abierto |
| IMP-012 | ⚠️ **Discrepancia HU-09:** ¿la funcionalidad de eliminar usuarios existe realmente en el sistema construido? Si existe y la HU se descartó (`DEC-019`), queda sin respaldo documental. ¿Se retira del producto o se conserva? Debe resolverse antes del cierre. | M1/M5 / HU-09 | Abierto |
| IMP-013 | Mecanismo exacto de **"gestionar alertas de desempeño"** (HU-36) y **módulo** (M3 o M4): ¿configurar umbrales?, ¿marcar como atendida?, ¿escalar? Sin esto, HU-36 no es construible; se mantiene diferida. | M3/M4 / HU-36 | Abierto |
| IMP-015 | **Reflejo del multi-rol (`DEC-004`) en autenticación y registro:** ¿cómo se expresa el conjunto de roles de un usuario en el resultado del login (rol activo, selección, todos) y en el formulario de registro de HU-08 (hoy una sola lista desplegable)? Detectado al reconstruir HU-02 y HU-08. | M1 / HU-02, HU-08 | Abierto |
| IMP-016 | **Extensión de Equipo en HU-08 (`DEC-014`):** control de UI, obligatoriedad por rol y comportamiento de error al capturar el Equipo del Colaborador al registrarlo. Depende de HU-31 (crear equipo) y HU-41 (listar equipos). Bloquea el DoR de la extensión de HU-08. | M1/M5 / HU-08 | Abierto |
| IMP-017 | **Campos y reglas de la tarea (HU-12):** conjunto exacto de campos obligatorios, formatos y reglas de fecha (¿fecha de inicio y/o límite?, formato, ¿fecha límite no anterior a hoy?). El documento oficial dice "título, descripción, fechas" sin precisar. Bloquea el DoR de HU-12. | M2 / HU-12 | Abierto |
| IMP-018 | **Alcance de candidatos a responsable (HU-13, HU-17):** ¿cualquier Colaborador de la organización o solo los del equipo del Coordinador? Relacionado con `DEC-003` (Equipo). | M2 / HU-13, HU-17 | Abierto |
| IMP-019 | **Detalle de transiciones de estado operadas por el Colaborador (HU-15):** transiciones manuales exactas permitidas (¿retroceso de estado?) más allá del conjunto y reglas ya fijados por `DEC-005`/`DEC-015`; y mensajería de transición inválida. | M2 / HU-15 | Abierto |
| IMP-020 | **Persistencia de notificaciones/alertas sin sesión activa (HU-21, HU-24):** ¿la notificación in-app se acumula/persiste hasta el próximo ingreso del destinatario o solo se muestra en tiempo real si está conectado? | M3 / HU-21, HU-24 | Abierto |
| IMP-021 | **Fórmulas de los indicadores de cumplimiento (HU-27) y de los reportes (HU-29, HU-38):** definición exacta de cada indicador/reporte (numerador/denominador, período, exclusiones como tareas canceladas, definición de "tiempo de cierre"). Sin ellas, los indicadores de HU-27 y el contenido de HU-29/HU-38 no son verificables. | M4/M5 / HU-27, HU-29, HU-38 | Abierto |
| IMP-022 | **Efecto de reasignar un colaborador a otro equipo (HU-33) sobre sus tareas activas:** ¿las tareas ya asignadas se mantienen con él?, ¿cambian de alcance de seguimiento al nuevo equipo/Coordinador? | M5 / HU-33 | Abierto |
| IMP-023 | **Configuración de zona horaria (HU-28):** catálogo de zonas horarias válidas y efecto del cambio de zona sobre tareas con fecha límite ya fijada. | M5 / HU-28 | Abierto |

---

## Menores

Pueden definirse posteriormente sin cambiar sustancialmente el comportamiento.

| ID | Pendiente | Módulo / HU | Estado |
|---|---|---|---|
| MEN-001 | Mensajería al usuario y **códigos de error** fuera de M1 (M2–M5 no tienen catálogo como `AUTH_00x`). | M2–M5 | Abierto |
| MEN-002 | **Prioridad** de HU-10 en adelante: recomendación del agente, no decisión formal; pendiente de aprobación del Product Owner. | Todas las nuevas | Abierto |
| MEN-003 | Contenido/columnas exactas de cada reporte (HU-29, HU-38): detalle para Fase 6, no bloqueante (`DEC-009`, `DEC-027`). | M5 | Abierto |
| MEN-004 | Campos exactos de la entidad `Equipo` más allá del nombre (descripción, etc.). | M5 / HU-31 | Abierto |
| MEN-005 | Detalles de identidad heredados del PDF de HU: política exacta de contraseñas de Cognito (INC-05, UI 10 vs backend 8), vigencia de OTP y del código de recuperación, bloqueo por intentos, expiración/refresco de token, respuesta genérica en recuperación (INC-06), reutilización de contraseñas, persistencia del contador de reenvío. A confirmar durante la reconstrucción documental (`DEC-017`) de HU-01..HU-08; donde falte evidencia, se mantiene pendiente. | M1 / HU-01..HU-08 | Abierto |

---

## Pendientes resueltos recientemente

| ID | Resolución | Decisión |
|---|---|---|
| IMP-014 | "Carga de trabajo por responsable" queda cubierta como indicador en el panel M4 (HU-27); **no** se crea un reporte separado en M5 (no se usa HU-40 para ello). | `DEC-028` |
| BLOQ-003 | HU-08/HU-09 confirmadas en M1; HU-10/HU-11 en M5. | (resuelto en `DECISIONES.md`) |
| IMP-011 | Mecanismo de reportes configurables (plantillas predefinidas, no constructor libre). HU-34 diferida por alcance. | `DEC-016`, `DEC-027` |

---

## Contradicciones registradas (referencia a 00-analisis-funcional §9)

| INC | Descripción breve | Estado |
|---|---|---|
| INC-01 | JWT propio (ADR-05) vs identidad delegada en Cognito (ADR-10). | Resuelto por prioridad (Cognito). |
| INC-02 | Carga de trabajo en M4 vs M5. | Resuelto (`DEC-020`, `DEC-028`): vive en HU-27. |
| INC-03 | RBAC de diseño vs no validado en servidor. | Abierto → BLOQ-008 (`DEC-010`). |
| INC-04 | ADR-06 (proveedor de nube) abierto vs AWS fijado por Cognito. | Infraestructura; fuera del alcance funcional. |
| INC-05 | Contraseña mínima UI (10) vs backend (8). | Abierto → MEN-005. |
| INC-06 | Recuperación puede revelar existencia de la cuenta. | Abierto → MEN-005. |

---

## Resumen

- Bloqueantes: 2 (BLOQ-008 deuda de seguridad `DEC-010`; BLOQ-009 umbral "próxima a vencer", afecta HU-24 y HU-27).
- Importantes: 13 (IMP-001, IMP-002, IMP-012, IMP-013, IMP-015 a IMP-023).
- Menores: 5 (MEN-001 a MEN-005).
- Discrepancia crítica a resolver antes del cierre: IMP-012 (HU-09 descartada vs
  backlog oficial).

> Mientras BLOQ-008 siga abierto, ninguna HU de M2–M5 puede declararse
> `Lista para desarrollo`. HU-34 y HU-36 permanecen diferidas (alcance /
> `IMP-013`). La resolución de cada pendiente sigue el flujo
> `PENDIENTE → DECISIÓN → actualización de HU/inventario/matriz → revalidación DoR`.
