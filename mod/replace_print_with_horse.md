Mod Name: replace_print_with_horse

This mod’s purpose is to rename the `print` command to `horse` in Python so that programs use `horse(...)` for output. At a high level, it’s a language-syntax swap: a single, recognizable keyword change that makes src-horse’s identity visible in everyday code. The mod name is `replace_print_with_horse`.

To implement this, the patches would adjust the parser/grammar to accept `horse` where `print` is currently recognized, and update any related compiler or bytecode paths so `horse` maps to the same runtime behavior as `print`. The headline representation is “Replace `print` with `horse` as the standard output command.” This keeps the behavior identical while changing the surface syntax to signal the language variant.

Reasons it could work:
1. The change is localized and easy to test because it targets a single well-known command.
2. Backwards compatibility can be preserved by optionally supporting both names during a transition.
3. The runtime behavior remains the same, reducing risk in the execution pipeline.

Reasons it might not work:
1. If `print` is deeply embedded in tooling or language grammar rules, the change could be more invasive than expected.
2. Existing codebases and educational materials would break or confuse users without a compatibility mode.
3. Some parts of the interpreter may treat `print` specially (historical or internal), making a direct rename tricky.
