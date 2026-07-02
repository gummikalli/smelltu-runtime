# Smelltu classroom — local runtime

Run the classroom on your own machine: follow the live presentation on your
screen and get **your own sandbox shell** for the interactive parts.

## Setup (one command)

Requirements: [Docker Desktop](https://www.docker.com/products/docker-desktop/)
(running) and git.

**Mac / Linux / WSL:**

```bash
curl -fsSL https://courses.smelltuher.is/start | bash
```

**Windows (PowerShell):**

```powershell
irm https://courses.smelltuher.is/start.ps1 | iex
```

The installer asks for one thing: your **runtime token** (log in at
<https://courses.smelltuher.is>, click **local runtime**, generate one). It
generates the local passwords for you, starts everything, and prints your
login. Re-run the same command any time to update.

<details><summary>Manual setup (if you prefer)</summary>

```bash
git clone https://github.com/gummikalli/smelltu-runtime.git
cd smelltu-runtime
cp .env.example .env   # paste your runtime token + choose two passwords
docker compose up -d
```

Open <http://localhost:3000> and log in with `student@localhost.local` and
your `LOCAL_USER_PASSWORD`.
</details>

## AI in the sandbox (Claude Code)

Your sandbox shell ships with **Claude Code** (`claude`). By default it uses
the **classroom AI** — the instructor's Anthropic access, proxied through the
course server using your runtime token (a daily budget applies; the
instructor's key never reaches your machine). If you have your own Anthropic
account, put `ANTHROPIC_API_KEY=sk-ant-...` in `.env` and restart — your key
is then used directly and the classroom budget doesn't apply.

## During class

- Your local pages **follow the teacher** automatically (`● LIVE (cloud)`).
  Navigate freely any time; snap back with **follow teacher ↩**.
- On interactive slides, toggle between **👁 watch teacher** (a read-only
  mirror of the teacher's shell) and **⌨ take control** (your own local
  sandbox shell — type away, you can't break anything outside the box).
- The sandbox shell also works outside class hours, so you can redo any
  exercise whenever you like.

## Everyday commands

```bash
docker compose up -d      # start (also refreshes course content)
docker compose down       # stop
docker compose down -v    # full reset (wipes local progress + sandbox)
docker compose pull       # update to the newest images
```

## Troubleshooting

- **"RUNTIME_TOKEN … generate one at /runtime"** — the `.env` value is
  missing. Paste the full `smrt_…` token.
- **`init` fails with 401** — your token expired or was revoked; generate a
  new one on the course site and `docker compose up -d` again.
- **Not following the presentation** — tokens are personal and time-limited;
  check `docker compose logs sync` for `bridge:` lines.
