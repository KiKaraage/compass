# Homebrew formula (staging for ublue-os/homebrew-experimental-tap)

`compass.rb` is the formula as submitted to the tap. Audit constraints
learned the hard way — keep them true when the install changes:

- Depends order is load-bearing: `:build` deps first (alphabetical), then
  `depends_on :linux`, then normal deps (alphabetical). Anything else fails
  `FormulaAudit/DependencyOrder` — note a `:linux` after the normal deps
  reads naturally (ydotool does it with build-only deps) but fails audit
  once normal deps exist.
- The install must run `cargo install ... *std_cargo_args`;
  `FormulaAudit/Text` refuses `cargo build`. The multi-binary layout (helpers
  in `libexec/compass`, data under `share/compass`) is therefore spelled out
  here, mirroring `scripts/packaging/install-rust-engine.sh`, which stays the
  canonical list of what an install contains.
- `node` is a normal (not `:build`) dependency: the installed engine runs
  the extension runtime bundle with the distribution's Node, the way the
  Arch and Nix packages do.
- The `sha256` belongs to the GitHub-generated tag tarball and exists only
  after the tag is pushed; `scripts/bump_version.sh` moves the url and
  resets the hash to the placeholder, so filling it is part of the release.
