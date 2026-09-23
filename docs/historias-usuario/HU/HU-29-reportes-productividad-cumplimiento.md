# HU-29 — Reportes de productividad y cumplimiento de tareas

| Campo | Valor |
|---|---|
| Identificador | HU-29 |
| Nombre | Reportes de productividad y cumplimiento de tareas (solo pantalla, bajo demanda) |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.1) + DEC (`DEC-009`, `DEC-027`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-15 (estados de tarea), HU-19 (cierre) |

> HU nueva en `Borrador`. `DEC-027` reúne HU-37 (cumplimiento) en esta HU. Lo no
> definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **consultar reportes de productividad y
de cumplimiento de tareas**, para **evaluar el desempeño del equipo y tomar
decisiones de gestión**.

## Contexto funcional

El Administrador consulta reportes de desempeño sobre las tareas. Esta HU agrupa dos
reportes que comparten fuente de datos y patrón de presentación: **productividad** y
**cumplimiento** (`DEC-027`). Son de **solo consulta en pantalla** (sin exportación)
y **bajo demanda** (sin periodicidad programada), según `DEC-009`. Módulo M5; actor:
Administrador; resultado: reportes consultables en pantalla. Las **fórmulas de
cálculo** de cada reporte no están definidas (IMP-021). Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Reporte de productividad | Vista/tabla (solo lectura) | N/A | Pendiente por definir | — | — | No | Columnas y cálculo: Pendiente por definir (IMP-021). |
| Reporte de cumplimiento | Vista/tabla (solo lectura) | N/A | Pendiente por definir | — | — | No | Columnas y cálculo: Pendiente por definir (IMP-021). |
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

Los reportes son de solo consulta en pantalla (sin exportación a PDF/Excel) y se
generan bajo demanda (sin periodicidad programada).
Origen: DEC (`DEC-009`).

## RN-02. Agrupación productividad + cumplimiento

Esta HU cubre los reportes de productividad y de cumplimiento, cada uno con sus
columnas y reglas propias, presentados como una capacidad.
Origen: DEC (`DEC-027`).

---

# Reglas de comportamiento

## RC-01. Generación bajo demanda

El reporte se genera al solicitarlo; no hay generación programada.

---

# Criterios de aceptación

## CA-01. Consulta de reporte de productividad

Cuando el Administrador solicita el reporte de productividad, el sistema lo muestra
en pantalla. **Verificable una vez definida la fórmula/columnas (IMP-021).**

## CA-02. Consulta de reporte de cumplimiento

Cuando el Administrador solicita el reporte de cumplimiento, el sistema lo muestra en
pantalla. **Verificable una vez definida la fórmula/columnas (IMP-021).**

## CA-03. Solo pantalla, bajo demanda

Cuando el Administrador consulta un reporte, el sistema lo presenta en pantalla, sin
opción de exportación y sin haberlo programado.

---

# Escenarios de prueba

## CP-01 — Consulta bajo demanda

**Dado que** el Administrador está autenticado
**Cuando** solicita el reporte de productividad
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

- HU-15 (estados de tarea), HU-19 (cierre) como fuente de datos.

---

# Aclaraciones

- `DEC-027`: `HU-37` (cumplimiento) quedó fusionada en HU-29; su identificador no se
  reutiliza. Reabrió `DEC-023`.
- Las fórmulas de cálculo de productividad y cumplimiento no están definidas
  (IMP-021), compartidas con los indicadores de HU-27.

---

# Fuera de alcance

- Exportación de reportes (excluida por `DEC-009`).
- Reportes configurables desde plantilla (HU-34, diferida).
- Reportes de retraso y tiempos de cierre (HU-38).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Fórmulas y columnas de los reportes de productividad y cumplimiento (IMP-021).
2. Parámetros de consulta (¿filtro por equipo/período?).
3. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-29-reportes-productividad-cumplimiento.svg` —
solo pantalla, bajo demanda (`DEC-009`); columnas y fórmulas marcadas como Pendiente
(IMP-021). El diseño visual final es decisión de UX del equipo.

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

**Pendientes:** 3 importantes.
