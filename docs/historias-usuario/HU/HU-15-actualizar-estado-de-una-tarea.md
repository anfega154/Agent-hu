# HU-15 — Actualizar estado de una tarea

| Campo | Valor |
|---|---|
| Identificador | HU-15 |
| Nombre | Actualizar estado de una tarea |
| Módulo | M2 — Gestión de tareas |
| Actor | Colaborador |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §4, §6) + DEC (`DEC-005`, `DEC-007`, `DEC-012`, `DEC-015`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-13 (tarea con responsable) |

> HU nueva en `Borrador` y la de mayor esfuerzo del módulo: concentra el motor de
> estados. Debe implementarse y probarse como una unidad (no fragmentar). Lo no
> definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Colaborador** necesito **actualizar el estado de la tarea que ejecuto**,
para **reflejar su avance real y alimentar el seguimiento y los indicadores**.

## Contexto funcional

El Colaborador responsable avanza el estado de su tarea dentro del conjunto cerrado
definido en `DEC-005`. El Colaborador puede llevar la tarea hasta `Completada`,
pero **no** hasta `Cerrada` (el cierre requiere aprobación del Coordinador, HU-19,
`DEC-007`). El estado `Retrasada` no lo fija el usuario: se activa automáticamente
al vencer la fecha límite (`DEC-012`). Módulo M2; actor: Colaborador; resultado:
estado de la tarea actualizado y registrado en el histórico (HU-20). Origen: DOC / DEC.

### Conjunto cerrado de estados (`DEC-005`)

`Pendiente` → `En progreso` → (`Retrasada`) → `Completada` → `Cerrada`;
`En reasignación` (transversal, instante de transición, `DEC-015(a)`);
`Cancelada` (terminal, `DEC-015(b)`).

- `Retrasada` reemplaza a `Pendiente`/`En progreso` al superar la fecha límite; es
  un valor del campo `estado`, no una marca aparte (`DEC-005`, `DEC-012`).
- `Completada` y `Cerrada` son estados independientes (`DEC-005`).
- `En reasignación` es un instante de transición: al reasignar, la tarea conserva
  el estado que tenía antes (`DEC-015(a)`; ver HU-17).
- `Cancelada` se alcanza desde cualquier estado excepto `Cerrada` (`DEC-015(b)`;
  ver HU-18).

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Estado | Selección / acción de transición | Sí | Enum | — | `Pendiente`, `En progreso`, `Completada` (por el Colaborador) | Sí (según transición permitida) | El Colaborador avanza el estado hasta `Completada`. No puede fijar `Cerrada` (`DEC-007`) ni `Retrasada` (automático, `DEC-012`). |

> Las transiciones exactas que el Colaborador puede ejecutar manualmente (p. ej.
> `Pendiente`→`En progreso`→`Completada`, y si puede retroceder) están parcialmente
> definidas: `DEC-005`/`DEC-015` fijan el conjunto y algunas reglas; el detalle de
> cada transición manual permitida es Pendiente por definir donde no lo cubran esas
> decisiones.

---

# Validaciones funcionales

## VF-01. Transición permitida

**Qué se valida:** que la transición de estado solicitada sea válida según `DEC-005`/`DEC-015` y el rol.
**Cuándo:** al solicitar el cambio de estado.
**Si cumple:** se aplica el nuevo estado y se registra en el histórico (HU-20).
**Si no cumple:** se rechaza; mensaje Pendiente por definir.

## VF-02. Prohibición de cierre por el Colaborador

**Qué se valida:** que el Colaborador no lleve la tarea a `Cerrada`.
**Cuándo:** al intentar cerrar.
**Si cumple (no intenta cerrar):** continúa.
**Si no cumple:** se rechaza; el cierre corresponde al Coordinador (HU-19).

---

# Reglas de negocio

## RN-01. Conjunto cerrado de estados

El estado de la tarea es un único campo con valores excluyentes del conjunto de 7
estados de `DEC-005`.
Origen: DEC (`DEC-005`).

## RN-02. El Colaborador no cierra

El Colaborador puede llevar la tarea hasta `Completada`, pero el paso a `Cerrada`
requiere aprobación del Coordinador (HU-19).
Origen: DEC (`DEC-007`).

## RN-03. Retrasada automática

`Retrasada` se activa de forma inmediata al cumplirse la fecha/hora límite (según la
zona horaria global, `DEC-011`) si la tarea no está `Completada` ni `Cerrada`; no es
una acción manual del usuario.
Origen: DEC (`DEC-012`).

---

# Reglas de comportamiento

## RC-01. Registro en histórico

Cada cambio de estado se registra en el histórico de la tarea (HU-20).

## RC-02. Reflejo en seguimiento

El nuevo estado se refleja en el panel de seguimiento (HU-25) y en los indicadores
(HU-27) en tiempo real (RT-04).

---

# Criterios de aceptación

## CA-01. Avance de estado válido

Cuando el Colaborador responsable solicita una transición válida (p. ej.
`Pendiente`→`En progreso` o `En progreso`→`Completada`), el sistema aplica el nuevo
estado y lo registra en el histórico.

## CA-02. Bloqueo del cierre por el Colaborador

Cuando el Colaborador intenta llevar la tarea a `Cerrada`, el sistema lo impide; el
cierre solo lo realiza el Coordinador (HU-19).

## CA-03. Transición a Retrasada automática

Cuando una tarea no `Completada`/`Cerrada` supera su fecha límite (zona horaria
global, `DEC-011`), el sistema la marca `Retrasada` sin intervención del usuario.

## CA-04. Transición inválida rechazada

Cuando se solicita una transición no permitida por `DEC-005`/`DEC-015`, el sistema
la rechaza (mensaje Pendiente por definir).

---

# Escenarios de prueba

## CP-01 — Avance a Completada

**Dado que** una tarea está `En progreso` y su responsable es el usuario
**Cuando** el Colaborador la marca `Completada`
**Entonces** el sistema aplica `Completada` y lo registra en el histórico.

## CP-02 — Colaborador no puede cerrar

**Dado que** una tarea está `Completada`
**Cuando** el Colaborador intenta marcarla `Cerrada`
**Entonces** el sistema lo impide (el cierre es del Coordinador, HU-19).

## CP-03 — Retraso automático

**Dado que** una tarea `En progreso` supera su fecha límite
**Cuando** se cumple la fecha/hora límite (zona horaria global)
**Entonces** el sistema la marca `Retrasada` automáticamente.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Transición no permitida | Rechazo; mensaje Pendiente por definir. |
| FA-02 | Intento de cierre por Colaborador | Rechazo; corresponde al Coordinador (HU-19). |
| FA-03 | Actualización por quien no es el responsable | Pendiente por definir; ligado a autorización (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Trazabilidad: cada cambio de estado se registra en el histórico (RT-03, HU-20). |
| RNF-02 | Tiempo real: el cambio se refleja en el panel sin recarga (RT-04). |
| RNF-03 | Seguridad: solo el responsable/rol autorizado puede cambiar el estado (validación en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-13 (tarea con responsable).
- HU-19 (cierre por el Coordinador), HU-17 (reasignación), HU-18 (cancelación),
  HU-20 (histórico), HU-24/HU-27 (efecto de `Retrasada`).

---

# Aclaraciones

- El motor de estados se documenta como una única HU por acoplamiento de reglas
  (`DEC-005`, `DEC-007`, `DEC-012`, `DEC-015`); dividirlo fragmentaría una capacidad
  que debe probarse junta.
- La transición hacia/desde `En reasignación` la produce HU-17; hacia `Cancelada`,
  HU-18; hacia `Cerrada`, HU-19. Aquí se documenta la parte operada por el Colaborador.

---

# Fuera de alcance

- Cierre de la tarea (HU-19).
- Reasignación (HU-17) y cancelación (HU-18) — disparan transiciones documentadas
  en sus propias HU.

---

# Preguntas pendientes

## Bloqueantes

Ninguna (el conjunto de estados y las reglas clave están definidos por decisiones).

## Importantes

1. Detalle de las transiciones manuales permitidas al Colaborador y si puede
   retroceder de estado (más allá de lo fijado por `DEC-005`/`DEC-015`) (IMP nuevo).
2. Mensajería de transición inválida (MEN-001).
3. Autorización por rol/responsable en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-15-actualizar-estado.svg` — incluye el diagrama
del conjunto cerrado de 7 estados (`DEC-005`) y las transiciones del Colaborador;
detalle de transiciones manuales marcado como Pendiente (IMP-019). El diseño visual
final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (Detalle de transiciones manuales parcialmente pendiente.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Detalle de transiciones; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendientes
importantes de detalle de transiciones y mensajería. El núcleo de estados sí está
definido por `DEC-005`/`DEC-007`/`DEC-012`/`DEC-015`.

**Pendientes:** 3 importantes.
