# Get Athena

This is a self-contained, GitHub Pages–ready site for installing and running the **Athena** VS Code extension. It is intentionally a **separate public repo** so the main **Portal Playground** workspace can stay private.

- **`index.html`** — the hosted landing page (download button + step-by-step install guide).
- **`downloads/athena-0.1.7.vsix`** — the packaged extension file.
- **`README.md`** — this guide (also renders on GitHub).

## Hosting on GitHub Pages

Pages is enabled on this repo, serving from the `main` branch root:

`https://viksin0.github.io/get-athena/`

Anyone who visits sees the instructions, downloads the VSIX, installs it, and then opens the in-app **Get Started** page.

> **Note:** the Athena workspace repo (`Portal-playground`) is private. Viewers need access granted to clone it (step 1 below).

---

## ⚠️ Athena is not a standalone extension

The VSIX is only the UI shell + local bridge. The actual intelligence is a **fleet of AI agents** (`@athena`, `@design`, `@research`, `@reflect`, `@figma`) that:

- live as Markdown agent files inside the **Portal Playground repository**, and
- are executed by **GitHub Copilot Chat in Agent Mode**.

So "download and run" requires three things together: **the VSIX**, **the repo open in VS Code**, and **an active GitHub Copilot subscription**.

## Prerequisites

| Requirement | Why |
| --- | --- |
| VS Code 1.92+ | Extension engine target. |
| GitHub Copilot + Copilot Chat extensions (signed in) | Agents run on your Copilot models — no separate API key. |
| Copilot **Agent Mode** | `@athena` can only spawn the fleet in Agent Mode. |
| Portal Playground repo cloned locally | Agents, rules, and `designs/` are read from your open workspace. |

## Step-by-step

1. **Get access to the workspace, then clone it.** The `Portal-playground` repo is **private** — request access from whoever shared this page (or the repo owner) first. Once granted:
   ```sh
   git clone https://github.com/viksin0/Portal-playground.git
   cd Portal-playground
   code .
   ```
   Then install workspace deps once:
   ```sh
   cd Playground && npm install
   ```

2. **Download the extension** — [`downloads/athena-0.1.7.vsix`](downloads/athena-0.1.7.vsix).

3. **Install the VSIX**
   - UI: Extensions view → `···` → **Install from VSIX…**, or
   - Terminal: `code --install-extension athena-0.1.7.vsix`

4. **Reload the window** — Command Palette → *Developer: Reload Window*. The Athena icon appears in the Activity Bar; the bridge starts on `localhost:7878`.

5. **Open Athena** — click the Athena Activity Bar icon → **Open in VS Code Panel** (or **Open in Browser**). The **Get Started** page loads automatically on first run.

6. **Switch Copilot Chat to Agent Mode** — in Copilot Chat, change the mode picker from *Ask* to *Agent*. (Most common mistake: stages do nothing in *Ask* mode.)

## Running the agents

- **Create a design** — `＋ Create new design`, describe the blade in a sentence, pick an outcome.
- **Run a stage** — use the stepper: Research → Ideation → Generation → Validation → Learning.
- **Run everything** — `▶ Run Auto` chains all five stages.
- **Chat / slash commands** — type `/research`, `/generate`, etc. directly in the Athena chat.

Full docs: the built-in **📖 Get-Started Docs** (book icon in the panel), mirroring `Playground/Get started/`.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| Empty panel / "No designs found" | Open the folder containing `Playground/designs/` as your workspace. |
| Running a stage does nothing | Copilot Chat is in *Ask* mode — switch to *Agent*. |
| "Open in Browser" → *Not found* | A stale window holds port 7878; reload the VS Code window once. |
| Model errors / not signed in | Confirm Copilot + Copilot Chat are installed and signed in. |

## Rebuilding / updating the VSIX

When the extension source changes (in the private `Portal-playground` repo), repackage and copy the new file into this public repo:

```sh
# in the private Portal-playground repo
cd athena-extension
npm run build
npx --yes @vscode/vsce package --allow-missing-repository \
  --baseContentUrl https://example.invalid/athena \
  --baseImagesUrl https://example.invalid/athena

# copy the fresh VSIX into this public repo, then commit + push
cp athena-<version>.vsix /path/to/get-athena/downloads/
```

Then bump the version references in `index.html` and `README.md` and push. GitHub Pages redeploys automatically.
