# LifeOS

Sistema operativo personal para Jesús: un asistente integral que funciona como espejo crítico, no como sabelotodo.

## Estructura del Proyecto

```
lifeos/
├── README.md                    # Este archivo
├── PROMPT.md                    # Prompt principal para iniciar conversaciones
│
├── docs/
│   ├── research/
│   │   └── self-analysis.md     # Análisis cruzado de 4 asistentes sobre Jesús
│   │
│   ├── architecture/
│   │   └── (system prompt, decisiones de diseño)
│   │
│   └── guides/
│       └── (guías de uso de skills)
│
├── skills/
│   ├── _index.md                # Índice y guía de cuándo usar cada skill
│   └── (skills individuales por área de vida)
│
└── changelog.md                 # Historial de cambios del proyecto
```

## Estado Actual

- [x] Análisis inicial completado (`docs/research/self-analysis.md`)
- [ ] System prompt base
- [ ] Definición de áreas de vida (Wheel of Life)
- [ ] Skills individuales
- [ ] Documentación de orquestación

## Principios de Diseño

1. **Honestidad brutal** — Crítico y directo, sin filtros
2. **Base científica** — Evidencia, no mitos
3. **Sin overfitting** — Simple y adaptable, no rígido
4. **Acción sobre planificación** — Detectar y confrontar procrastinación productiva

## Cómo Usar

1. Leer `PROMPT.md` para contexto inicial
2. Revisar `docs/research/self-analysis.md` para entender los patrones identificados
3. El system prompt (cuando exista) estará en `docs/architecture/`
