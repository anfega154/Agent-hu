# Plantilla oficial de Historia de Usuario — COMPIRA

Toda Historia de Usuario debe utilizar esta estructura.

---

# HU-XXX — [Nombre de la historia]

| Campo | Valor |
|---|---|
| Identificador | HU-XXX |
| Nombre | [Nombre funcional] |
| Módulo | [Módulo] |
| Actor | [Actor] |
| Estado | [Ver lista única de estados abajo] |
| Prioridad | [Alta / Media / Baja — o "Pendiente por definir"] |
| Versión | [Ej. 1.0; incrementar ante cambios importantes] |
| Fuente principal | DOC / HU / DEC / RT |
| Última actualización | YYYY-MM-DD |
| Dependencias | [HU relacionadas] |

> **Prioridad:** no inventar. Si no fue definida, usar `Pendiente por definir`.
>
> **Identificador inmutable:** una vez asignado `HU-XXX` no se renumera ni se
> reutiliza. Si la HU deja de aplicar usar `Estado: Descartada`; si se combina,
> `Estado: Fusionada en HU-XXX`; si se reemplaza, `Estado: Reemplazada por HU-XXX`.

**Estados válidos (lista única):** `Borrador` · `En análisis` · `Con pendientes` ·
`Lista para validación` · `Aprobada` · `Lista para desarrollo` · `En desarrollo` ·
`En validación` · `Completada` · `Descartada` · `Fusionada en HU-XXX` ·
`Reemplazada por HU-XXX`.

El paso `Lista para validación → Aprobada` requiere decisión humana.

---

## Resumen ágil

Como **[actor]** necesito **[necesidad]**, para **[beneficio / objetivo funcional]**.

> Este apartado es solo un resumen. La especificación completa es el "Contexto
> funcional" siguiente.

## Contexto funcional

Explica, cuando corresponda:

- por qué existe la HU y qué problema resuelve;
- dónde inicia el proceso y dónde termina;
- actores involucrados;
- módulos involucrados;
- resultado esperado.

Debe ser entendible por negocio sin conocimientos técnicos profundos. Si algo no
está definido, usar `Pendiente por definir`, nunca un supuesto.

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

Para cada regla, registrar su procedencia cuando pueda resultar ambigua:
`Origen: DOC / HU / DEC / RT / REC`.

## RN-01. [Nombre]

[Regla]

Origen: [DOC / HU / DEC / RT / REC]

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