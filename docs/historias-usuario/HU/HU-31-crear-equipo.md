# HU-31 — Crear equipo (nombre + Coordinador inicial)

| Campo | Valor |
|---|---|
| Identificador | HU-31 |
| Nombre | Crear equipo (nombre + Coordinador inicial) |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DEC (`DEC-003`, `DEC-014`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-08 (usuarios, para elegir Coordinador) |

> HU nueva en `Borrador`. La entidad `Equipo` y su ciclo de vida provienen de
> `DEC-003`/`DEC-014` (no del documento oficial original). Lo no definido se marca
> `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **crear un equipo definiendo su nombre y
su Coordinador inicial**, para **organizar a los colaboradores y habilitar la
gestión y el seguimiento por equipo**.

## Contexto funcional

El Administrador crea manualmente los equipos (`DEC-014(a)`). Un equipo tiene
exactamente un Coordinador a la vez (`DEC-003(b)`) y agrupa colaboradores (cada
colaborador pertenece a exactamente un equipo, `DEC-003(a)`). Al crear el equipo se
define al menos su nombre y su Coordinador inicial. Los equipos no se eliminan
(`DEC-014(d)`). Módulo M5; actor: Administrador; resultado: equipo creado con
Coordinador. Origen: DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Nombre del equipo | Texto | Sí | Pendiente por definir | Pendiente por definir | — | Sí | Nombre del equipo. |
| Coordinador inicial | Selección (un usuario) | Sí | — | Un Coordinador | Usuarios que puedan ejercer rol Coordinador | Sí | Coordinador vigente del equipo (`DEC-003(b)`). |
| Otros campos del equipo | Pendiente por definir | Pendiente por definir | — | — | — | — | Descripción u otros atributos: MEN-004 (detalle menor). |

---

# Validaciones funcionales

## VF-01. Nombre y Coordinador presentes

**Qué se valida:** que se indique nombre del equipo y un Coordinador inicial válido.
**Cuándo:** al crear.
**Si cumple:** se crea el equipo con su Coordinador.
**Si no cumple:** Pendiente por definir.

## VF-02. Un solo Coordinador

**Qué se valida:** que el equipo tenga exactamente un Coordinador.
**Cuándo:** al crear.
**Si cumple:** registra el Coordinador único.
**Si no cumple:** no aplica (la UI selecciona uno).

---

# Reglas de negocio

## RN-01. Creación manual por el Administrador

El Administrador crea los equipos manualmente.
Origen: DEC (`DEC-014(a)`).

## RN-02. Un Coordinador por equipo

Un equipo tiene exactamente un Coordinador a la vez.
Origen: DEC (`DEC-003(b)`).

## RN-03. Equipo no eliminable

Un equipo no puede eliminarse; solo puede reasignarse su Coordinador (HU-32).
Origen: DEC (`DEC-014(d)`).

---

# Reglas de comportamiento

## RC-01. Disponibilidad para asignación de colaboradores

Tras crearse, el equipo queda disponible para asignarle colaboradores (la
asignación inicial de un colaborador ocurre al crear su usuario, HU-08; la
reasignación posterior, HU-33).

---

# Criterios de aceptación

## CA-01. Creación exitosa

Cuando el Administrador indica un nombre y un Coordinador inicial válido y confirma,
el sistema crea el equipo con ese Coordinador.

## CA-02. Coordinador único

Cuando se crea el equipo, queda registrado con exactamente un Coordinador.

---

# Escenarios de prueba

## CP-01 — Creación de equipo

**Dado que** el Administrador está autenticado
**Cuando** crea un equipo con nombre y Coordinador inicial
**Entonces** el sistema registra el equipo con su Coordinador.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Falta nombre o Coordinador | Pendiente por definir. |
| FA-02 | Creador no Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: creación exclusiva del Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-08 (usuarios existentes, para elegir el Coordinador inicial).

---

# Aclaraciones

- La entidad `Equipo` proviene de `DEC-003`/`DEC-014`, no del documento oficial
  original; es una definición del equipo del proyecto.
- Campos del equipo más allá del nombre (descripción, etc.): MEN-004, detalle menor
  para Fase 6.

---

# Fuera de alcance

- Eliminación de equipos (excluida por `DEC-014(d)`).
- Asignación inicial de colaboradores (ocurre en HU-08); reasignación en HU-33.

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. Autorización por rol en el servidor (`DEC-010`).

## (Menores)

- Campos del equipo más allá del nombre (MEN-004).

---

# Prototipo

**Requerido:** Pendiente.

Descripción: Pendiente por definir.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos. (Atributos extra menores pendientes — MEN-004.)
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`. El
comportamiento funcional está bien definido por `DEC-003`/`DEC-014`.

**Pendientes:** 1 importante (autorización), 1 menor (campos del equipo).
