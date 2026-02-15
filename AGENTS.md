# Repository Guidelines

## Project Structure & Module Organization
- `config/west.yml` defines the manifest (ZMK core, `zmk-userspace`, `zmk-ls0xxvcom-driver`) and treats this repo as the west config layer.
- `boards/shields/rolio/` holds shield-specific sources: overlays, DTSI fragments, keymap behaviors/macros, layout files, and `rolio.zmk.yml` metadata.
- `build.yaml` drives the GitHub Actions matrix for left/right nice_nano_v2 + nice_view builds; mirror its format when adding boards or snippets.
- `zephyr/module.yml` registers this config as a Zephyr module so DTS overlays and keymaps are picked up during builds.

## Build, Test, and Development Commands
- One-time setup in a fresh workspace: `west init -l config .` then `west update` to pull modules.
- Left half with Studio + display: `west build -p -b nice_nano_v2 -d build/rolio_left -- -DSHIELD="rolio_left nice_view" -DEXTRA_CONF_FILE=../../config/rolio_nv.conf -DSNIPPET=studio-rpc-usb-uart`. (west build -p -b nice_nano_v2 -- -DSHIELD="rolio_left nice_view" -DZMK_CONFIG=/home/hudbrog/zmk-config/config -DZMK_EXTRA_MODULES=/home/hudbrog/zmk-config/ -DEXTRA_CONF_FILE=/home/hudbrog/zmk-config/config/rolio_nv.conf)
- Right half: `west build -p -b nice_nano_v2 -d build/rolio_right -- -DSHIELD="rolio_right nice_view" -DEXTRA_CONF_FILE=../../config/rolio_nv.conf`.
- Flash the currently built side: `west flash` (ensure the matching split half is connected).
- If build state drifts, use `west build -t pristine` on the build dir before rebuilding.

## Coding Style & Naming Conventions
- DTS overlays use 4-space indentation and Zephyr-style braces; align GPIO lists vertically for readability.
- Layer and macro identifiers stay UPPERCASE and are declared near the top of `rolio.keymap`; `___` remains the transparent placeholder.
- Keep shield file names consistent (`rolio_left.overlay`, `rolio_right.overlay`, `rolio.keymap`); add new behaviors to `keymap_behaviors.dtsi` and macros to `keymap_macros.dtsi`.
- Include SPDX headers on new files and preserve existing ASCII layer diagrams and comments for layout clarity.

## Testing Guidelines
- Build both halves before proposing changes and confirm clean logs (no warnings/errors).
- When tweaking layouts or behaviors, toggle between Mac/Windows layers and ensure overlays still compile; consider `-t run` targets only if they exist upstream.
- For display or pointing changes, bench-test on hardware: pair, exercise encoders/scroll macros, and note observed behavior in the PR.

## Commit & Pull Request Guidelines
- Commit messages are short and imperative, often namespaced (e.g., `rolio: adjust encoder`, `build: add studio snippet`); keep each commit focused.
- PRs should summarize intent, list the build commands executed (both halves), and mention any artifact-name changes mirrored in `build.yaml`.
- Link related issues, include screenshots for Studio/display-visible tweaks, and request review once firmware artifacts are attached or referenced.
