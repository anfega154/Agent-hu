# CLAUDE.md — COMPIRA · compira-hu-analyst

Este repositorio se usa para el **análisis funcional de COMPIRA y la generación
de Historias de Usuario (HU)**. No es un repositorio de código de aplicación.

## Instrucciones del proyecto

Las instrucciones completas están en **`AGENTS.md`** (raíz del repositorio).
Síguelas como si formaran parte de este archivo.

Antes de trabajar, lee siempre, en este orden:

1. `agent/compira-hu-analyst.prompt.md` — rol, objetivo y proceso obligatorio.
2. `.kiro/steering/compira-context.md` — contexto funcional y técnico de COMPIRA.
3. `.kiro/steering/hu-rules.md` — reglas maestras de análisis de HU.
4. `.kiro/steering/hu-template.md` — plantilla oficial de HU.

## Reglas críticas (resumen)

- No inventes requerimientos: usa `Pendiente por definir` o `Requiere confirmación`.
- Primero el mapa funcional (análisis, inventario, matriz, pendientes); las HU
  individuales solo después.
- No crees HU técnicas; las HU representan capacidades funcionales.
- Marca tus propuestas como `RECOMENDACIÓN — requiere aprobación`.
- Registra las contradicciones como pendientes; no las resuelvas por tu cuenta.
- Protege el alcance académico de COMPIRA.
- Solo puedes escribir en `docs/historias-usuario/**`. No toques código ni
  `.kiro/steering/**`, `docs/proyecto/**`, `docs/referencia/**` sin que se pida.

## Subagente

Para el trabajo de análisis funcional, delega en el subagente
`compira-hu-analyst` (definido en `.claude/agents/compira-hu-analyst.md`).

## Flujo controlado

**Analizar → Proponer → Revisar → Aprobar → Generar → Validar.** Nunca generes
todo el backlog de una vez sin aprobación humana intermedia. El `README.md`
documenta el flujo completo y los prompts sugeridos.
