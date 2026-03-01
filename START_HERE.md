# LifeOS Empieza Aquí (Checklist de Implementación)

Bienvenido a tu LifeOS. Este sistema está diseñado para ser tu espejo honesto y partner de pensamiento, ayudándote a pensar *a través* de los problemas en lugar de crear falsos sistemas de productividad.

Sigue estos pasos para personalizar tu LifeOS:

## 1. Configuración de Identidad y Contexto
- [ ] Abre `CLAUDE.md` y revisa las reglas. Cambia `[Tu Nombre]` por tu nombre, y personaliza las "Frases de bandera roja" basándote en tus excusas más frecuentes.
- [ ] Abre `context/about.md` y llena tus fortalezas, áreas de dificultad, influencias y frustraciones. Trata de ser brutalmente honesto.
- [ ] Abre `context/current-state.md` y define cuál será tu foco principal de este mes (máximo 3 proyectos activos).
- [ ] Opcional: Abre `context/self-analysis.md` y completa el análisis de tus patrones de evasión. (Puedes usar a un asistente LLM para que te analice según tus interacciones previas con él).

## 2. Configuración del Entorno de IA
- [ ] (Si usas Cursor/Cline/Windsurf u otro IDE con IA) Asegúrate de que este repositorio esté abierto como proyecto para que el LLM pueda leer el `CLAUDE.md` y la carpeta de `context/`.
- [ ] **Paso Inicial:** Abre el archivo `INITIALIZATION.md`, copia la instrucción inicial y pégala en tu IA favorita. Esto iniciará una entrevista paso a paso donde el agente te ayudará a extraer tu información y personalizar todos tus archivos de contexto automáticamente.
- [ ] **Configuración de Proyecto (Claude Projects, ChatGPT Custom GPTs, etc):** Si vas a usar una interfaz web en lugar de un IDE local, abre `WORKSPACE_PROMPT.md` para darle a tu IA el contexto sobre cómo debe comportarse como tu LifeOS y crear las instrucciones del sistema.


## 3. Limpieza y Uso de Estructura
- [ ] `projects/`: Crea una carpeta para cada proyecto personal activo (recuerda la regla, no más de 3).
- [ ] `inbox/`: Usa esta carpeta para soltar enlaces, ideas crudas, y archivos desordenados. Pídele al IA periódicamente que "procese la carpeta inbox".
- [ ] `library/`: Guarda aquí los aprendizajes validados, extractos o conceptos maduros. (Estructura tipo Second Brain).
- [ ] `logs/`: Guarda tus reflexiones (`reflections/`) o razonamientos críticos de decisiones (`decisions/`).

## 4. Evolución de Skills
- [ ] Revisa periódicamente la carpeta `.claude/skills` o `.agents/workflows` (dependiendo de tu entorno).
- [ ] Crea skills solo *on demand*. No crees un "playbook para escribir artículos" si no has escrito ni uno. Crea la herramienta de pensamiento cuando detectes un patrón repetitivo.

¡Y listo! Ya puedes empezar a usar tu LifeOS. Recuerda el principio #1: **Acción > estructura**.
