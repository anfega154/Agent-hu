# HU-25 — Consultar panel de seguimiento (incluye filtros por usuario/estado/fecha)

| Campo | Valor |
|---|---|
| Identificador | HU-25 |
| Nombre | Consultar panel de seguimiento (incluye filtros por usuario/estado/fecha) |
| Módulo | M4 — Panel de seguimiento |
| Actor | Coordinador / Administrador |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §6 M4) + DEC (`DEC-006`, `DEC-018(b)`, `DEC-002`, `DEC-003`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-13 (tareas asignadas), HU-15 (estados), RT-04 |

> HU nueva en `Borrador`. `DEC-018(b)` fusiona HU-26 (filtros) y `DEC-006` fusiona
> HU-30 (panel del Administrador) en esta HU. Lo no definido se marca `Pendiente
> por definir`.

---

## Resumen ágil

Como **Coordinador o Administrador** necesito **consultar un panel consolidado del
estado y avance de las tareas, con filtros por usuario, estado y fecha**, para
**dar seguimiento en tiempo real al trabajo del equipo**.

## Contexto funcional

El panel de seguimiento ofrece una vista consolidada de las tareas del equipo:
estado, avance, tareas vencidas y próximas a vencer, y niveles de cumplimiento
(estos últimos como indicadores se detallan en HU-27). Es la **misma vista** para
Coordinador y Administrador; solo cambia el **alcance de datos** (`DEC-006`):
- Administrador: todos los equipos de la organización (`DEC-002`).
- Coordinador: únicamente el/los equipo(s) donde es Coordinador vigente (`DEC-003`).

Los filtros por usuario/colaborador, estado y fecha forman parte de esta HU
(`DEC-018(b)`). El panel se actualiza en tiempo real sin recarga completa (RT-04).
Módulo M4; resultado: vista consolidada y filtrable de tareas según el alcance del
rol. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Listado de tareas del alcance | Tabla/listado (solo lectura) | N/A | — | — | Tareas dentro del alcance del rol | No | Vista consolidada. Columnas exactas: Pendiente por definir. |
| Filtro por usuario/colaborador | Control de filtro | No | — | — | Colaboradores del alcance | Sí | Filtra las tareas por responsable. |
| Filtro por estado | Control de filtro | No | Enum | — | Estados de `DEC-005` (Pendiente, En progreso, Retrasada, En reasignación, Completada, Cerrada, Cancelada) | Sí | Filtra por estado de tarea. |
| Filtro por fecha | Control de filtro | No | Fecha | — | Pendiente por definir (¿fecha límite?, ¿rango?) | Sí | Filtra por fecha. Qué fecha y si es rango: Pendiente por definir. |

> Columnas, orden, paginación y estados vacíos/carga del panel: Pendiente por
> definir (MEN-001 / IMP de detalle de listado).

---

# Validaciones funcionales

## VF-01. Alcance de datos por rol

**Qué se valida:** que el usuario solo vea tareas dentro de su alcance
(Coordinador: su equipo; Administrador: toda la organización).
**Cuándo:** al cargar el panel y al aplicar filtros.
**Si cumple:** muestra únicamente las tareas permitidas.
**Si no cumple:** no muestra tareas fuera del alcance.

## VF-02. Filtros válidos

**Qué se valida:** que los valores de filtro sean válidos (usuario del alcance,
estado del conjunto de `DEC-005`, fecha con formato válido).
**Cuándo:** al aplicar un filtro.
**Si cumple:** aplica el filtro sobre el listado.
**Si no cumple:** Pendiente por definir.

---

# Reglas de negocio

## RN-01. Misma vista, alcance por rol

El panel es la misma vista/funcionalidad para Coordinador y Administrador; solo
cambia el alcance de datos. No hay capacidades exclusivas del Administrador en esta
pantalla.
Origen: DEC (`DEC-006`).

## RN-02. Alcance del Coordinador

El Coordinador ve únicamente el/los equipo(s) donde es Coordinador vigente.
Origen: DEC (`DEC-003`, `DEC-006`).

## RN-03. Alcance del Administrador

El Administrador ve todos los equipos de la organización (mono-organización).
Origen: DEC (`DEC-002`, `DEC-006`).

---

# Reglas de comportamiento

## RC-01. Actualización en tiempo real

El panel se actualiza en tiempo real, sin recarga completa de página (RT-04).

## RC-02. Filtros combinables

Los filtros por usuario, estado y fecha pueden aplicarse (y su combinación).
Detalle de combinación: Pendiente por definir.

---

# Criterios de aceptación

## CA-01. Vista consolidada según alcance

Cuando un Coordinador abre el panel, el sistema muestra las tareas de su(s)
equipo(s); cuando lo abre un Administrador, muestra las de todos los equipos de la
organización.

## CA-02. Filtro por usuario, estado y fecha

Cuando el usuario aplica un filtro por usuario, estado o fecha, el sistema muestra
solo las tareas que cumplen el filtro, dentro de su alcance.

## CA-03. Actualización en tiempo real

Cuando cambia el estado de una tarea del alcance (HU-15), el panel refleja el cambio
sin requerir recarga completa.

---

# Escenarios de prueba

## CP-01 — Alcance del Coordinador

**Dado que** un Coordinador tiene un equipo asignado
**Cuando** abre el panel
**Entonces** el sistema muestra solo las tareas de su equipo.

## CP-02 — Filtro por estado

**Dado que** el panel está cargado
**Cuando** el usuario filtra por estado `Retrasada`
**Entonces** el sistema muestra solo las tareas retrasadas de su alcance.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Sin tareas en el alcance | Estado vacío (detalle Pendiente por definir). |
| FA-02 | Consulta fuera del alcance por rol | No muestra tareas fuera del alcance. |
| FA-03 | Valor de filtro inválido | Pendiente por definir. |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Tiempo real: el panel se actualiza sin recarga completa (RT-04). |
| RNF-02 | Seguridad: el alcance de datos por rol/equipo debe garantizarse en el servidor (validación de token/rol pendiente — `DEC-010`). |

---

# Dependencias

- HU-13 (tareas asignadas), HU-15 (estados de tarea).
- `DEC-003` (entidad Equipo) para el alcance del Coordinador.

---

# Aclaraciones

- `DEC-018(b)`: `HU-26` (filtros) quedó fusionada en HU-25; `DEC-006`: `HU-30`
  (panel del Administrador) también. Sus identificadores no se reutilizan.
- Los indicadores de cumplimiento y las agregaciones (vencidas/próximas/retrasadas,
  carga de trabajo) se documentan en HU-27, no aquí.

---

# Fuera de alcance

- Indicadores calculados y carga de trabajo (HU-27).
- Consulta de la vista del Colaborador sobre sus propias tareas (HU-14, otro actor).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Columnas, orden, paginación, estados vacíos/carga del panel y qué "fecha" usa el
   filtro (¿fecha límite?, ¿rango?) (MEN-001 / detalle de listado).
2. Autorización/alcance por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-25-panel-seguimiento.svg` — vista con filtros
(usuario/estado/fecha) y alcance por rol (`DEC-006`); columnas y paginación marcadas
como Pendiente (MEN-001). El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Columnas y "fecha" del filtro Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Estado vacío / filtro inválido pendientes.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Detalle de listado; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendiente el
detalle de columnas/filtros. El alcance por rol está definido por `DEC-006`.

**Pendientes:** 2 importantes.
