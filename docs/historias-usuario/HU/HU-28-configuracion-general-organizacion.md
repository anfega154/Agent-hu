# HU-28 — Configuración general de la organización (zona horaria + notificaciones)

| Campo | Valor |
|---|---|
| Identificador | HU-28 |
| Nombre | Configuración general de la organización (zona horaria global + notificaciones generales) |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Media (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DOC (`compira-context.md` §5.1, §6 M5) + DEC (`DEC-002`, `DEC-011`, `DEC-008`, `DEC-026`) |
| Última actualización | 2026-09-21 |
| Dependencias | — (una organización preexistente por instancia, `DEC-002`) |

> HU nueva en `Borrador`. `DEC-026` reúne HU-35 (notificaciones) en esta HU. Lo no
> definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **configurar la zona horaria global y el
interruptor de notificaciones de la organización**, para **que los vencimientos se
calculen con una referencia horaria única y controlar la entrega de notificaciones**.

## Contexto funcional

COMPIRA es mono-organización por despliegue (`DEC-002`). El Administrador configura
dos parámetros generales de esa única organización: (a) la **zona horaria global**,
que aplica a todos los usuarios y se usa para calcular vencimientos y retrasos
(`DEC-011`); y (b) el **interruptor global de notificaciones** in-app (M3), que
activa o desactiva las notificaciones para todos los usuarios (`DEC-008`). Ambas se
reúnen en una sola HU (`DEC-026`). Módulo M5; actor: Administrador; resultado:
configuración de la organización actualizada. Origen: DOC / DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Zona horaria | Selección | Sí | Zona horaria | — | Zonas horarias válidas (catálogo Pendiente por definir) | Sí | Zona horaria global de la organización (`DEC-011`). |
| Notificaciones activas | Interruptor (booleano) | Sí | Booleano | — | Activado / Desactivado | Sí | Interruptor global de notificaciones in-app (`DEC-008`). |

---

# Validaciones funcionales

## VF-01. Zona horaria válida

**Qué se valida:** que la zona horaria seleccionada sea válida.
**Cuándo:** al guardar la configuración.
**Si cumple:** actualiza la zona horaria global.
**Si no cumple:** Pendiente por definir. El catálogo de zonas válidas: Pendiente por definir.

---

# Reglas de negocio

## RN-01. Mono-organización

Existe una única organización por instancia; esta HU configura esa organización.
Origen: DEC (`DEC-002`).

## RN-02. Zona horaria global

La zona horaria es global a la organización; no hay zona horaria por usuario. Aplica
al cálculo de vencimientos y retrasos.
Origen: DEC (`DEC-011`).

## RN-03. Notificaciones como interruptor global

"Configurar notificaciones" es un interruptor global (activado/desactivado) para
todos los usuarios; no hay configuración por tipo de evento ni por usuario/rol.
Origen: DEC (`DEC-008`).

---

# Reglas de comportamiento

## RC-01. Efecto de la zona horaria

Al cambiar la zona horaria, los cálculos de vencimiento/retraso (HU-24, HU-27) usan
la nueva referencia. El efecto sobre tareas con fecha límite ya fijada es Pendiente
por definir.

## RC-02. Efecto del interruptor de notificaciones

Al desactivar las notificaciones, el sistema deja de entregar notificaciones in-app
(HU-21, HU-24) a todos los usuarios; al activarlas, se reanudan.

---

# Criterios de aceptación

## CA-01. Configuración de zona horaria

Cuando el Administrador selecciona una zona horaria válida y guarda, el sistema la
fija como zona horaria global de la organización.

## CA-02. Interruptor de notificaciones

Cuando el Administrador desactiva las notificaciones y guarda, el sistema deja de
entregar notificaciones in-app a todos los usuarios; al activarlas, se reanudan.

---

# Escenarios de prueba

## CP-01 — Desactivar notificaciones

**Dado que** las notificaciones están activas
**Cuando** el Administrador las desactiva y guarda
**Entonces** el sistema deja de entregar notificaciones in-app a los usuarios.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Zona horaria inválida | Pendiente por definir. |
| FA-02 | Quien configura no es Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Zona horaria: referencia global única para cálculos de vencimiento (RT-05, `DEC-011`). |
| RNF-02 | Seguridad: configuración exclusiva del Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |

---

# Dependencias

- Organización preexistente por instancia (`DEC-002`).
- Efecto sobre M3 (HU-21, HU-24) y M4 (HU-27).

---

# Aclaraciones

- `DEC-026`: `HU-35` (notificaciones generales) quedó fusionada en HU-28; su
  identificador no se reutiliza. Reabrió `DEC-018(c)`.
- El catálogo de zonas horarias válidas y el efecto del cambio de zona sobre
  vencimientos ya fijados no están definidos.

---

# Fuera de alcance

- Configuración de notificaciones por tipo de evento o por usuario (excluida por `DEC-008`).
- Zona horaria por usuario (excluida por `DEC-011`).
- Alta de organización (no aplica por `DEC-002`).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Catálogo de zonas horarias válidas y efecto del cambio de zona sobre tareas con
   fecha límite ya fijada (nuevo IMP-023).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Pendiente.

Descripción: Pendiente por definir.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (Catálogo de zonas Pendiente por definir.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Catálogo de zonas / efecto del cambio; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendiente el
catálogo de zonas y el efecto del cambio (IMP-023). El comportamiento está bien
definido por `DEC-008`/`DEC-011`.

**Pendientes:** 2 importantes.
