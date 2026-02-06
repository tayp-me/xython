# AGENTS.md

This project uses the following terms and conventions:

- "src horse" is the name of this project and workflow.
- "mod" refers to a modification made to upstream language source code in `./src`.
- A mod is effectively a patch.
- Each mod is stored and explained in a `.md` file in `./mod`.
- Each mod references a patch in `./patch`.
- Each patch is applied in order against a `./tmp` copy of the `./src` folder.
- Patch filenames must start with a sequence number like `01_patch_name.patch`.
- "src" always refers to the upstream language source code in `./src`.
- `bin/clone` takes a language parameter and clones the latest upstream release into `./src` for that language.
- "build" means the built output produced after patches are applied.
- `bin/patch` copies `./src` to `./tmp` and patches `./tmp`.
- `bin/build` builds the patched source from `./tmp` and symlinks `./horse` to the built binary for the selected language.
- `bin/clone` and `bin/build` support: PHP, Python, Node.js, Golang, and Chromium.
- `bin/generate` and `bin/transpiler` support: Codex, Copilot, Grok, Gemini, Cursor, Claude, and Q. They use provider-specific non-interactive commands where available; Cursor is interactive-only and will error in automated runs. Copilot uses `gh copilot -- --silent -p` for non-interactive prompts.
- Whenever the system behavior or workflow changes, update `README.md` to reflect it.
- For any request that creates a mod (for example, “make a mod called __”, “create a mod named __”, or similar phrasing), create a new `.md` file in `./mod` using an underscore-style filename plus `.md` (e.g., `my_mod_name.md`). The file should be a 3–4 paragraph LLM-written summary of the mod idea in your own words (not a verbatim copy). It must clearly cover the mod’s purpose, a high-level headline description, the mod’s name, three reasons it could work, and three reasons it might not work. This rule applies to any mod-creation context, regardless of phrasing.
