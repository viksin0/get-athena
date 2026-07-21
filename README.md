# Get Athena

This public gateway explains how Microsoft employees can request access to the private **Athena** VS Code extension and starter workspace. Athena source, release artifacts, and workspace content remain in Microsoft EMU repositories.

- **`index.html`** — the hosted landing page (download button + step-by-step install guide).
- **Access request** — [submit the public issue form](https://github.com/viksin0/get-athena/issues/new?template=access-request.yml) while signed into a personal GitHub account, then provide your Microsoft EMU username.
- **`README.md`** — this guide (also renders on GitHub).

## Hosting on GitHub Pages

Pages is enabled on this repo, serving from the `main` branch root:

`https://viksin0.github.io/get-athena/`

Anyone can read the request instructions. Only approved Microsoft EMU identities can open the private repositories or download the current VSIX.

> **Request access:** Microsoft EMU accounts cannot create public issues outside the enterprise. Sign into a personal GitHub account to submit the [Athena access request](https://github.com/viksin0/get-athena/issues/new?template=access-request.yml), then provide only your managed GitHub username and a non-confidential use case.

---

## ⚠️ Athena is not a standalone extension

The VSIX is only the UI shell + local bridge. The actual intelligence is a **fleet of AI agents** (`@athena`, `@design`, `@research`, `@reflect`, `@figma`) that:

- live as Markdown agent files inside the **Athena starter workspace**, and
- are executed by **GitHub Copilot Chat in Agent Mode**.

So "download and run" requires three things together: **the VSIX**, **the workspace open in VS Code**, and **an active GitHub Copilot subscription**.

## Prerequisites

| Requirement | Why |
| --- | --- |
| VS Code 1.92+ | Extension engine target. |
| GitHub Copilot + Copilot Chat extensions (signed in) | Agents run on your Copilot models — no separate API key. |
| Copilot **Agent Mode** | `@athena` can only spawn the fleet in Agent Mode. |
| Access to the private Athena starter repo | Either setup path clones the same agents, rules, and `designs/` workspace. |

## Step-by-step

1. **Request access** — sign into a personal GitHub account and submit the [Athena access request](https://github.com/viksin0/get-athena/issues/new?template=access-request.yml) with the Microsoft EMU username that should receive access.

2. **After approval, download the extension** from the [private Athena release repository](https://github.com/viksin_microsoft/get-athena/releases/latest), then install the VSIX:
   - UI: Extensions view → `···` → **Install from VSIX…**, or
   - Terminal: `code --install-extension athena-*.vsix`

3. **Reload the window** — Command Palette → *Developer: Reload Window*. The Athena icon appears in the Activity Bar; the bridge starts on `localhost:7878`.

4. **Get access to the private starter repo**, then choose exactly one setup path:
   - **Guided (recommended):** on the hosted page choose **Guided setup** → **Continue setup in VS Code**, or run **Athena: Set Up or Update Workspace**. Select a parent folder when prompted. Athena creates `athena-starter/`, clones the repo, runs `npm ci`, and opens it.
   - **Terminal:** run the equivalent commands yourself:
     ```sh
   git clone https://github.com/viksin_microsoft/athena-starter.git
     cd athena-starter
     npm ci
     code .
     ```

5. **Open Athena** — click the Athena Activity Bar icon → **Open in VS Code Panel** (or **Open in Browser**). The **Get Started** page loads automatically on first run.

6. **Switch Copilot Chat to Agent Mode** — in Copilot Chat, change the mode picker from *Ask* to *Agent*. (Most common mistake: stages do nothing in *Ask* mode.)

Do not run both workspace paths. The extension reports the real local state:

- **Set Up Athena Workspace** — no Athena workspace is open.
- **Finish Workspace Setup** — the repo exists but `npm ci` still needs to run.
- **Check for Workspace Updates** — setup is complete; updates use a safe `git pull --ff-only` flow.

## Running the agents

- **Create a design** — `＋ Create new design`, describe the blade in a sentence, pick an outcome.
- **Run a stage** — use the stepper: Research → Ideation → Generation → Validation → Learning.
- **Run everything** — `▶ Run Auto` chains all five stages.
- **Chat / slash commands** — type `/research`, `/generate`, etc. directly in the Athena chat.

Full docs: the built-in **📖 Get-Started Docs** (book icon in the panel), mirroring `KB/docs/get-started/`.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| Empty panel / "No designs found" | Open the `athena-starter` folder (it contains `designs/`) as your workspace. |
| Running a stage does nothing | Copilot Chat is in *Ask* mode — switch to *Agent*. |
| "Open in Browser" → *Not found* | A stale window holds port 7878; reload the VS Code window once. |
| Model errors / not signed in | Confirm Copilot + Copilot Chat are installed and signed in. |

## Rebuilding / updating the VSIX

The private `Portal-playground` repo owns the release workflow. With clean sibling checkouts of `athena-starter` and `get-athena-site` beside it, run:

```sh
cd athena-extension
npm run release:distribution
```

This audits and packages the VSIX, validates its payload, synchronizes the canonical starter workspace, archives the previous public VSIX, publishes the new artifact and provenance, stamps version references, and verifies both sibling repositories. Review and commit each repository separately; GitHub Pages redeploys when this repository is pushed.
