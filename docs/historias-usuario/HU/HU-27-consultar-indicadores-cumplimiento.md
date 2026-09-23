# HU-27 — Consultar indicadores de cumplimiento y tareas vencidas/próximas/retrasadas

| Campo | Valor |
|---|---|
| Identificador | HU-27 |
| Nombre | Consultar indicadores de cumplimiento y tareas vencidas/próximas/retrasadas (incluye carga de trabajo por responsable) |
| Módulo | M4 — Panel de seguimiento |
| Actor | Coordinador / Administrador |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §6 M4) + DEC (`DEC-006`, `DEC-020`, `DEC-012`, `DEC-011`, `DEC-028`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-15 (estados de tarea) |

> HU nueva en `Borrador`. `DEC-020`/`DEC-028` incorporan aquí el indicador "carga de
> trabajo por responsable" (sin reporte separado en M5). Lo no definido se marca
> `Pendiente por definir`.

---

## Resumen ágil

Como **Coordinador o Administrador** necesito **consultar indicadores de
cumplimiento y las tareas vencidas, próximas a vencer y retrasadas, además de la
carga de trabajo por responsable**, para **medir el desempeño del equipo y tomar
decisiones de seguimiento**.

## Contexto funcional

Sobre las tareas del alcance del panel (HU-25), el sistema calcula y muestra
indicadores: niveles de cumplimiento, tareas vencidas, próximas a vencer y
retrasadas, y la carga de trabajo por responsable (cantidad de tareas asignadas por
Colaborador, `DEC-020`). El alcance de datos por rol es análogo al del panel
(`DEC-006`): Coordinador = su equipo; Administrador = toda la organización. El
estado `Retrasada` proviene de `DEC-012`/`DEC-011`. Las **fórmulas de cálculo** de
los indicadores de cumplimiento **no están definidas**, y el **umbral de "próxima a
vencer"** tampoco (BLOQ-009). Módulo M4; resultado: indicadores consultables en
tiempo real. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Nivel de cumplimiento | Indicador (solo lectura) | N/A | Pendiente por definir (¿porcentaje?) | — | — | No | Fórmula de cálculo: Pendiente por definir. |
| Tareas vencidas / retrasadas | Indicador/conteo (solo lectura) | N/A | Conteo | — | — | No | Basado en el estado `Retrasada` (`DEC-012`). |
| Tareas próximas a vencer | Indicador/conteo (solo lectura) | N/A | Conteo | — | — | No | Depende del umbral "próxima a vencer" (**BLOQ-009**, no definido). |
| Carga de trabajo por responsable | Indicador/conteo (solo lectura) | N/A | Conteo por Colaborador | — | — | No | Cantidad de tareas asignadas por Colaborador (`DEC-020`). |

> Las fórmulas exactas (numerador/denominador, período, si excluye canceladas,
> etc.) están `Pendiente por definir`.

---

# Validaciones funcionales

## VF-01. Alcance de datos por rol

**Qué se valida:** que los indicadores se calculen solo sobre las tareas dentro del
alcance del rol (Coordinador: su equipo; Administrador: toda la organización).
**Cuándo:** al calcular/mostrar los indicadores.
**Si cumple:** muestra indicadores del alcance permitido.
**Si no cumple:** no incluye datos fuera del alcance.

---

# Reglas de negocio

## RN-01. Alcance de datos por rol

Los indicadores (incluida la carga de trabajo) se calculan con alcance por rol,
análogo a `DEC-006`: Coordinador = su equipo; Administrador = todos los equipos.
Origen: DEC (`DEC-020`, `DEC-006`).

## RN-02. Carga de trabajo en el panel, no como reporte separado

La carga de trabajo por responsable se cubre como indicador del panel (M4); no se
crea un reporte separado en M5.
Origen: DEC (`DEC-020`, `DEC-028`).

## RN-03. Retrasada como estado

Las tareas retrasadas se cuentan con base en el estado `Retrasada` (`DEC-012`), no
como un cálculo aparte.
Origen: DEC (`DEC-012`, `DEC-005`).

---

# Reglas de comportamiento

## RC-01. Actualización en tiempo real

Los indicadores se actualizan en tiempo real conforme cambian las tareas (RT-04).

---

# Criterios de aceptación

## CA-01. Indicadores según alcance

Cuando un Coordinador consulta los indicadores, el sistema los calcula sobre las
tareas de su equipo; cuando lo hace un Administrador, sobre todas las de la
organización.

## CA-02. Conteo de retrasadas

Cuando existen tareas en estado `Retrasada` dentro del alcance, el sistema las
refleja en el indicador correspondiente.

## CA-03. Carga de trabajo por responsable

Cuando el usuario consulta el panel, el sistema muestra la cantidad de tareas
asignadas por Colaborador dentro de su alcance.

## CA-04. Cumplimiento (sujeto a fórmula)

Cuando se defina la fórmula de cumplimiento, el sistema mostrará el nivel de
cumplimiento del alcance. **Verificable solo una vez definida la fórmula.**

## CA-05. Próximas a vencer (sujeto a umbral)

Cuando se defina el umbral de "próxima a vencer" (BLOQ-009), el sistema mostrará el
conteo de tareas próximas a vencer. **Verificable solo una vez definido el umbral.**

---

# Escenarios de prueba

## CP-01 — Carga de trabajo por responsable

**Dado que** un Coordinador tiene un equipo con tareas asignadas
**Cuando** consulta los indicadores
**Entonces** el sistema muestra el conteo de tareas por cada Colaborador de su equipo.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Fórmula de cumplimiento no definida | El indicador de cumplimiento no es construible hasta definir la fórmula. |
| FA-02 | Umbral "próxima a vencer" no definido | El indicador de próximas a vencer no es construible (BLOQ-009). |
| FA-03 | Sin tareas en el alcance | Indicadores en cero / estado vacío (detalle Pendiente por definir). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Tiempo real: los indicadores se actualizan sin recarga completa (RT-04). |
| RNF-02 | Zona horaria: los cálculos de vencimiento usan la referencia global (RT-05, `DEC-011`). |
| RNF-03 | Seguridad: alcance de datos por rol garantizado en el servidor (validación pendiente — `DEC-010`). |

---

# Dependencias

- HU-15 (estados de tarea, incluido `Retrasada`).
- HU-25 (panel base y alcance de datos).

---

# Aclaraciones

- Dependencia ajustada por `DEC-027`: HU-27 depende de los datos de tareas (HU-15),
  no de la vista del panel (HU-25) para su cálculo, aunque se presenta junto al panel.
- `DEC-028` resolvió `IMP-014`: la carga de trabajo vive aquí, no como reporte
  separado de M5.

---

# Fuera de alcance

- Reporte separado de carga de trabajo en M5 (descartado por `DEC-028`).
- Reportes bajo demanda del Administrador (HU-29, HU-38, M5).

---

# Preguntas pendientes

## Bloqueantes

1. **Umbral de "próxima a vencer"** (BLOQ-009): compartido con HU-24; sin él, el
   indicador de próximas a vencer no es construible.
2. **Fórmulas de los indicadores de cumplimiento** (y tiempos asociados): no
   definidas (IMP-021, ex resumen de análisis).

## Importantes

1. Definición exacta de cada indicador (período, exclusiones como canceladas, etc.).

---

# Prototipo

**Requerido:** Pendiente.

Descripción: Pendiente por definir.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Fórmulas y umbral Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [x] Errores relevantes definidos.
- [ ] Criterios verificables. (CA-04 y CA-05 sujetos a fórmula/umbral.)
- [x] Dependencias identificadas.
- [ ] Sin preguntas bloqueantes. (Umbral BLOQ-009; fórmulas IMP-021.)
- [ ] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** No cumple. Bloqueado por el umbral de "próxima a vencer" (BLOQ-009),
las fórmulas de indicadores (IMP-021) y el RNF de seguridad `DEC-010`. La carga de
trabajo y el conteo de retrasadas sí están definidos.

**Pendientes:** 1 bloqueante (umbral), 1 bloqueante/importante (fórmulas, IMP-021),
1 importante (definición de indicadores).
