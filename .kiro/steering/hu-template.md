# Plantilla oficial de Historia de Usuario — COMPIRA

Toda Historia de Usuario debe utilizar esta estructura.

---

# HU-XXX — [Nombre de la historia]

| Campo | Valor |
|---|---|
| Identificador de la historia | HU-XXX |
| Nombre de la historia | [Nombre funcional] |
| Módulo | [Módulo] |
| Actor | [Actor] |
| Estado | Borrador / Con pendientes / Lista para validación / Aprobada |
| Dependencias | [HU relacionadas] |

---

## Descripción funcional

Como **[actor]** necesito **[necesidad]**, para **[beneficio / objetivo funcional]**.

Complementar con una descripción funcional cuando sea necesario para evitar ambigüedad.

---

# Campos

| Campo | Tipo / Control | Obligatorio | Formato | Longitud / Rango | Valores permitidos | Editable | Descripción / Comportamiento |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

Si algún atributo no está definido utilizar:

**Pendiente por definir**

Nunca inventar restricciones.

---

# Validaciones funcionales

## VF-01. [Nombre]

**Qué se valida:**  
[Descripción]

**Cuándo:**  
[Momento]

**Si cumple:**  
[Comportamiento]

**Si no cumple:**  
[Comportamiento]

---

# Reglas de negocio

## RN-01. [Nombre]

[Regla]

---

# Reglas de comportamiento

## RC-01. [Nombre]

[Comportamiento esperado]

---

# Criterios de aceptación

## CA-01. [Nombre]

[Comportamiento verificable]

## CA-02. [Nombre]

[Comportamiento verificable]

---

# Escenarios de prueba

Cuando sea útil:

## CP-01 — [Nombre]

**Dado que** [precondición]  
**Cuando** [acción]  
**Entonces** [resultado]

---

# Escenarios alternativos / error

| ID | Escenario | Comportamiento esperado |
|---|---|---|
| FA-01 | | |

---

# Requisitos no funcionales

Documentar únicamente cuando hayan sido definidos.

| ID | Requisito |
|---|---|
| RNF-01 | |

---

# Dependencias

- [HU]
- [módulo]
- [servicio]

No inventar dependencias.

---

# Aclaraciones

[Aclaraciones necesarias.]

---

# Fuera de alcance

[Elementos explícitamente excluidos.]

---

# Preguntas pendientes

## Bloqueantes

1. ...

## Importantes

1. ...

---

# Prototipo

**Requerido:** Sí / No / Pendiente

Descripción del prototipo requerido:

[Descripción]

---

# Definition of Ready

- [ ] Actor definido.
- [ ] Objetivo definido.
- [ ] Campos definidos.
- [ ] Validaciones definidas.
- [ ] Reglas de negocio definidas.
- [ ] Flujo principal definido.
- [ ] Errores relevantes definidos.
- [ ] Criterios verificables.
- [ ] Dependencias identificadas.
- [ ] Sin preguntas bloqueantes.
- [ ] Sin supuestos funcionales críticos.

## Resultado

**Estado DoR:** Cumple / No cumple

**Pendientes:** [cantidad]