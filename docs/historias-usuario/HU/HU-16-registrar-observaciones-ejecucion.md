# HU-16 — Registrar observaciones de ejecución de una tarea

| Campo | Valor |
|---|---|
| Identificador | HU-16 |
| Nombre | Registrar observaciones de ejecución de una tarea |
| Módulo | M2 — Gestión de tareas |
| Actor | Colaborador |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.3, §6) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-13 (tarea asignada al Colaborador) |

> HU nueva en `Borrador`. Lo no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Colaborador** necesito **registrar observaciones y documentar la ejecución
de mi tarea**, para **dejar constancia del trabajo realizado y aportar a la
trazabilidad**.

## Contexto funcional

El Colaborador responsable registra observaciones sobre la ejecución de la tarea
(el documento oficial menciona "registrar observaciones" y "documentar la ejecución
de la tarea"). Módulo M2; actor: Colaborador; resultado: observación registrada y
asociada a la tarea (y a su histórico, HU-20). Origen: DOC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Observación | Texto | Pendiente por definir | Pendiente por definir | Pendiente por definir | — | Pendiente por definir | Nota de ejecución sobre la tarea. |
| Adjunto(s) | Pendiente por definir | Pendiente por definir | Pendiente por definir | Pendiente por definir | Pendiente por definir | Pendiente por definir | ¿Se admiten adjuntos para "documentar la ejecución"? No definido. |

> No se inventa si "documentar la ejecución" implica solo texto o también
> adjuntos, ni límites de tamaño/cantidad/tipos (ver `PENDIENTES.md`).

---

# Validaciones funcionales

## VF-01. Contenido de la observación

**Qué se valida:** que la observación cumpla las reglas de contenido definidas.
**Cuándo:** al registrar.
**Si cumple:** se guarda la observación.
**Si no cumple:** Pendiente por definir.

> Las reglas de contenido (obligatoriedad, longitud, adjuntos) están Pendiente por
> definir.

---

# Reglas de negocio

## RN-01. Autor de la observación

Las observaciones de ejecución las registra el Colaborador responsable de la tarea.
Origen: DOC (§5.3).

> Si el Coordinador u otros roles pueden registrar observaciones es Pendiente por
> definir.

---

# Reglas de comportamiento

## RC-01. Registro en histórico

Las observaciones se asocian a la tarea y quedan disponibles en su histórico (HU-20).
Detalle Pendiente por definir.

---

# Criterios de aceptación

## CA-01. Registro de observación

Cuando el Colaborador responsable registra una observación válida, el sistema la
guarda asociada a la tarea. Reglas de contenido: Pendiente por definir.

---

# Escenarios de prueba

## CP-01 — Registro exitoso

**Dado que** el Colaborador es responsable de una tarea
**Cuando** registra una observación válida
**Entonces** el sistema la guarda asociada a la tarea.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Observación inválida (reglas de contenido) | Pendiente por definir. |
| FA-02 | Registro por quien no es responsable | Pendiente por definir; autorización (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Trazabilidad: las observaciones aportan al histórico de la tarea (RT-03). |
| RNF-02 | Seguridad: autorización por responsable/rol (validación en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-13 (tarea asignada).
- HU-20 (histórico).

---

# Aclaraciones

- El documento oficial no precisa si "documentar la ejecución" admite adjuntos ni
  sus límites; se deja Pendiente por definir sin inventar.

---

# Fuera de alcance

- Adjuntos, si finalmente se decide que no forman parte (Pendiente por definir).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Reglas de contenido de la observación: ¿solo texto o admite adjuntos? Límites de
   tamaño/cantidad/tipos y obligatoriedad (MEN / IMP de detalle de observaciones).
2. ¿Pueden otros roles (Coordinador) registrar observaciones?

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-16-registrar-observaciones.svg` — reglas de
contenido y adjuntos marcados como Pendiente por definir. El diseño visual final es
decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [ ] Campos definidos. (Contenido/adjuntos Pendiente por definir.)
- [ ] Validaciones definidas. (Dependen de las reglas de contenido.)
- [x] Reglas de negocio definidas. (Autor definido; alcance a otros roles pendiente.)
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos.
- [x] Criterios verificables. (Parcial.)
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** No cumple. Faltan las reglas de contenido de la observación y el
RNF de seguridad `DEC-010`.

**Pendientes:** 2 importantes.
