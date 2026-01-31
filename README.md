# Mojo Devcontainer

This repo is a **Mojo Devcontainer** that can be used in **two independent ways**:

1. **VS Code Dev Containers** (GUI workflow)
2. **Neovim-only** using plain Docker (no VS Code required)

Both workflows use the same toolchain inside the container:

- Ubuntu + common CLI tooling
- Neovim
- Pixi
- Mojo (via Modular Pixi flow)

---

## Repo layout

```text
.
├─ .devcontainer/
│ ├─ Dockerfile
│ └─ devcontainer.json
├─ pixi.toml
├─ pixi.lock
└─ src/
└─ main.mojo
```

> Notes:
>
> - `.pixi/` is the local environment directory and should be **gitignored**
> - `pixi.lock` should be **committed** for reproducibility

---

## Requirements

### For the VS Code workflow

- Docker
- VS Code
- VS Code extension: **Dev Containers**

### For the Neovim-only workflow

- Docker
- (Optional) Neovim on the host, if you want to edit locally instead of inside the container

That’s it. You do **not** need Mojo or Pixi installed on the host.

---

## Mojo commands (same in both workflows)

All Mojo usage goes through Pixi:

- Mojo REPL:

  ```bash
  pixi run repl
  ```

- Run the example:

```bash
pixi run run-main
```

- Tests:

```bash
pixi run test
```

```

```

- Format:

````
```bash
pixi run fmt
````

---

## Workflow A: Neovim

This workflow uses plain Docker to build and run the same container, and you work entirely from your terminal with Neovim.

### Option B1 (recommended): Edit in Neovim inside the container

1. Build the image
   From the repo root:

```bash
docker build -t mojo-playground -f .devcontainer/Dockerfile .
```

2. Start a shell in the container with your repo mounted

```bash
docker run --rm -it \
  -v "$PWD:/workspaces/mojo-playground" \
  -w "/workspaces/mojo-playground" \
  mojo-playground bash
```

3. Create the Pixi environment (first time only)
   Inside the container:

```bash
pixi install
pixi run mojo --version
```

4. Use Neovim and Mojo
   Inside the container:

```bash
nvim .
```

Then run Mojo commands from Neovim’s terminal, or from the shell:

```bash
pixi run run-main
pixi run repl
```

### Option B2: Edit on the host, run Mojo inside the container

If you prefer to use your host Neovim (or any editor), you can keep editing locally and run Mojo in the container.

1. Build the image

```bash
docker build -t mojo-playground -f .devcontainer/Dockerfile .
```

2. Run Mojo commands in a one-off container
   From the repo root:

```bash
docker run --rm -it \
  -v "$PWD:/workspaces/mojo-playground" \
  -w "/workspaces/mojo-playground" \
  mojo-playground bash -lc "pixi install && pixi run run-main"
```

Or start an interactive shell:

```bash
docker run --rm -it \
  -v "$PWD:/workspaces/mojo-playground" \
  -w "/workspaces/mojo-playground" \
  mojo-playground bash
```

Then:

```bash
pixi run repl
```

---

### Making the Neovim-only workflow faster (optional)

Pixi downloads can be sped up by persisting Pixi’s cache in a Docker volume.

Create a cache volume

```bash
docker volume create mojo-playground-pixi-cache
```

## Workflow B: VS Code Dev Containers

1. Clone the repo:

```bash
git clone <repo-url>
cd <repo>
```

2. Open in VS Code:

```bash
code .
```

3. Choose “Reopen in Container”

On first create, the devcontainer runs:

- `pixi install`
- `pixi run mojo --version`
  Then use the integrated terminal to run commands (or VS Code Tasks if configured).
