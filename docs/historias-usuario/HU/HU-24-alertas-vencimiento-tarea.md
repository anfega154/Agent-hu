# HU-24 — Alertas de vencimiento de tarea (próxima a vencer / retrasada)

| Campo | Valor |
|---|---|
| Identificador | HU-24 |
| Nombre | Alertas de vencimiento de tarea (próxima a vencer / retrasada) |
| Módulo | M3 — Recordatorios y alertas |
| Actor | Sistema COMPIRA → Colaborador y Coordinador (destinatarios) |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §6 M3) + DEC (`DEC-025`, `DEC-012`, `DEC-011`, `DEC-008`) |
| Última actualización | 2026-09-21 |
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
de la organización (`DEC-011`). El escenario "próxima a vencer" depende de un umbral
que **no está definido** (`BLOQ-004` se resolvió solo para "retraso"; el umbral de
"próximo" sigue pendiente). Módulo M3; resultado: alerta in-app entregada al
destinatario según el escenario. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Alerta | Elemento in-app (solo lectura) | N/A | — | — | — | No | Aviso mostrado al destinatario. |
| Escenario | Derivado (no editable) | N/A | Enum | — | Próxima a vencer / Retrasada | No | Determina destinatario y disparo. |
| Umbral "próxima a vencer" | Parámetro | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Pendiente por definir | Cuánto antes del vencimiento se alerta. **Pendiente por definir** (no resuelto). |

---

# Validaciones funcionales

## VF-01. Disparo de alerta de retraso

**Qué se valida:** que una tarea no `Completada`/`Cerrada` haya superado su fecha
límite (zona horaria global, `DEC-011`).
**Cuándo:** de forma inmediata al cumplirse la fecha/hora límite (`DEC-012`).
**Si cumple:** el sistema marca la tarea `Retrasada` (HU-15) y alerta al Coordinador.
**Si no cumple:** no se genera alerta de retraso.

## VF-02. Disparo de alerta de próxima a vencer

**Qué se valida:** que la tarea entre en la ventana "próxima a vencer" según el umbral.
**Cuándo:** al alcanzarse el umbral previo al vencimiento.
**Si cumple:** el sistema alerta al Colaborador responsable.
**Si no cumple:** no se genera alerta.
**Nota:** el umbral está `Pendiente por definir`; sin él, este escenario no es
construible.

---

# Reglas de negocio

## RN-01. Retraso inmediato

Una tarea pasa a `Retrasada` de forma inmediata al cumplirse su fecha/hora límite
(zona horaria global) si no está `Completada` ni `Cerrada`; sin margen de tolerancia.
Origen: DEC (`DEC-012`).

## RN-02. Destinatario del retraso

La alerta de retraso se dirige al Coordinador.
Origen: DOC (§6 M3).

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

Las alertas se entregan/actualizan en tiempo real (RT-04).

---

# Criterios de aceptación

## CA-01. Alerta de retraso al Coordinador

Cuando una tarea no `Completada`/`Cerrada` supera su fecha límite (zona horaria
global) y las notificaciones están activas, el sistema alerta al Coordinador y la
tarea queda `Retrasada` (HU-15).

## CA-02. Alerta de próxima a vencer al Colaborador

Cuando una tarea entra en la ventana "próxima a vencer" (según el umbral definido) y
las notificaciones están activas, el sistema alerta al Colaborador responsable.
**Verificable solo una vez definido el umbral.**

## CA-03. Sin entrega con notificaciones desactivadas

Cuando el interruptor global de notificaciones está desactivado, el sistema no
entrega la alerta.

---

# Escenarios de prueba

## CP-01 — Retraso inmediato

**Dado que** una tarea `En progreso` tiene fecha límite hoy a las 17:00 (zona global)
**Cuando** el reloj alcanza las 17:00 sin que esté `Completada`/`Cerrada`
**Entonces** el sistema la marca `Retrasada` y alerta al Coordinador.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Notificaciones desactivadas (`DEC-008`) | No se entrega la alerta. |
| FA-02 | Umbral de "próxima a vencer" no definido | El escenario CA-02 no es construible hasta definir el umbral (BLOQ nuevo). |
| FA-03 | Tarea ya `Completada`/`Cerrada` al vencer | No se genera alerta de retraso. |

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
- `BLOQ-004` (umbral) se resolvió por `DEC-012` **solo para "retraso"** (inmediato);
  el umbral de "próxima a vencer" sigue `Pendiente por definir`.

---

# Fuera de alcance

- Alertas por canales externos (correo/push).
- Gestión de alertas de desempeño (HU-36, diferida).

---

# Preguntas pendientes

## Bloqueantes

1. **Umbral de "próxima a vencer":** cuánto antes del vencimiento debe alertarse.
   Sin él, el escenario CA-02/VF-02 no es construible (nuevo BLOQ-009).

## Importantes

1. Comportamiento cuando el destinatario no tiene sesión activa (persistencia de la
   alerta) — mismo pendiente que HU-21 (IMP-020).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-24-alertas-vencimiento.svg` — escenario de
retraso definido (`DEC-012`) y escenario "próxima a vencer" bloqueado por el umbral
sin definir (BLOQ-009). El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Umbral "próxima a vencer" Pendiente por definir.)
- [x] Validaciones definidas. (VF-02 no construible sin umbral.)
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [x] Criterios verificables. (CA-02 sujeto al umbral.)
- [x] Dependencias identificadas.
- [ ] Sin preguntas bloqueantes. (Umbral "próxima a vencer" — BLOQ-009.)
- [ ] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** No cumple. Bloqueado por el umbral de "próxima a vencer" (BLOQ-009)
y por el RNF de seguridad `DEC-010`. El escenario de **retraso** sí está
completamente definido por `DEC-012`/`DEC-011`.

**Pendientes:** 1 bloqueante (umbral próxima a vencer), 1 importante (persistencia
de alerta sin sesión).
