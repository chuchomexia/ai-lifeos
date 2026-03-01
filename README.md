# LifeOS

Sistema personal de pensamiento asistido por AI. No es software — es contexto estructurado para un asistente que me conoce.

## Qué es

Un repositorio que alimenta a Claude Code (y eventualmente OpenClaw) con contexto sobre quién soy, qué estoy haciendo, y cómo ayudarme mejor. El asistente funciona como espejo honesto y partner de pensamiento, no como guru ni coach.

## Cómo funciona

- `CLAUDE.md` define el comportamiento del asistente.
- `context/` tiene mi perfil, estado actual y auto-análisis.
- `.claude/skills/` tiene playbooks por área de vida que se cargan bajo demanda.
- `library/` es mi second brain: contenido procesado y conectado.
- `inbox/` es temporal: lo que entra se procesa o se descarta.

## Uso

```bash
# Con Claude Code
claude

# El asistente lee CLAUDE.md automáticamente y carga
# context/ y skills/ según lo necesite.
```

## Principio rector

El valor se mide en comportamiento cambiado, no en documentación producida.