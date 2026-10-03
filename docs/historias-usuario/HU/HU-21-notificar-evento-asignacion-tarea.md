# HU-21 — Notificar evento de asignación de tarea (nueva asignación / reasignación)

| Campo | Valor |
|---|---|
| Identificador | HU-21 |
| Nombre | Notificar evento de asignación de tarea (nueva asignación / reasignación) |
| Módulo | M3 — Recordatorios y alertas |
| Actor | Sistema COMPIRA → Colaborador (destinatario) |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.1 |
| Fuente principal | DOC (`compira-context.md` §6 M3) + DEC (`DEC-018(a)`, `DEC-008`) |
| Última actualización | 2026-09-26 |
| Dependencias | HU-13 (asignación), HU-17 (reasignación), RT-04 |

> HU nueva en `Borrador`. `DEC-018(a)` fusiona HU-22 (notificar reasignación) en
> esta HU: un solo mecanismo de notificación in-app disparado por dos eventos. Lo
> no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Colaborador** necesito **recibir una notificación cuando se me asigna o
reasigna una tarea**, para **enterarme oportunamente del trabajo del que soy
responsable**.

## Contexto funcional

El sistema genera una notificación dentro de la plataforma (in-app, en tiempo real)
cuando ocurre un evento de asignación: (a) se asigna una tarea a un Colaborador
(HU-13), o (b) se reasigna una tarea a un nuevo responsable (HU-17). Es el mismo
mecanismo de notificación con dos disparadores (`DEC-018(a)`). El alcance de M3 son
notificaciones dentro de la plataforma; canales externos (correo/push) están fuera
de alcance / pendientes. Módulo M3; actor emisor: Sistema; destinatario: el
Colaborador responsable; resultado: notificación in-app entregada. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Notificación | Elemento in-app (solo lectura) | N/A | — | — | — | No | Aviso mostrado al Colaborador destinatario. |
| Evento origen | Derivado (no editable) | N/A | Enum | — | Nueva asignación (HU-13) / Reasignación (HU-17) | No | Determina el texto/tipo de la notificación. |
| Texto del mensaje | Texto | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | No | Contenido exacto del mensaje: Pendiente por definir (MEN-001). |

---

# Validaciones funcionales

## VF-01. Disparo por evento de asignación

**Qué se valida:** que exista un evento de asignación (HU-13) o reasignación (HU-17)
para el destinatario.
**Cuándo:** al ocurrir el evento.
**Si cumple:** el sistema genera la notificación in-app al Colaborador responsable.
**Si no cumple:** no se genera notificación.

> El disparo queda condicionado al interruptor global de notificaciones (`DEC-008`,
> HU-35 → fusionada en HU-28): si las notificaciones están desactivadas para la
> organización, no se entregan.

---

# Reglas de negocio

## RN-01. Un mecanismo, dos eventos

La notificación de nueva asignación y la de reasignación son el mismo mecanismo
in-app, disparado por dos eventos distintos.
Origen: DEC (`DEC-018(a)`).

## RN-02. Sujeta al interruptor global de notificaciones

La entrega de notificaciones depende de la configuración general de la organización
(interruptor global, `DEC-008`, gestionado en HU-28).
Origen: DEC (`DEC-008`).

## RN-03. Alcance in-app

Las notificaciones de M3 son dentro de la plataforma: entrega en tiempo real para
usuarios conectados y persistencia para consulta al próximo ingreso si están
desconectados. No se requiere correo ni push.
Origen: DOC (§6 M3), DEC (`DEC-029`).

---

# Reglas de comportamiento

## RC-01. Entrega en tiempo real

La notificación se entrega/actualiza en tiempo real, sin recarga completa de página
(RT-04). Los avisos persisten también sin sesión activa (`DEC-029`).

---

# Criterios de aceptación

## CA-01. Notificación por nueva asignación

Cuando el Coordinador asigna una tarea a un Colaborador (HU-13) y las notificaciones
están activas, el sistema entrega al Colaborador una notificación in-app del evento.

## CA-02. Notificación por reasignación

Cuando una tarea se reasigna a un nuevo responsable (HU-17) y las notificaciones
están activas, el sistema entrega al nuevo responsable una notificación in-app del
evento.

## CA-04. Destinatario desconectado

Con notificaciones activas, el aviso se guarda aunque el destinatario no tenga
sesión y está disponible al volver a ingresar, sin duplicar el evento.

## CA-03. Sin entrega con notificaciones desactivadas

Cuando el interruptor global de notificaciones está desactivado (`DEC-008`), el
sistema no entrega la notificación.

---

# Escenarios de prueba

## CP-01 — Notificación por asignación

**Dado que** las notificaciones están activas
**Cuando** el Coordinador asigna una tarea a un Colaborador
**Entonces** el Colaborador recibe una notificación in-app en tiempo real.

## CP-02 — Notificación por reasignación

**Dado que** las notificaciones están activas
**Cuando** una tarea se reasigna a un nuevo responsable
**Entonces** el nuevo responsable recibe una notificación in-app.

---

## CP-03 — Persistencia sin sesión

**Dado que** el destinatario está desconectado y las notificaciones están activas
**Cuando** se asigna o reasigna una tarea a ese destinatario
**Entonces** el aviso queda guardado y se muestra al volver a ingresar.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Notificaciones desactivadas (`DEC-008`) | No se entrega la notificación. |
| FA-02 | Destinatario sin sesión activa en ese momento | Se persiste y se muestra al próximo ingreso (`DEC-029`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Tiempo real: entrega/actualización sin recarga completa (RT-04). |

---

# Dependencias

- HU-13 (asignación) y HU-17 (reasignación) como disparadores.
- HU-28 (interruptor global de notificaciones, `DEC-008`).

---

# Aclaraciones

- `DEC-018(a)`: `HU-22` (notificar reasignación) quedó fusionada en HU-21; su
  identificador no se reutiliza.
- El texto exacto de los mensajes y el catálogo de mensajería de M3 no están
  definidos (MEN-001).

---

# Fuera de alcance

- Notificaciones por canales externos (correo/push): fuera del alcance in-app de M3
  (Pendiente/fuera de alcance).
- Alertas de vencimiento/retraso (HU-24) y de desempeño (HU-36).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Texto de los mensajes de notificación (MEN-001).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-21-notificar-asignacion.svg` — notificación
in-app por asignación/reasignación; persistencia sin sesión aprobada (`DEC-029`). El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Texto del mensaje Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos. (Persistencia sin sesión aprobada.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Aceptación de seguridad `DEC-010` pendiente.)

## Resultado

**Estado DoR:** Pendiente de revalidación humana. `IMP-020` resuelto por `DEC-029`.
Seguridad y configuración implementadas como prerrequisitos mínimos (`DEC-030`),
sin declarar aceptado el RNF transversal `DEC-010`. Texto de mensajes: MEN-001.

**Pendientes:** aceptación de seguridad y catálogo de mensajes.
