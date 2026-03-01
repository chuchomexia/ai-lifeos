---
name: process-content
description: Procesar contenido (artículos, videos, libros, papers, notas, threads) para extraer valor aplicable usando metodología CODE. Activar cuando Jesús traiga URLs, PDFs, mencione contenido que vio/leyó, o pida procesar/digerir material. El objetivo es aprendizaje aplicable, no solo resumen. Si menciona "guardar", "procesar", "qué opinas de este artículo", "vi este video", o trae enlaces, usa esta skill.
---

# Procesamiento de contenido

Esta skill convierte contenido en aprendizaje aplicable. No se trata de resumir — se trata de transformar información en conocimiento que cambia cómo piensas o actúas.

## Por qué existe esta skill

El problema no es falta de información, es exceso de consumo sin digestión. Leer 50 artículos sin procesar = entretenimiento disfrazado de productividad. Esta skill fuerza la pregunta crítica: **¿esto cambia algo en mi comportamiento o pensamiento?** Si la respuesta es no, no vale archivarlo.

## Metodología: CODE (Tiago Forte)

**C**apture → **O**rganize → **D**istill → **E**xpress

Adaptado para procesamiento efectivo:

### 1. CAPTURE — Extraer lo esencial (no todo)

¿Cuáles son las 3-5 ideas centrales? Máximo 5. Si necesitas más, el contenido es difuso o estás capturando detalles irrelevantes.

**Por tipo de contenido:**

- **Artículos/Papers**: Tesis principal + evidencia clave + conclusiones aplicables
- **Videos**: Ideas principales (ignorar relleno y anécdotas), timestamps útiles si el video vale revisar
- **Libros**: Conceptos centrales por capítulo relevante (no resumir todo el libro — nadie lee esos resúmenes)
- **Threads/Hilos**: Argumento principal + puntos que contradicen o expanden tu visión actual

### 2. ORGANIZE — Conectar con tu contexto

No guardes ideas en vacío. Pregunta:
- ¿Cómo se relaciona con proyectos actuales? (revisar CLAUDE.md, context/, projects/)
- ¿Confirma, contradice o expande algo que ya sabes?
- ¿Conecta con otros contenidos procesados? (buscar en library/ por temas relacionados)
- ¿Es evidencia para una decisión pendiente?
- **¿Nutre alguna idea fantasma previa?** (Las conexiones que haces aquí son lo que permite al agente aplicar las *thinking-tools* transversalmente luego).

**Si no puedes hacer al menos 2 conexiones claras, probablemente no vale archivar.**

### 3. DISTILL — Aplicar, no acumular

La pregunta crítica: **¿Qué cambia concretamente?**

- ¿Qué acción nueva puedo tomar?
- ¿Qué dejar de hacer?
- ¿Qué pregunta nueva tengo?
- ¿Qué hipótesis puedo probar?
- ¿Qué marco mental cambia?

**Si la respuesta es "nada", detente aquí.** El contenido fue entretenimiento, no aprendizaje. No hay problema con eso, pero no finjas que fue productivo.

### 4. EXPRESS — Archivar para uso futuro

Si pasó los filtros anteriores, crear nota estructurada en `library/`.

**Formato base** (adaptar según tipo):

```markdown
# [Título descriptivo]

**Fuente:** [URL o referencia completa]
**Tipo:** [Artículo | Video | Libro | Thread | Paper]
**Procesado:** [YYYY-MM-DD]
**Tags:** #tema1 #tema2 #tema3

## Ideas clave

- [Idea 1 — máximo una frase]
- [Idea 2]
- [Idea 3]
- (máximo 5)

## Conexiones

- **Proyectos:** [¿Con qué estoy trabajando relaciona esto?]
- **Conceptos:** [¿Qué marcos mentales o ideas previas toca?]
- **Contenido relacionado:** [Links a otras notas en library/ si existen]

## Aplicación

**Cambio concreto:** [Qué voy a hacer diferente]

- [ ] [Acción específica 1]
- [ ] [Acción específica 2]

## Notas adicionales (opcional)

[Contexto, citas textuales importantes, limitaciones del contenido, por qué es relevante ahora]
```

**Nombres de archivo**: Usar formato `YYYY-MM-DD-titulo-descriptivo.md` para fácil ordenamiento cronológico.

## Diferenciación por tipo de contenido

### Artículos técnicos / Papers

- Enfoque en metodología y resultados reproducibles
- ¿Los datos soportan las conclusiones? (crítica metodológica)
- Guardar referencias bibliográficas si vale investigar más

### Videos (YouTube, cursos, charlas)

- Timestamps de secciones clave (formato `[12:34] - tema`)
- Distinguir entre contenido denso vs. relleno motivacional
- Si es >30min y no tiene momentos destacables, probablemente no vale procesarlo completo

### Libros

- **No resumir todo el libro.** Procesar por capítulo o sección relevante
- Un libro puede generar 2-3 notas separadas si cubre temas distintos
- Incluir críticas: ¿qué falta? ¿Qué está sobresimplificado?

### Threads / Hilos sociales

- Identificar si es argumentación sólida o storytelling emocional
- Separar opinión de evidencia
- ¿El thread cambió tu opinión? ¿Por qué?

## Reglas de higiene

### Señal de alerta: inbox overflow

Si `inbox/` tiene >10 items sin procesar, **STOP**. Es acumulación, no aprendizaje.

**Protocolo:**
1. Procesar los 3 más relevantes
2. Descartar el resto sin culpa
3. No agregar nada nuevo hasta bajar de 5

**Por qué**: Si algo lleva más de una semana en inbox y no lo has procesado, no era tan importante. El FOMO de "perder" contenido es ilusorio — siempre habrá más contenido. La escasez real es tiempo y atención.

### Contenido sin aplicación = entretenimiento

No hay nada malo con consumir contenido por entretenimiento. El problema es disfrazarlo de "desarrollo personal" o "investigación" cuando realmente es procrastinación.

**Test rápido:** Si después de leer/ver algo tu primera reacción es buscar más contenido del mismo tema en lugar de aplicar lo aprendido, es entretenimiento.

### Evitar sobre-ingeniería

- No crear subcarpetas por tema hasta tener >20 notas
- No crear sistemas de tags elaborados — 3-5 tags pragmáticos bastan
- No reescribir notas viejas "para mejorar el formato" — es productive procrastination

## Workflow práctico

**Cuando Jesús trae contenido:**

1. **Validar intención**: ¿Quieres que lo procese completo o solo opinión rápida?
2. **Consumir el contenido**: Leer/ver el material
3. **Aplicar CODE**: Seguir los 4 pasos (Capture, Organize, Distill, Express)
4. **Decisión de archivo**:
   - Si cambia algo → Crear nota en `library/`
   - Si no cambia nada → Decir "Esto fue entretenimiento, no aprendizaje aplicable" y terminar
5. **Check de inbox**: Si hay >10 items, señalarlo

**Cuando Jesús pida "revisar inbox":**

1. Listar contenido pendiente con antigüedad
2. Priorizar por relevancia actual (no por FOMO)
3. Procesar top 3
4. Proponer descartar el resto

## Anti-patrones a detectar

Si noto estos patrones, llamarlos directamente:

- **"Guardar para leer después"** repetido sin procesar backlog → Inbox como cementerio
- **Resumir sin conectar** → Acumulación de resúmenes que nunca se revisan
- **Procesamiento perfeccionista** → Retrasar archivo porque "falta pulir la nota"
- **Rabbit holes de investigación** → Procesar contenido lleva a buscar más contenido en lugar de aplicar

## Ejemplos

**Ejemplo 1: Artículo técnico**

Input: "Procesa este artículo sobre Design Tokens architecture: [URL]"

Output:
```markdown
# Design Tokens Architecture - Netflix Design System

**Fuente:** https://netflixtechblog.com/...
**Tipo:** Artículo técnico
**Procesado:** 2026-02-28
**Tags:** #design-tokens #design-systems #architecture

## Ideas clave

- Tokens en 3 niveles: core (primitivos) → semantic (intención) → component (específicos)
- Versionado independiente de tokens vs componentes previene breaking changes
- Build-time transformation mejor que runtime para performance

## Conexiones

- **Proyectos:** Boreal Design System — actualmente usando solo 2 niveles (core + component), falta semántico
- **Conceptos:** Similar a atomic design pero aplicado a valores, no componentes
- **Contenido relacionado:** [design-system-patterns.md](library/2026-01-15-design-system-patterns.md)

## Aplicación

**Cambio concreto:** Implementar capa semántica de tokens en Boreal para desacoplar intención de valores específicos

- [ ] Mapear tokens actuales a 3 niveles
- [ ] Proponer estructura semantic tokens al equipo
- [ ] Crear POC con theming usando capa semántica

## Notas adicionales

Artículo asume mono-repo, Boreal usa Native Federation (módulos independientes). Necesito adaptar estrategia de versionado.
```

**Ejemplo 2: Video sin valor aplicable**

Input: "Vi este video sobre productividad: [URL de video motivacional genérico]"

Output:
```
Procesé el video. Es contenido motivacional sin metodología específica ni evidencia.

Ideas principales:
- "Levántate temprano"
- "Haz ejercicio"
- "Ten metas claras"

Conexión: Nada que no sepas ya.

Aplicación: Ninguna — esto fue entretenimiento, no aprendizaje aplicable.

¿Guardar en library/? No vale la pena. Si quieres entretenimiento motivacional, OK, pero no finjas que es desarrollo personal.
```

## Notas finales

Esta skill es un **filtro brutal**, no un archivador automático. Su trabajo es protegerte de la ilusión de productividad que viene de consumir sin digerir.

Si 70% del contenido que procesas termina descartado, **la skill está funcionando correctamente**.
