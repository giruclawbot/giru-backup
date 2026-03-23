# 🤖 Giru — OpenClaw Backup

Backup de configuración del agente **Giru** (👾 Giru-Giru!), un bot de software engineer corriendo en [OpenClaw](https://openclaw.ai).

Este repo contiene todo lo necesario para reconstruir a Giru desde cero si el servidor muere.

> ⚠️ **Repositorio público** — todos los secrets han sido removidos. Antes de restaurar, deberás reemplazar los valores `REPLACE_WITH_YOUR_VALUE` con tus credenciales reales.

---

## 📁 Estructura del repo

```
├── openclaw.template.json   # Configuración principal de OpenClaw (sin secrets)
├── AGENTS.md                # Instrucciones de comportamiento del agente
├── SOUL.md                  # Personalidad y voz de Giru
├── IDENTITY.md              # Identidad: nombre, emoji, avatar
├── USER.md                  # Info sobre el usuario (Kicks)
├── TOOLS.md                 # Notas de herramientas y setup local
├── HEARTBEAT.md             # Tareas periódicas del heartbeat
├── MEMORY.md                # Memoria long-term del agente
├── memory/                  # Logs diarios de memoria
│   ├── YYYY-MM-DD.md
│   └── *.json               # Estado de heartbeat, poemas, recetas, padel
└── skills/                  # Skills personalizadas instaladas
    ├── sonoscli/
    ├── ai-sdk-5/
    ├── django-drf/
    ├── github-pr/
    ├── jira-epic/
    ├── jira-task/
    ├── nextjs/
    ├── nextjs-15/
    ├── playwright/
    ├── pytest/
    ├── react-19/
    ├── self-improving-agent/
    ├── skill-creator/
    ├── tailwind-4/
    ├── typescript/
    ├── zod-4/
    └── zustand-5/
```

---

## 🔑 Secrets que necesitas recuperar

Antes de restaurar, ten listos los siguientes valores. Están marcados como `REPLACE_WITH_YOUR_VALUE` en `openclaw.template.json`:

| Campo | Descripción | Dónde obtenerlo |
|---|---|---|
| `channels.telegram.botToken` | Token del bot de Telegram | [@BotFather](https://t.me/BotFather) |
| `gateway.auth.token` | Token de autenticación del gateway | Generar uno nuevo con `openssl rand -hex 32` |
| `env.NVIDIA_API_KEY` | API Key de NVIDIA NIM | [build.nvidia.com](https://build.nvidia.com) |
| `tools.web.search.apiKey` | Brave Search API Key | [brave.com/search/api](https://brave.com/search/api/) |
| `auth.profiles.google:default` | Google AI API Key | [aistudio.google.com](https://aistudio.google.com) |
| `auth.profiles.openrouter:default` | OpenRouter API Key | [openrouter.ai/keys](https://openrouter.ai/keys) |
| `auth.profiles.github-copilot:github` | GitHub Copilot token | `gh auth login` |
| `models.providers.nvidia.baseUrl` | Base URL de NVIDIA NIM | `https://integrate.api.nvidia.com/v1` |
| `models.providers.nvidia.apiKey` | NVIDIA API Key (mismo que arriba) | Ver arriba |

---

## 🚀 Cómo restaurar a Giru

### 1. Instalar OpenClaw

```bash
npm install -g openclaw
```

### 2. Clonar este repo

```bash
git clone https://github.com/giruclawbot/giru-backup.git
cd giru-backup
```

### 3. Configurar OpenClaw

```bash
# Inicializar (crea ~/.openclaw/)
openclaw init

# Copiar la config plantilla
cp openclaw.template.json ~/.openclaw/openclaw.json
```

### 4. Reemplazar los secrets

Edita `~/.openclaw/openclaw.json` y reemplaza todos los `REPLACE_WITH_YOUR_VALUE` con los valores reales.

```bash
nano ~/.openclaw/openclaw.json
```

O usa `openclaw config` para configurarlos interactivamente.

### 5. Restaurar el workspace

```bash
# Copiar archivos del agente al workspace
WORKSPACE=$(jq -r '.agents.defaults.workspace' ~/.openclaw/openclaw.json)
mkdir -p "$WORKSPACE/memory"

# Archivos de identidad y comportamiento
cp AGENTS.md SOUL.md IDENTITY.md USER.md TOOLS.md HEARTBEAT.md MEMORY.md "$WORKSPACE/"

# Memoria
cp memory/* "$WORKSPACE/memory/"
```

### 6. Instalar las skills

```bash
SKILLS_DIR=~/.openclaw/skills
mkdir -p "$SKILLS_DIR"

# Copiar skills del backup al directorio de skills de OpenClaw
for skill in skills/*/; do
  name=$(basename "$skill")
  cp -r "$skill" "$SKILLS_DIR/$name"
  echo "✅ Skill instalada: $name"
done

# La skill sonoscli va al workspace
cp -r skills/sonoscli "$WORKSPACE/skills/sonoscli"
```

### 7. Configurar los canales

#### Telegram
```bash
openclaw channels add telegram --token "TU_BOT_TOKEN"
```

#### GitHub Copilot
```bash
gh auth login
openclaw auth add github-copilot
```

### 8. Iniciar el gateway

```bash
openclaw gateway start
```

### 9. Verificar

```bash
openclaw status
openclaw doctor
```

---

## 🔁 Mantener el backup actualizado

Para actualizar este repo cuando la configuración cambie:

```bash
# Desde el servidor donde corre Giru
cd /tmp && rm -rf giru-backup-update && mkdir giru-backup-update && cd giru-backup-update
git clone git@github.com:giruclawbot/giru-backup.git .

# Copiar archivos actualizados (sin secrets)
cp ~/.openclaw/workspace/AGENTS.md .
cp ~/.openclaw/workspace/SOUL.md .
cp ~/.openclaw/workspace/IDENTITY.md .
cp ~/.openclaw/workspace/USER.md .
cp ~/.openclaw/workspace/TOOLS.md .
cp ~/.openclaw/workspace/HEARTBEAT.md .
cp ~/.openclaw/workspace/MEMORY.md .
cp ~/.openclaw/workspace/memory/*.md memory/
cp ~/.openclaw/workspace/memory/*.json memory/

git add -A && git commit -m "chore: update backup $(date +%Y-%m-%d)" && git push
```

---

## 📝 Proyectos activos de Giru

| Repo | Descripción |
|---|---|
| [girupoemas](https://github.com/giruclawbot/girupoemas) | Poemas diarios de países en español |
| [blog-recetas-fit](https://github.com/giruclawbot/blog-recetas-fit) | 5 recetas saludables diarias |
| [blog-ejercicios-padel](https://github.com/giruclawbot/blog-ejercicios-padel) | 10 ejercicios de Padel diarios |
| [nombres](https://github.com/giruclawbot/nombres) | Enciclopedia de nombres en español |
| [snake-game](https://github.com/giruclawbot/snake-game) | Snake game con controles mobile |

---

## ℹ️ Info del sistema original

- **OS:** Linux 6.8.0 (Ubuntu, x64)
- **Node:** v24.14.0
- **OpenClaw version:** 2026.3.13
- **Default model:** github-copilot/claude-sonnet-4.6
- **Canal principal:** Telegram

---

*Backup mantenido por Giru 👾 — última actualización automática disponible en los commits del repo.*
