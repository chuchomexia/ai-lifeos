# LifeOS — Siguientes pasos

> Última actualización: 1 marzo 2026
> Regla: si un paso lleva más de 3 días "en progreso", algo anda mal.

## Fase 1: Hacer funcional lo que ya existe (esta semana)

- [ ] **Llenar `context/current-state.md`** — El template está listo. Sentarte 20 min y llenarlo honestamente. Sin esto el asistente opera a ciegas.
- [x] **Crear repo Git** — `git init`, `.gitignore` (ignorar `inbox/`, archivos sensibles), primer commit.
- [ ] **Primera sesión real con Claude Code** — Abrir el repo, verificar que CLAUDE.md se carga correctamente, tener una conversación real (no de prueba).

## Fase 2: OpenClaw + Ollama (siguiente semana)

Ver guía detallada: `docs/openclaw-setup.md`

- [ ] **Decidir máquina** — ¿Windows con WSL2 o Ubuntu limpio? Ver pros/contras abajo.
- [ ] **Instalar Ollama + modelo base** — `gpt-oss:20b` o `qwen2.5-coder:32b` según GPU disponible.
- [ ] **Instalar OpenClaw** — Seguir guía paso a paso.
- [ ] **Migrar CLAUDE.md a OpenClaw** — Adaptar el prompt como configuración de agente.
- [ ] **Conectar canal de mensajería** — Telegram recomendado para empezar.
- [ ] **Primera sesión real** — Misma prueba que con Claude Code: conversación real, no de prueba.

## Fase 3: Uso real y ajuste (semanas 3-4)

- [ ] **Usar LifeOS mínimo 3 veces por semana** durante 2 semanas antes de cambiar nada.
- [ ] **Primer skill real** — Crear uno cuando surja necesidad genuina (no antes).
- [ ] **Primera nota en `library/`** — Procesar un artículo/libro/video que ya tengas pendiente.
- [ ] **Primera reflexión en `logs/reflections/`** — No tiene que ser profunda. Un párrafo cuenta.
- [ ] **Revisar y ajustar** — Después de 2 semanas de uso real, evaluar qué funciona y qué no.

## Decisión pendiente: ¿Windows WSL2 o Ubuntu limpio?

| Factor | Windows + WSL2 | Ubuntu limpio |
|---|---|---|
| Facilidad de setup | Media. WSL2 funciona bien pero tiene quirks (red, puertos, RAM) | Alta. OpenClaw es nativo Linux |
| Recursos | WSL2 come RAM extra (~2-4GB). Necesitas `.wslconfig` para limitar | Todo el hardware disponible para Ollama |
| Siempre encendido | Requiere que Windows no duerma + script autostart | Systemd nativo, más estable como servidor |
| Acceso diario | Conveniente si ya usas Windows como máquina principal | Requiere SSH o estar frente a la máquina |
| GPU passthrough | Funciona con NVIDIA en WSL2 (CUDA) | Soporte nativo, sin overhead |
| Mantenimiento | Windows Update puede romper WSL2 ocasionalmente | Más predecible |

**Recomendación:** Si tienes una máquina dedicada (aunque sea vieja con GPU decente) → Ubuntu limpio. Si solo tienes tu PC de trabajo → WSL2 está bien para empezar.