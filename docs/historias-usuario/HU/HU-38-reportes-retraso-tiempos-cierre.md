# HU-38 — Reportes de retraso y tiempos de cierre de tareas

| Campo | Valor |
|---|---|
| Identificador | HU-38 |
| Nombre | Reportes de retraso y tiempos de cierre de tareas (solo pantalla, bajo demanda) |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.1) + DEC (`DEC-009`, `DEC-027`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-15 (estados de tarea), HU-19 (cierre) |

> HU nueva en `Borrador`. `DEC-027` reúne HU-39 (tiempos de cierre) en esta HU. Lo
> no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **consultar reportes de retraso y de
tiempos de cierre de tareas**, para **identificar cuellos de botella y medir la
eficiencia del equipo**.

## Contexto funcional

El Administrador consulta dos reportes que comparten fuente y patrón (`DEC-027`):
**retraso** de tareas y **tiempos de cierre**. Son de **solo consulta en pantalla**
(sin exportación) y **bajo demanda** (`DEC-009`). El reporte de retraso se apoya en
el estado `Retrasada` (`DEC-012`); el de tiempos de cierre, en el paso a `Cerrada`
(HU-19). Las **fórmulas de cálculo** no están definidas (IMP-021). Módulo M5; actor:
Administrador; resultado: reportes consultables en pantalla. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Reporte de retraso | Vista/tabla (solo lectura) | N/A | Pendiente por definir | — | — | No | Basado en el estado `Retrasada`. Columnas/cálculo: Pendiente por definir (IMP-021). |
| Reporte de tiempos de cierre | Vista/tabla (solo lectura) | N/A | Pendiente por definir | — | — | No | Tiempo hasta el cierre (HU-19). Definición de "tiempo de cierre": Pendiente por definir (IMP-021). |
| Parámetros de consulta | Control | Pendiente por definir | — | — | Pendiente por definir | Sí | ¿Filtros por equipo/período? Pendiente por definir. |

---

# Validaciones funcionales

## VF-01. Acceso exclusivo del Administrador

**Qué se valida:** que solo el Administrador consulte estos reportes.
**Cuándo:** al solicitar el reporte.
**Si cumple:** muestra el reporte en pantalla.
**Si no cumple:** Pendiente por definir; autorización por rol (`DEC-010`).

---

# Reglas de negocio

## RN-01. Solo consulta en pantalla, bajo demanda

Los reportes son de solo consulta en pantalla y se generan bajo demanda.
Origen: DEC (`DEC-009`).

## RN-02. Agrupación retraso + tiempos de cierre

Esta HU cubre los reportes de retraso y de tiempos de cierre, cada uno con sus
columnas y reglas propias.
Origen: DEC (`DEC-027`).

## RN-03. Base de datos de los reportes

El retraso se basa en el estado `Retrasada` (`DEC-012`); el tiempo de cierre, en la
transición a `Cerrada` (HU-19, `DEC-007`).
Origen: DEC (`DEC-012`, `DEC-007`).

---

# Reglas de comportamiento

## RC-01. Generación bajo demanda

El reporte se genera al solicitarlo; no hay generación programada.

---

# Criterios de aceptación

## CA-01. Consulta de reporte de retraso

Cuando el Administrador solicita el reporte de retraso, el sistema lo muestra en
pantalla. **Verificable una vez definida la fórmula/columnas (IMP-021).**

## CA-02. Consulta de reporte de tiempos de cierre

Cuando el Administrador solicita el reporte de tiempos de cierre, el sistema lo
muestra en pantalla. **Verificable una vez definida la fórmula/columnas (IMP-021).**

## CA-03. Solo pantalla, bajo demanda

Cuando el Administrador consulta un reporte, el sistema lo presenta en pantalla, sin
exportación y sin haberlo programado.

---

# Escenarios de prueba

## CP-01 — Consulta de retraso

**Dado que** existen tareas retrasadas
**Cuando** el Administrador solicita el reporte de retraso
**Entonces** el sistema lo muestra en pantalla (sin exportación).

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Fórmulas/columnas no definidas | El contenido del reporte no es construible hasta definir IMP-021. |
| FA-02 | Usuario no Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: reportes exclusivos del Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-15 (estado `Retrasada`), HU-19 (cierre) como fuente de datos.

---

# Aclaraciones

- `DEC-027`: `HU-39` (tiempos de cierre) quedó fusionada en HU-38; su identificador
  no se reutiliza. Reabrió `DEC-023`.
- La definición exacta de "tiempo de cierre" (¿desde creación?, ¿desde asignación?,
  ¿hasta `Completada` o hasta `Cerrada`?) es parte de IMP-021.

---

# Fuera de alcance

- Exportación de reportes (excluida por `DEC-009`).
- Reportes de productividad y cumplimiento (HU-29).
- Reportes configurables desde plantilla (HU-34, diferida).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Fórmulas y columnas de los reportes de retraso y tiempos de cierre, y la
   definición de "tiempo de cierre" (IMP-021).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Pendiente.

Descripción: Pendiente por definir.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Columnas/fórmulas Pendiente por definir — IMP-021.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [ ] Criterios verificables. (CA-01/CA-02 sujetos a IMP-021.)
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Fórmulas IMP-021; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por las fórmulas/columnas (IMP-021) y el RNF de
seguridad `DEC-010`. El formato y periodicidad están definidos por `DEC-009`.

**Pendientes:** 2 importantes.
