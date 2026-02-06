# src horse

A lightweight workflow for creating patched language builds by applying a sequence of mods to upstream source. Mods describe changes, patches implement them, and build produces the resulting binaries for the selected language.

**Purpose**
- Track and explain modifications to upstream language sources in `./mod`.
- Generate and apply ordered patches in `./patch` against `./src` (then `./tmp`).
- Build the patched source to produce the final language binaries.

**Terms**
- "src horse": the name of this project and workflow.
- "src": the upstream language source cloned into `./src`.
- "mod": a modification to upstream source, described in a `.md` file in `./mod`.
- "patch": a unified diff stored in `./patch` and applied to `./tmp` in order.
- "build": the built output produced after patches are applied.

**Commands**
- `bin/clone <php|python|nodejs|golang|chromium>`: Clone the latest upstream source for the chosen language into `./src` (run once per language selection).
- `bin/generate <codex|copilot|grok|gemini|cursor|claude|q>`: Read all mods in `./mod` and use the selected LLM tool to produce ordered patches in `./patch`.
- `bin/transpiler <codex|copilot|grok|gemini|cursor|claude|q>`: Use the selected LLM to generate a Node.js transpiler in `./transpiler` based on `./mod` and `./patch`, then run `npm install`.
- `bin/patch`: Copy `./src` to `./tmp` and apply patches from `./patch` to `./tmp` in order. Patches that include a leading `src/` path prefix are applied with the appropriate strip level.
- `bin/build <php|python|nodejs|golang|chromium> [flags...]`: Build the patched source in `./tmp`, pass any flags through to the language build command, and symlink `./horse` to the resulting binary.

**LLM Providers**
- Codex: uses `codex exec` for non-interactive prompts.
- Gemini: uses `gemini -p` for non-interactive prompts.
- Claude: uses `claude -p` for non-interactive prompts.
- Grok: uses `grok-one-shot --prompt` for non-interactive prompts.
- Q: uses `q chat --no-interactive --trust-all-tools` for non-interactive prompts.
- Copilot: uses `gh copilot -- --silent -p` for non-interactive prompts.
- Cursor: the CLI only opens files/folders and does not accept prompts for automated generation.

**Supported Languages and Build Processes**
- PHP: runs `./buildconf --force` (if needed), then `configure` from `./build/php`, then `make` and `make install`; flags are forwarded to `configure`.
- Python: runs `configure` from `./build/python`, then `make` and `make install`; flags are forwarded to `configure`.
- Node.js: runs `configure` from `./build/nodejs`, then `make` and `make install`; flags are forwarded to `configure`.
- Golang: runs `./src/make.bash` inside `./tmp/src`, then copies `./tmp/bin` into `./build/golang/install/bin`; flags are forwarded to `make.bash`.
- Chromium: runs `gn gen` into `./build/chromium/out`, then `ninja -C ... chrome`; flags are forwarded to `gn gen`.

**Patch Order and Naming**
- Patches must be named with a numeric prefix that defines order, like `01_patch_name.patch`.
- Patches are applied in filename order against `./tmp`.

**How To Use**
1. Run `bin/clone <language>` to fetch upstream source into `./src`.
2. Add or update mods as `.md` files in `./mod`.
3. Run `bin/generate <provider>` to create ordered patches in `./patch`.
4. Run `bin/patch` to copy `./src` into `./tmp` and apply patches.
5. Run `bin/build <language> [flags...]` to build and create the `./horse` symlink.
6. Run `bin/transpiler <provider>` to generate the Node.js transpiler in `./transpiler`.

**Repo Layout**
- `bin/`: helper scripts
- `mod/`: mod descriptions
- `patch/`: generated patches
- `src/`: upstream source (cloned)
- `tmp/`: patched working tree generated from `src/`
- `build/`: build output per language (e.g., `build/python/`, `build/php/`)
- `transpiler/`: generated Node.js transpiler and its binary (`./transpiler/transpile`)
