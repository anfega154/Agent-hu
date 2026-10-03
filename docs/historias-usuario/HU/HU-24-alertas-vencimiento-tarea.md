# HU-24 — Alertas de vencimiento de tarea (próxima a vencer / retrasada)

| Campo | Valor |
|---|---|
| Identificador | HU-24 |
| Nombre | Alertas de vencimiento de tarea (próxima a vencer / retrasada) |
| Módulo | M3 — Recordatorios y alertas |
| Actor | Sistema COMPIRA → Colaborador y Coordinador (destinatarios) |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.1 |
| Fuente principal | DOC (`compira-context.md` §6 M3) + DEC (`DEC-025`, `DEC-012`, `DEC-011`, `DEC-008`) |
| Última actualización | 2026-09-26 |
| Dependencias | HU-12 (tarea con fecha límite), HU-15 (estado), RT-04, RT-05 |

> HU nueva en `Borrador`. `DEC-025` fusiona HU-23 (próxima a vencer) en esta HU:
> un mecanismo de alerta temporal con dos escenarios. Lo no definido se marca
> `Pendiente por definir`.

---

## Resumen ágil

Como **Colaborador (y Coordinador para el retraso)** necesito **recibir alertas
cuando una tarea está próxima a vencer o se ha retrasado**, para **actuar a tiempo
y supervisar el cumplimiento**.

## Contexto funcional

El sistema genera alertas in-app relacionadas con el vencimiento de una tarea, en
dos escenarios: (a) **próxima a vencer**, dirigida al Colaborador responsable; y
(b) **retrasada**, que alerta al Coordinador (documento oficial: "una tarea está
retrasada → alerta al coordinador"). El estado `Retrasada` se activa de forma
inmediata al cumplirse la fecha/hora límite (`DEC-012`) según la zona horaria global
de la organización (`DEC-011`). El aviso de "próxima a vencer" se genera **24 horas antes**
del vencimiento (`DEC-029`). El retraso se comunica al coordinador actual del equipo
de la tarea; no al creador por defecto. Las tareas canceladas quedan excluidas. Módulo M3; resultado: alerta in-app entregada al
destinatario según el escenario. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Alerta | Elemento in-app (solo lectura) | N/A | — | — | — | No | Aviso mostrado al destinatario. |
| Escenario | Derivado (no editable) | N/A | Enum | — | Próxima a vencer / Retrasada | No | Determina destinatario y disparo. |
| Umbral "próxima a vencer" | Regla fija | Sí | Horas | 24 | 24 horas antes | No | Avisar al entrar en las 24 horas previas al vencimiento (`DEC-029`). |

---

# Validaciones funcionales

## VF-01. Disparo de alerta de retraso

**Qué se valida:** que una tarea no `Completada`/`Cerrada`/`Cancelada` haya alcanzado su fecha
límite (zona horaria global, `DEC-011`).
**Cuándo:** de forma inmediata al cumplirse la fecha/hora límite (`DEC-012`).
**Si cumple:** el sistema marca la tarea `Retrasada` (HU-15) y alerta al Coordinador.
**Si no cumple:** no se genera alerta de retraso.

## VF-02. Disparo de alerta de próxima a vencer

**Qué se valida:** que la tarea entre en la ventana "próxima a vencer" de 24 horas previas al vencimiento.
**Cuándo:** al alcanzarse el umbral previo al vencimiento.
**Si cumple:** el sistema alerta al Colaborador responsable.
**Si no cumple:** no se genera alerta.
**Exclusiones:** no aplica a tareas `Completada`, `Cerrada` o `Cancelada`.

---

# Reglas de negocio

## RN-01. Retraso inmediato

Una tarea pasa a `Retrasada` de forma inmediata al cumplirse su fecha/hora límite
(zona horaria global) si no está `Completada`, `Cerrada` ni `Cancelada`; sin margen de tolerancia.
Origen: DEC (`DEC-012`).

## RN-02. Destinatario del retraso

La alerta de retraso se dirige al **coordinador actual del equipo de la tarea**,
no necesariamente al coordinador que la creó.
Origen: DOC (§6 M3), DEC (`DEC-029`).

## RN-03. Referencia horaria única

Los vencimientos se calculan con la zona horaria global de la organización.
Origen: DEC (`DEC-011`).

## RN-04. Sujeta al interruptor global de notificaciones

La entrega de alertas depende del interruptor global de notificaciones (`DEC-008`,
HU-28).
Origen: DEC (`DEC-008`).

---

# Reglas de comportamiento

## RC-01. Coherencia con el estado Retrasada

La alerta de retraso es consistente con la transición de estado a `Retrasada`
gestionada en HU-15 (mismo disparador temporal).

## RC-02. Entrega en tiempo real

Las alertas se entregan/actualizan en tiempo real (RT-04) y persisten para el
próximo ingreso del destinatario desconectado (`DEC-029`).

---

# Criterios de aceptación

## CA-01. Alerta de retraso al Coordinador

Cuando una tarea no `Completada`/`Cerrada`/`Cancelada` alcanza su fecha límite (zona horaria
global) y las notificaciones están activas, el sistema alerta al Coordinador y la
tarea queda `Retrasada` (HU-15).

## CA-02. Alerta de próxima a vencer al Colaborador

Cuando una tarea entra en la ventana "próxima a vencer" (24 horas antes de su fecha límite) y
las notificaciones están activas, el sistema alerta al Colaborador responsable.
Se excluyen tareas `Completada`, `Cerrada` y `Cancelada`.

## CA-03. Sin entrega con notificaciones desactivadas

Cuando el interruptor global de notificaciones está desactivado, el sistema no
entrega la alerta.

---

# Escenarios de prueba

## CP-01 — Retraso inmediato

**Dado que** una tarea `En progreso` tiene fecha límite hoy a las 17:00 (zona global)
**Cuando** el reloj alcanza las 17:00 sin que esté `Completada`/`Cerrada`/`Cancelada`
**Entonces** el sistema la marca `Retrasada` y alerta al Coordinador.

---

## CP-02 — Aviso 24 horas antes

**Dado que** una tarea activa vence mañana a las 17:00 y los avisos están activos
**Cuando** el reloj alcanza hoy las 17:00
**Entonces** se guarda un aviso de próxima a vencer para el responsable.

## CP-03 — Coordinador vigente, cancelación y desconexión

**Dado que** el equipo cambió de coordinador y el nuevo está desconectado
**Cuando** vence una tarea activa del equipo
**Entonces** el aviso persiste para el coordinador actual y aparece al ingresar.
Una tarea cancelada no genera aviso ni pasa a `Retrasada`.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Notificaciones desactivadas (`DEC-008`) | No se entrega la alerta. |
| FA-02 | Destinatario sin sesión activa | La alerta persiste y está disponible al próximo ingreso. |
| FA-03 | Tarea ya `Completada`/`Cerrada`/`Cancelada` al vencer | No se generan alertas ni transición automática a Retrasada. |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Tiempo real: entrega/actualización sin recarga completa (RT-04). |
| RNF-02 | Zona horaria: cálculo de vencimientos con la referencia global (RT-05, `DEC-011`). |

---

# Dependencias

- HU-12 (tarea con fecha límite), HU-15 (estado `Retrasada`).
- HU-28 (interruptor global de notificaciones, `DEC-008`).

---

# Aclaraciones

- `DEC-025`: `HU-23` (alertar próxima a vencer) quedó fusionada en HU-24; su
  identificador no se reutiliza.
- `BLOQ-009` e `IMP-020` resueltos por `DEC-029`: 24 horas y entrega persistente/en vivo.
- `DEC-030`: incluir seguridad, configuración y relación tarea-equipo como
  prerrequisitos mínimos; las tareas existentes requieren asociación explícita.

---

# Fuera de alcance

- Alertas por canales externos (correo/push).
- Gestión de alertas de desempeño (HU-36, diferida).

---

# Preguntas pendientes

## Bloqueantes

Ninguna pregunta funcional de M3; pendiente aceptación transversal de `DEC-010`.

## Importantes

Ninguna sobre persistencia: resuelto por `DEC-029`.

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-24-alertas-vencimiento.svg` — escenario de
retraso inmediato (`DEC-012`) y aviso 24 horas antes (`DEC-029`). El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (Umbral fijo de 24 horas.)
- [x] Validaciones definidas. (VF-02: 24 horas.)
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [x] Criterios verificables. (CA-02: 24 horas.)
- [x] Dependencias identificadas.
- [x] Sin preguntas funcionales bloqueantes de M3. (`DEC-029`.)
- [ ] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** Pendiente de revalidación humana. Umbral y persistencia resueltos
por `DEC-029`; prerrequisitos implementados según `DEC-030`. No se declara aceptado
el RNF transversal `DEC-010`.

**Pendientes:** aceptación de seguridad y configuración de la organización.
