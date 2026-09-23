# HU-32 — Asignar / reasignar coordinador de un equipo

| Campo | Valor |
|---|---|
| Identificador | HU-32 |
| Nombre | Asignar / reasignar coordinador de un equipo |
| Módulo | M5 — Administración |
| Actor | Administrador del sistema |
| Estado | Borrador |
| Prioridad | Alta (RECOMENDACIÓN — requiere aprobación) |
| Versión | 1.0 |
| Fuente principal | DEC (`DEC-003`, `DEC-014`) |
| Última actualización | 2026-09-21 |
| Dependencias | HU-31 (equipo creado), HU-41 (listar/seleccionar equipo) |

> HU nueva en `Borrador`. La reasignación del Coordinador es acción exclusiva del
> Administrador (`DEC-003(c)`). Lo no definido se marca `Pendiente por definir`.

---

## Resumen ágil

Como **Administrador del sistema** necesito **asignar o reasignar el coordinador de
un equipo**, para **mantener un responsable de coordinación vigente cuando cambian
las circunstancias**.

## Contexto funcional

Un equipo tiene exactamente un Coordinador a la vez (`DEC-003(b)`). El Coordinador
puede reasignarse, y esa reasignación es exclusiva del Administrador (`DEC-003(c)`).
El Administrador selecciona un equipo (HU-41) y designa un nuevo Coordinador.
Módulo M5; actor: Administrador; resultado: equipo con nuevo Coordinador vigente.
Origen: DEC.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| Equipo | Selección | Sí | — | Un equipo | Equipos existentes (HU-41) | Sí | Equipo cuyo Coordinador se cambia. |
| Nuevo Coordinador | Selección (un usuario) | Sí | — | Un Coordinador | Usuarios que puedan ejercer rol Coordinador | Sí | Reemplaza al Coordinador vigente (uno solo, `DEC-003(b)`). |

---

# Validaciones funcionales

## VF-01. Nuevo Coordinador válido

**Qué se valida:** que el nuevo Coordinador sea un usuario válido para ese rol.
**Cuándo:** al reasignar.
**Si cumple:** el equipo queda con el nuevo Coordinador.
**Si no cumple:** Pendiente por definir.

---

# Reglas de negocio

## RN-01. Un Coordinador por equipo

Tras la reasignación, el equipo sigue teniendo exactamente un Coordinador.
Origen: DEC (`DEC-003(b)`).

## RN-02. Reasignación exclusiva del Administrador

La reasignación del Coordinador de un equipo es acción exclusiva del Administrador.
Origen: DEC (`DEC-003(c)`, `DEC-014(d)`).

---

# Reglas de comportamiento

## RC-01. Efecto en el alcance del panel

Al cambiar el Coordinador, el alcance de datos del panel (HU-25/HU-27) pasa a
reflejar al nuevo Coordinador para ese equipo (`DEC-006`).

---

# Criterios de aceptación

## CA-01. Reasignación exitosa

Cuando el Administrador selecciona un equipo y un nuevo Coordinador válido y
confirma, el sistema reemplaza al Coordinador vigente por el nuevo.

## CA-02. Un solo Coordinador

Cuando se reasigna, el equipo queda con exactamente un Coordinador (el nuevo).

---

# Escenarios de prueba

## CP-01 — Reasignación de coordinador

**Dado que** un equipo tiene al Coordinador A
**Cuando** el Administrador lo reasigna al Coordinador B
**Entonces** el equipo queda con B como único Coordinador vigente.

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | Nuevo Coordinador no válido | Pendiente por definir. |
| FA-02 | Quien reasigna no es Administrador | Pendiente por definir; autorización por rol (`DEC-010`). |

---

# Requisitos no funcionales

| ID | Requisito |
|---|---|
| RNF-01 | Seguridad: acción exclusiva del Administrador (validación de token/rol en servidor pendiente — `DEC-010`). |

---

# Dependencias

- HU-31 (equipo creado), HU-41 (listar/seleccionar equipo).

---

# Aclaraciones

- Si el usuario designado como Coordinador debe tener ya el rol Coordinador o el
  sistema se lo asigna (relación con `DEC-004` multi-rol y HU-10) es Pendiente por
  definir.

---

# Fuera de alcance

- Reasignación de colaboradores entre equipos (HU-33).
- Eliminación de equipos (excluida por `DEC-014(d)`).

---

# Preguntas pendientes

## Bloqueantes

Ninguna.

## Importantes

1. ¿El nuevo Coordinador debe tener ya el rol Coordinador (HU-10/`DEC-004`) o se le
   asigna al reasignarlo? (relación con IMP-015).
2. Autorización por rol en el servidor (`DEC-010`).

---

# Prototipo

**Requerido:** Sí (wireframe placeholder).

Wireframe funcional (baja fidelidad):
`docs/historias-usuario/prototipos/HU-32-reasignar-coordinador.svg` — reasignación
exclusiva del Administrador (`DEC-003c`); relación con el rol Coordinador marcada
como Pendiente (IMP-015). El diseño visual final es decisión de UX del equipo.

---

# Definition of Ready

- [x] Actor definido.
- [x] Objetivo definido.
- [x] Campos definidos.
- [x] Validaciones definidas.
- [x] Reglas de negocio definidas.
- [x] Flujo principal definido.
- [ ] Errores relevantes definidos. (Mensajería Pendiente por definir.)
- [x] Criterios verificables.
- [x] Dependencias identificadas.
- [x] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos. (Rol previo del Coordinador; autorización `DEC-010`.)

## Resultado

**Estado DoR:** No cumple. Bloqueado por el RNF de seguridad `DEC-010`; pendiente la
relación con el rol Coordinador (IMP-015).

**Pendientes:** 2 importantes.
