# LifeOS - AI Thinking Partner Template

Un sistema personal de pensamiento asistido por Inteligencia Artificial. No es una aplicación de software — es una arquitectura de contexto estructurado (basada en Markdown) para que tu asistente de IA (Claude, ChatGPT, etc.) te conozca profundamente y actúe como un socio estratégico y un espejo honesto.

## ¿Qué es LifeOS?

La mayoría de nosotros usamos los LLMs como enciclopedias motivacionales. **LifeOS cambia la dinámica.**
En lugar de darte consejos genéricos, LifeOS lee tus vulnerabilidades, tus patrones de procrastinación, tus fortalezas reales y tu enfoque actual para **forzarte a pensar a través de los problemas** y evitar que te mientas a ti mismo.

Es el anti-gurú. Es tu socio para decisiones difíciles.

## Cómo empezar

1. Clona este repositorio en tu máquina local o abrelo en un IDE soportado por IA (como Cursor, Windsurf, o Cline).
2. Sigue las instrucciones del archivo **`START_HERE.md`** en la raíz del proyecto.
3. El proceso te guiará a través de una entrevista paso a paso asistida por IA para personalizar tu propio LifeOS.

## Arquitectura

- `CLAUDE.md`: Define el comportamiento nuclear del asistente (sus reglas, cómo debe hablarte, qué no hacer).
- `context/`: La "memoria base" sobre ti (quién eres, en qué estás trabajando, cuáles son tus puntos ciegos).
- `.claude/skills/`: Playbooks y herramientas de pensamiento que el asistente puede invocar según la situación.
- `library/`: Tu *Second Brain*; contenido que verdaderamente procesaste y conectaste.
- `inbox/`: Carpeta temporal para recolectar información en bruto.
- `logs/`: Historial de tus mayores decisiones y reflexiones para que el agente siga tu evolución.

---

## Cómo contribuir

LifeOS es de código abierto y cualquiera puede aportar para mejorar la plantilla y las herramientas de pensamiento subyacentes.

### ¿Qué tipo de contribuciones buscamos?

1. **Nuevas "Skills" o Thinking Tools**: Si tienes una metodología, modelo mental o framework psicológico (ej: metodologías para toma de decisiones, priorización de tiempo, análisis de código) que crees que todo LifeOS debería tener como herramienta predeterminada, envíanos la skill en la carpeta `.claude/skills/`. Revisa la skill `thinking-tools` como ejemplo.
2. **Mejoras a los Prompts Bases**: `WORKSPACE_PROMPT.md`, `INITIALIZATION.md`, o `MEMORY_EXTRACTION_PROMPT.md` pueden ser iterados eternamente. Si descubres formas de extraer mejor información de un modelo, abre un PR.
3. **Refinamiento a la arquitectura del contexto**: Sugerencias de nuevas carpetas o formas de estructurar la información útil para el IA.

### Pasos para contribuir

1. Haz un **Fork** de este repositorio.
2. Crea una rama para tu característica o mejora (`git checkout -b feature/nueva-skill`).
3. Asegúrate de que tus aportes sean **GENÉRICOS**. Esto es una plantilla. ¡NO introduzcas tu información personal, nombres, datos o archivos privados en tu PR!
4. Realiza tus commits con un formato claro (`git commit -m "feat: agrega nueva herramienta de análisis post-mortem"`).
5. Empuja la rama (`git push origin feature/nueva-skill`).
6. Abre un **Pull Request** detallando por qué crees que tu cambio mejorará LifeOS para todos.

## Principio rector

> *El valor se mide en comportamiento cambiado, no en documentación producida.*