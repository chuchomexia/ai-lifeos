Si tu prioridad es **seguridad**, OpenClaw no es “otra app más”: por diseño mezcla **ejecución de acciones/herramientas** con **credenciales persistentes** (correo, archivos, servicios). Microsoft recomienda tratarlo como **ejecución de código no confiable** y **no correrlo en una workstation normal**; si se evalúa, que sea **en un entorno totalmente aislado (VM o máquina separada)**, con **credenciales dedicadas y de bajo privilegio**. ([Microsoft][1])

Con eso en mente, así lo haría (de más seguro → menos seguro):

## Qué opción es “lo más seguro”

1. **Mejor (recomendado): una máquina dedicada** (un mini-PC barato / NUC / otra torre) con Linux, solo para OpenClaw.

   * Aísla el riesgo real: si el agente se equivoca o se compromete, no afecta tu PC principal. ([Microsoft][1])

2. **Muy buena en tu caso: Ubuntu en disco/partición separada + uso “server”**

   * **Más seguro que “Windows con otro usuario”** (un usuario adicional *no* es una frontera fuerte contra un proceso con permisos y acceso a herramientas).
   * Consejo clave: **no montes** tus particiones de Windows en Ubuntu (o móntalas *solo lectura* si alguna vez lo necesitas), y usa **cifrado** del disco de Ubuntu si puedes.

3. **Aceptable si necesitas convivir: VM en tu PC (Ubuntu Server dentro de VirtualBox/Proxmox/Hyper-V)**

   * Aísla bastante, pero puede complicarse si quieres **GPU** a full (passthrough).

4. **Menos seguro: Windows “en otro usuario”**

   * No es una separación real de seguridad para este tipo de runtime con herramientas/credenciales.

## La forma más simple de instalar con Ollama (y que además te guía)

La vía “one-liner” que planteas está bien como *mecánica de instalación*:
**`ollama launch openclaw`**
Ollama hace el onboarding, muestra un aviso de riesgos, instala lo necesario (vía npm si falta) y levanta el “gateway daemon”. ([Ollama Docs][2])

## Hardening mínimo (lo que de verdad marca la diferencia)

### 1) Aislamiento y usuario dedicado

* En Ubuntu crea un usuario solo para esto (sin uso diario).
* Evita darle acceso a tus llaves SSH, carpetas personales, etc.

### 2) Sandboxing de herramientas (muy recomendado)

OpenClaw puede ejecutar herramientas dentro de **contenedores Docker** para reducir “blast radius”. Es **opcional**, y si está apagado, las herramientas corren en el host. ([OpenClaw][3])

* Actívalo (modo `non-main` o `all`) para que comandos/shell y herramientas vivan en el sandbox.

### 3) No lo expongas a Internet

* El gateway suele correr en **localhost** (p.ej. `http://localhost:18789`). ([Localtonet][4])
* Hay reportes de **muchas instancias expuestas** en Internet; mala idea. ([Bitsight][5])
* Si necesitas acceso remoto: **VPN (Tailscale) o túnel SSH**, no “abrir puertos”.

### 4) Autenticación fuerte del gateway

OpenClaw soporta token del gateway: si defines `OPENCLAW_GATEWAY_TOKEN`, las conexiones deben presentar el token o se cierran. ([OpenClaw][6])

* Usa un token largo/aleatorio y **no lo reutilices**.

### 5) Ollama: cuidado con “escuchar en 0.0.0.0”

Ollama te deja configurar `OLLAMA_HOST=0.0.0.0:11434` (por systemd) para recibir conexiones remotas. ([Ollama Docs][7])

* Para seguridad: **déjalo en localhost** salvo que tengas VPN/firewall bien hecho.

### 6) Credenciales “de quemar” al inicio

La recomendación más práctica (y alineada con Microsoft): prueba con **cuentas dedicadas y sin datos sensibles**, con permisos mínimos, y rota/revoca credenciales si haces cambios. ([Microsoft][1])

## Si tu meta es “servidor y dejarlo encendido”

En tu PC gamer, lo más coherente es:

* **Ubuntu Server en SSD separado**, booteas a Ubuntu cuando lo vayas a dejar 24/7.
* Durante esa etapa, no usas Windows (porque en dual boot no corren a la vez).

Si necesitas **jugar en Windows mientras el servidor sigue**, entonces ya no es dual boot: ahí conviene **otra máquina** o una **VM/Proxmox** dedicado.

---

Si me dices qué cosas quieres que haga OpenClaw (¿correo/calendario? ¿automatizar archivos locales? ¿WhatsApp/Telegram?), te digo un “perfil” de permisos y aislamiento recomendado (mínimo viable) para ese caso, sin sobre-abrir credenciales.

[1]: https://www.microsoft.com/en-us/security/blog/2026/02/19/running-openclaw-safely-identity-isolation-runtime-risk/?utm_source=chatgpt.com "Running OpenClaw safely: identity, isolation, and runtime risk"
[2]: https://docs.ollama.com/integrations/openclaw?utm_source=chatgpt.com "OpenClaw"
[3]: https://docs.openclaw.ai/gateway/sandboxing?utm_source=chatgpt.com "Sandboxing"
[4]: https://localtonet.com/blog/how-to-self-host-openclaw?utm_source=chatgpt.com "How to Self-Host OpenClaw and Access It Remotely"
[5]: https://www.bitsight.com/blog/openclaw-ai-security-risks-exposed-instances?utm_source=chatgpt.com "OpenClaw Security: Risks of Exposed AI Agents Explained"
[6]: https://docs.openclaw.ai/gateway/protocol?utm_source=chatgpt.com "Gateway Protocol"
[7]: https://docs.ollama.com/faq?utm_source=chatgpt.com "FAQ"


Vale — tu idea de “Ubuntu en otro boot/partición para aislar” **sirve**, pero hay un matiz importante: **dual-boot aísla procesos (porque solo corre un OS a la vez), pero NO aísla datos por sí solo**. Si Ubuntu puede montar la partición de Windows (o viceversa), puede leer/escribir esos archivos… a menos que estén **cifrados**.

A partir de ahí, esto es lo que te conviene.

## 1) Partición / dual-boot: qué te protege y qué no

### Lo que SÍ te da

* **Aislamiento en ejecución**: cuando estás en Ubuntu, Windows está apagado (y al revés). Eso reduce riesgos de “contagio en vivo” entre sistemas.

### Lo que NO te da automáticamente

* **Aislamiento de datos**: si tu partición de Windows está sin cifrar (o la clave está accesible), Ubuntu puede montarla y leerla. Y Windows puede leer Linux si Linux no está cifrado.

### La barrera real para “aislar”

* **Cifrado de disco/partición** en ambos lados (Linux con LUKS; Windows con BitLocker si aplica). Ubuntu documenta FDE/LUKS como la forma de proteger el contenido “en reposo”. ([Ubuntu Documentation][1])
* Y aun así: si alguien tiene acceso físico y suficiente privilegio, puede intentar atacar el sistema; el cifrado sube muchísimo la barra.

## 2) La forma más segura de hacerlo en tu PC gamer

### Opción A (mejor práctica): **Ubuntu en un SSD separado**

Es lo más limpio por seguridad y por dolores de cabeza de bootloader. Mucha gente recomienda dos discos y (si puedes) instalar Linux con el disco de Windows desconectado para evitar líos. ([xda-developers.com][2])

**Ventajas**:

* Más difícil “equivocarse” y pisar Windows.
* Puedes **cifrar todo el disco de Ubuntu** fácil durante instalación.

**Ojo**: Ubuntu 24.04 ha tenido limitaciones/quejas de que el instalador **no ofrece cifrado** cuando haces particionado “a mano” en un disco compartido (depende del camino de instalación). En disco completo suele ser más directo. ([Ubuntu Community Hub][3])

### Opción B: mismo disco, partición separada

Funciona, pero para tu objetivo de seguridad hay dos puntos clave:

1. **Cifra Ubuntu** (idealmente) y 2) **evita montar Windows** desde Ubuntu (y cifra Windows si quieres protegerlo de lectura desde Linux).

## 3) “No montar Windows” en Ubuntu (para minimizar exposición)

Esto no es una frontera “anti-root”, pero reduce accidentes y exposición casual.

### A) Desactivar automontaje (GNOME)

Puedes desactivar automount vía configuración (o con `gsettings`). ([Ask Ubuntu][4])

### B) fstab con `noauto` (evita montaje al arrancar)

Ubuntu explica `/etc/fstab` como el archivo para automatizar montajes. ([Ubuntu Documentation][5])
Y para impedir automontaje, se usa `noauto`. ([Ask Ubuntu][6])

Ejemplo (idea general; **no copies a ciegas** sin revisar tu UUID):

```bash
sudo blkid   # para ver UUID de la partición NTFS
sudo nano /etc/fstab
```

Línea típica:

```fstab
UUID=XXXX-XXXX  /mnt/windows  ntfs3  noauto,ro,nofail  0  0
```

* `noauto`: no monta solo
* `ro`: solo lectura (si alguna vez lo montas)
* `nofail`: no rompe el boot si no está

## 4) Telegram: lo más seguro (y con “internet mínimo”)

Para Telegram, el punto más importante es: **usa Bot API con long polling (`getUpdates`)**, no webhook.

* Telegram dice que `getUpdates` (polling) y webhook son métodos excluyentes, y con webhook **Telegram se conecta a tu servidor**, lo cual normalmente implica **abrir un puerto entrante + TLS**. Con `getUpdates`, tu servidor solo hace **conexiones salientes**. ([Telegram Core][7])
* OpenClaw indica que en Telegram **long polling es el modo por defecto** y webhook es opcional. ([OpenClaw][8])

### Pasos concretos con OpenClaw + Telegram (lo mínimo)

OpenClaw lo deja bastante guiado: ([OpenClaw][8])

1. Crear bot en BotFather y obtener token
2. Configurar token:

   * en config: `channels.telegram.botToken`, o
   * env: `TELEGRAM_BOT_TOKEN=...` (fallback) ([OpenClaw][8])
3. Arrancar gateway y aprobar primer DM (modelo “pairing” por defecto):

```bash
openclaw gateway
openclaw pairing list telegram
openclaw pairing approve telegram <CODIGO>
```

4. Para controlar quién puede hablarle:

   * deja `dmPolicy: "pairing"` o cambia a `allowlist` con `allowFrom` (IDs numéricos). ([OpenClaw][8])
   * Para encontrar tu user ID, OpenClaw sugiere mirar `from.id` en logs o usar el método oficial `getUpdates`. ([OpenClaw][8])

**Recomendación de seguridad para empezar**:

* **DMs**: `pairing` o `allowlist` (no “open”)
* **Grupos**: deshabilitado o allowlist estricto al inicio (porque en grupos el “quién puede inducir acciones” se vuelve más riesgoso). ([OpenClaw][8])

## 5) “Internet mínimo”: firewall práctico (sin romper Telegram)

### Entrante: NIEGA todo

Eso es lo más importante en un server casero:

* `ufw default deny incoming` (y solo abres SSH si lo necesitas)

### Saliente: aquí está el trade-off real

Para Telegram Bot API necesitas **salida HTTPS a `api.telegram.org`**. OpenClaw incluso menciona que si bloqueas DNS/HTTPS, fallan cosas como `setMyCommands`. ([OpenClaw][8])

Peeeero: **UFW no filtra por dominio**, filtra por IP. Si intentas “solo a api.telegram.org por nombre”, no va directo; tendrías que permitir IPs (que pueden cambiar). ([Ask Ubuntu][9])

**Opciones:**

1. **Simple y robusta**: permitir salida 443 (HTTPS) en general, pero mantener el host bien encerrado (sin puertos entrantes, gateway en localhost, token).
2. **Más estricta**: usar un **proxy de salida** (Squid/tinyproxy) y permitir salida solo al proxy; el proxy restringe a `api.telegram.org`. OpenClaw soporta proxy SOCKS5 para Telegram si lo necesitas. ([OpenClaw][8])

Si quieres un arranque rápido (y razonable) en casa:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp   # solo si vas a administrar por SSH
sudo ufw enable
```

Y **no expones** el gateway a la red.

## 6) Dos ajustes de OpenClaw que sí o sí haría

* **Gateway solo en loopback + auth por token**. La doc oficial de seguridad de OpenClaw da un baseline “bind loopback + token largo” como endurecimiento base. ([OpenClaw][10])
* **Cero “skills” de terceros al principio**. Ha habido reportes recientes de malware/infostealers alrededor de extensiones/skills y configuraciones. Mantén instalación mínima hasta que todo esté estable. ([The Verge][11])

---

### Recomendación directa para tu caso (servidor 24/7 + Telegram + mínimo internet)

1. **SSD separado** para Ubuntu (si puedes) + **cifrado (LUKS/FDE)**. ([Ubuntu Documentation][1])
2. OpenClaw + Ollama local.
3. Telegram por **long polling** (sin webhooks). ([OpenClaw][8])
4. Firewall: **deny incoming**, outgoing normal al principio; si después quieres “salida mínima real”, lo hacemos con proxy/egress controlado.

Si me dices:

* ¿Vas a usar **solo DM** o también **grupos**?
* ¿Necesitas administrar el server por **SSH** desde otro equipo?
  te dejo un ejemplo de `channels.telegram` (config) con allowlists y políticas “fail-closed” para que quede *bien cerrado* desde el día 1.

[1]: https://documentation.ubuntu.com/security/security-features/storage/encryption-full-disk/?utm_source=chatgpt.com "Full disk encryption (FDE) - Ubuntu security documentation"
[2]: https://www.xda-developers.com/5-pitfalls-of-dual-booting-windows-and-linux-that-i-wasnt-prepared-for/?utm_source=chatgpt.com "5 pitfalls of dual-booting Windows and Linux that I wasn't ..."
[3]: https://discourse.ubuntu.com/t/24-04-disk-encryption-not-available-during-install-if-using-a-partition-and-not-full-disk/55238?utm_source=chatgpt.com "24.04: Disk encryption not available during install if using a ..."
[4]: https://askubuntu.com/questions/497958/how-to-disable-automount-of-an-harddrive?utm_source=chatgpt.com "How to disable automount of an harddrive?"
[5]: https://help.ubuntu.com/community/Fstab?utm_source=chatgpt.com "Fstab - Community Help Wiki - Official Ubuntu Documentation"
[6]: https://askubuntu.com/questions/421585/how-can-i-prevent-auto-mounting-of-a-partition-in-fstab?utm_source=chatgpt.com "How can I prevent auto-mounting of a partition in fstab?"
[7]: https://core.telegram.org/bots/api?utm_source=chatgpt.com "Telegram Bot API"
[8]: https://docs.openclaw.ai/channels/telegram "Telegram - OpenClaw"
[9]: https://askubuntu.com/questions/1319825/how-can-i-block-outgoing-traffic-to-a-domain-with-all-subdomains-with-ufw-or-ipt?utm_source=chatgpt.com "How can I block outgoing traffic to a domain with all ..."
[10]: https://docs.openclaw.ai/gateway/security "Security - OpenClaw"
[11]: https://www.theverge.com/news/874011/openclaw-ai-skill-clawhub-extensions-security-nightmare?utm_source=chatgpt.com "OpenClaw's AI 'skill' extensions are a security nightmare"


# OpenClaw + Ollama — Guía de instalación

> Esta guía cubre las dos opciones: Ubuntu limpio y Windows con WSL2.
> OpenClaw era conocido antes como Clawdbot/Moltbot.

## Requisitos mínimos

- **RAM:** 16GB mínimo (32GB recomendado para modelos grandes)
- **GPU:** NVIDIA con 8-12GB VRAM para modelos locales decentes. Sin GPU funciona en CPU pero lento.
- **Disco:** ~20GB libres para modelos
- **Node.js:** v22+

## Opción A: Ubuntu limpio

### 1. Sistema base

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git build-essential
```

### 2. Instalar Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh

# Verificar
ollama --version

# Descargar modelo (elegir según tu GPU)
ollama pull gpt-oss:20b          # Buen balance rendimiento/calidad
# ollama pull qwen2.5-coder:32b  # Alternativa si tienes más VRAM
# ollama pull llama3.3            # Alternativa popular

# Verificar que funciona
ollama run gpt-oss:20b "Hola, prueba rápida"
```

### 3. Instalar Node.js 22

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
node --version  # Debe ser v22.x
```

### 4. Instalar OpenClaw

```bash
npm install -g openclaw@latest
openclaw onboard
```

Durante el onboard:
- Seleccionar **Gateway service** cuando pregunte
- Elegir Telegram (u otro canal) como mensajería
- Configurar el modelo cuando pregunte

### 5. Configurar Ollama como provider

```bash
# Habilitar Ollama para OpenClaw
export OLLAMA_API_KEY="ollama-local"

# O configurar directamente
openclaw config set models.providers.ollama.apiKey "ollama-local"
```

Verificar que OpenClaw ve los modelos:

```bash
openclaw models
```

### 6. Configurar modelo por defecto

Editar `~/.openclaw/config.json`:

```json
{
  "models": {
    "providers": {
      "ollama": {
        "apiKey": "ollama-local",
        "api": "ollama"
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/gpt-oss:20b"
      }
    }
  }
}
```

**IMPORTANTE:** NO usar `/v1` en la URL de Ollama. La URL nativa es `http://127.0.0.1:11434` sin sufijo. El modo `/v1` rompe tool calling.

### 7. Iniciar y verificar

```bash
openclaw gateway start
openclaw dashboard  # Abre http://127.0.0.1:18789
```

### 8. Autostart con systemd (para que corra siempre)

```bash
openclaw onboard --install-daemon
# Esto crea el servicio systemd automáticamente
```

---

## Opción B: Windows con WSL2

### 1. Instalar WSL2

En PowerShell como Administrador:

```powershell
wsl --install
# Reiniciar cuando pida
```

Después del reinicio, Ubuntu se abre automáticamente. Crear usuario y contraseña.

### 2. Habilitar systemd en WSL

Dentro de Ubuntu (WSL):

```bash
sudo tee /etc/wsl.conf >/dev/null <<'EOF'
[boot]
systemd=true
EOF
```

En PowerShell:

```powershell
wsl --shutdown
# Volver a abrir Ubuntu
```

### 3. Instalar Ollama

**Opción recomendada:** Instalar Ollama en Windows (no en WSL) para mejor acceso a GPU.

Descargar desde: https://ollama.com/download/windows

Después en WSL, apuntar OpenClaw al Ollama de Windows:

```bash
# Ollama en Windows es accesible desde WSL vía localhost
curl http://localhost:11434/api/tags  # Verificar conexión
```

**Alternativa:** Instalar Ollama dentro de WSL (funciona con GPU NVIDIA si tienes drivers WSL2-CUDA):

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama pull gpt-oss:20b
```

### 4. Instalar Node.js y OpenClaw en WSL

```bash
sudo apt update && sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs

npm install -g openclaw@latest
openclaw onboard
```

### 5. Configurar (mismo que Ubuntu, paso 5-6 arriba)

Si Ollama corre en Windows y OpenClaw en WSL, la URL por defecto `localhost:11434` debería funcionar. Si no:

```json
{
  "models": {
    "providers": {
      "ollama": {
        "apiKey": "ollama-local",
        "baseUrl": "http://host.docker.internal:11434",
        "api": "ollama"
      }
    }
  }
}
```

### 6. Limitar RAM de WSL

Crear `C:\Users\TU_USUARIO\.wslconfig`:

```ini
[wsl2]
memory=8GB
swap=4GB
```

### 7. Acceder desde Windows

```
http://localhost:18789
```

Si necesitas acceso desde otra máquina en la red, en PowerShell como Admin:

```powershell
netsh advfirewall firewall add rule name="OpenClaw" dir=in action=allow protocol=TCP localport=18789
```

---

## Conectar Telegram

1. Hablar con @BotFather en Telegram
2. Crear bot: `/newbot`
3. Copiar el token
4. En OpenClaw: pegar token cuando lo pida durante `openclaw onboard`, o configurar después en el dashboard

---

## Migrar LifeOS a OpenClaw

Una vez OpenClaw funciona:

1. Copiar el repo LifeOS al workspace de OpenClaw (`~/.openclaw/workspace/` o donde hayas configurado)
2. OpenClaw lee `CLAUDE.md` automáticamente (compatible con el formato)
3. Los skills en `.claude/skills/` son compatibles — OpenClaw los carga igual
4. Verificar con una conversación real que el contexto se carga correctamente

---

## Modelos recomendados según hardware

| GPU VRAM | Modelo sugerido | Notas |
|---|---|---|
| 8GB | `qwen2.5-coder:14b` | Funcional pero limitado para tareas complejas |
| 12GB | `gpt-oss:20b` | Buen balance. Recomendado para empezar |
| 16GB+ | `qwen2.5-coder:32b` | Mejor calidad, más lento |
| 24GB+ | `deepseek-r1:32b` | Razonamiento avanzado |
| Sin GPU | `gpt-oss:20b` (CPU) | Funciona pero respuestas lentas (~30-60s) |

Ollama también ofrece modelos cloud gratuitos para empezar: `ollama pull minimax:latest`

---

## Troubleshooting común

**"openclaw: command not found"**
→ `npm install -g openclaw@latest` de nuevo, o usar `npx openclaw`

**Ollama no detectado por OpenClaw**
→ Verificar: `curl http://localhost:11434/api/tags`
→ Asegurar que `OLLAMA_API_KEY` está seteado

**Modelos aparecen pero tool calling no funciona**
→ NO usar URL con `/v1`. Usar `http://localhost:11434` directamente.

**WSL2: "connection refused" al acceder dashboard**
→ Probar `http://127.0.0.1:18789` en vez de `localhost`

**Alta memoria en WSL2**
→ Configurar `.wslconfig` y hacer `wsl --shutdown` periódicamente