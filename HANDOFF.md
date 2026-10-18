# HANDOFF — Fifth

**Generated:** 2026-07-21
**Reason:** 76-fun Thelio to System76 for NVMe repair; work continues elsewhere.
**Repo path on origin machine:** /home/paul/go/src/github.com/PaulSnow/Fifth
**Remote:** https://github.com/PaulSnow/Fifth

## 1. What this repo is

The public release archive of **Professional Fifth**, a complete 32-bit programming environment for
8086/8087-based IBM PCs, designed and built by Paul Snow and Cliff Click in graduate school in the
mid-1980s. Fifth is Forth-adjacent but not Forth: heap-based memory management instead of Forth's
stack discipline, tree-based rather than linear scoping, a separate floating-point stack, lazy
compilation, VGA graphics operators, and a character-graphics UI. The repo holds the shipping DOS
binaries (`Fifth.exe`, the shareware `Fifth_sw.exe`, `fifth.hlp`), the `.fiv` library sources, and
the tutorial PDFs. Per `README.md`, the environment's own source code is presumed lost — this is a
binary-plus-library preservation archive, not a buildable project.

## 2. Current work in flight

The repo has been dormant since its original release; all seven uncommitted items are from a
**2026 effort to get Fifth running again under DOSBox on Linux and exercise the environment
end-to-end.** The work happened in two sessions, February 18–19 and March 6, and none of it is
committed.

- **`dosbox.conf` — the emulation harness (the keystone file).** A complete DOSBox 0.74-3 config
  tuned for Fifth: `machine=svga_s3` and `scaler=normal3x` (Fifth has VGA graphics operators),
  `memsize=16`, `output=opengl`, `captures=capture`, `core=auto`/`cycles=auto`. Its `[autoexec]`
  mounts the repo as drive C: and launches `fifth.exe` directly, so `dosbox -conf dosbox.conf` drops
  straight into the environment. This is the single most valuable untracked artifact — it is the
  reproducible way back into a 40-year-old environment.
- **`capture/` — 46 screenshots** (`dosbox_000.png`, `fifth_000.png` … `fifth_044.png`), all dated
  2026-02-19, written by DOSBox's capture facility into the directory `dosbox.conf` designates. A
  visual record of the environment running: the session that proved the harness works.
- **`showbox.fiv` — a new Fifth program** (2026-02-19), written the same day as the screenshots. A
  minimal smoke test of the character-graphics UI: it builds two NUL-terminated strings byte by byte
  with `c,` ("Fifth!" as a title, "Fifth UI test!" as a body), defines `: showbox 5 5 55 20 4 1
  titlestr bodystr popbox ;`, and immediately executes it — drawing a titled box at coordinates
  (5,5)–(55,20). `popbox` is Fifth's own UI primitive, also used by the tracked `cond.fiv`.
- **`A` and `ADSF` — byte-identical Fifth workspace exports** (2026-03-06, 1,151 bytes each). Both
  are in Fifth's `-*-`-delimited module format (same as the tracked `.fiv` files) and contain two
  modules: a `search` word that walks the `names` array comparing with `str=`, computes a file
  offset from the parallel offset table, `seek`s and `read`s 3000 bytes of text, locates the menu
  list with `scan`, and displays the entry via `boxit`; plus a `BUFF` module doing
  `create buff 3000 allot`. This is the implementation behind Fifth's help/name lookup. `ADSF` looks
  like an accidental save under a keyboard-mash filename — **decide which one to keep; they are
  identical.**
- **`BOXIT.EXE` and `SEARCH.EXE`** (2026-03-06, 24,864 and 32,416 bytes). These are **standalone
  executables produced by Fifth itself** — `README.md` lists "Exports stand-alone executables" as a
  headline feature, and their names match the two modules in `A`/`ADSF`. Producing them verified
  that Fifth's export path still works under emulation, which is the more interesting result of the
  March session.

Taken together: February proved the environment boots and its UI renders; March proved you can
write a program in it and export a working DOS binary. Neither result is captured in git, and there
are no notes in the repo describing it.

## 3. State at handoff

- **Branch:** master (upstream: origin/master, in sync)
- **Uncommitted changes:** 7 entries, all untracked
  - `dosbox.conf` — DOSBox 0.74-3 config; mounts the repo as C: and autoexecs `fifth.exe`
  - `capture/` — 46 PNG screenshots from the 2026-02-19 session
  - `showbox.fiv` — new Fifth source; `popbox` character-graphics smoke test
  - `A` — Fifth workspace export containing the `search` word and a 3000-byte `buff`
  - `ADSF` — byte-for-byte identical to `A`; almost certainly an accidental duplicate save
  - `BOXIT.EXE` — standalone executable exported from Fifth
  - `SEARCH.EXE` — standalone executable exported from Fifth
- **Unpushed commits:** 0
- **Recent commits:**
```
9c78c59 Merge pull request #1 from larsbrinkhoff/master
f5674eb Make GitHub detect *.fiv as Forth.
fbc2262 Delete test.md
d3428d3 Just a test file
72df449 Update README.md
```
(The repo was effectively dormant between that history and the 2026 work above.)

## 4. Claude setup

No Claude configuration in this repo. There is no `CLAUDE.md`, no `.claude/` directory, and no
`.mcp.json`.

## 5. Directives and conventions

There is **no build system, no test suite, and no CI** — nothing here compiles on a modern host.
`Fifth.exe` and `Fifth_sw.exe` are DOS binaries; `.fiv` files are Fifth source modules loaded and
executed *inside* the running environment. **Nothing below was executed** (disk fault; do not run on
the origin machine).

- **Running the environment:** `dosbox -conf dosbox.conf` from the repo root. The config mounts the
  repo as C: and autoexecs `fifth.exe`, so no manual mount or `cd` is needed.
- **Screenshots:** DOSBox's capture key (Ctrl+F5 by default in 0.74-3) writes PNGs to `capture/`,
  per `captures=capture` in the `[dosbox]` section.
- **Source format:** `.fiv` files use Fifth's module format — a module name, a `!XXXXXXXX` hex
  header, the body, and a `-*-` separator, with `v`/`^` markers for tree navigation (Fifth's scoping
  is tree-based, and the file layout mirrors that). Comments start with `\`, as in Forth. The
  untracked `A`/`ADSF` files use the same format without a `.fiv` extension.
- **Language tagging:** `.gitattributes` contains exactly `*.fiv linguist-language=Forth`, so GitHub
  syntax-highlights the library as Forth (added in commit `f5674eb`).
- **Documentation:** `fifth.hlp` (88 KB, itself written in Fifth), `FifthTutorial.pdf`,
  `FifthCoverPages.pdf`. There is no separate reference manual in the repo.
- **Licensing / provenance:** `README.md` states Fifth predates open source and was distributed as
  shareware; Paul Snow released it here for free redistribution but explicitly **not** as open
  source, because the environment's source is believed lost. New Fifth programs from contributors
  are welcomed (contact address in the README). Keep that distinction intact — the `.fiv` library
  is contributable, the `.exe` environment is a preserved binary.
- **Commit convention:** short plain-English subject lines, no prefixes; history is sparse and
  informal.

## 6. Resuming on another machine

1. `git clone https://github.com/PaulSnow/Fifth` recovers everything committed, including all the
   DOS binaries and the `.fiv` library. **The seven items in §2 are untracked** — copy them across
   (or commit and push) before the Thelio ships. `dosbox.conf` and `capture/` in particular exist
   nowhere else.
2. **`dosbox.conf` will not work as-is.** Its `[autoexec]` section hardcodes
   `mount c /home/paul/go/src/github.com/PaulSnow/Fifth`. Edit that line to the repo's new absolute
   path — this is the first thing to fix, and the only path change needed anywhere in the repo.
3. **Install DOSBox** (`apt install dosbox`, or DOSBox-X / DOSBox Staging). The config targets
   0.74-3; newer forks accept it but may warn on unknown keys.
4. **`mapperfile=mapper-0.74-3.map` lives outside the repo** (normally `~/.dosbox/`) and did not
   ship. Any custom key bindings from the origin machine are gone; DOSBox will regenerate defaults.
   This is cosmetic — the default capture and fullscreen keys will still work.
5. **Credentials on the origin machine were deleted** — `~/.ssh` (keys *and* known_hosts), `~/.aws`,
   `~/.gnupg`. The remote here is HTTPS so no URL change is required, but pushing needs a fresh
   GitHub personal access token, and signed commits will fail until a new GPG key exists.
6. **The `.EXE` files run only under emulation.** `Fifth.exe`, `Fifth_sw.exe`, `BOXIT.EXE`, and
   `SEARCH.EXE` are 16-bit DOS binaries — modern 64-bit Linux cannot execute them directly. DOSBox
   (or dosemu2) is mandatory, not a convenience.
7. **No fonts or document toolchain are needed.** The PDFs are read-only artifacts; the character
   graphics are rendered by DOSBox's own VGA emulation, not by host fonts.
8. Before committing, resolve the `A` / `ADSF` duplication — they are byte-identical, and `ADSF`
   reads as a stray save. Consider also giving both a `.fiv` extension so `.gitattributes` picks
   them up as Forth, and adding a short note recording what the March 6 export session established
   (that Fifth's standalone-executable export still works under DOSBox), since nothing in the repo
   currently says so.
9. The natural next step is more `.fiv` library work — `showbox.fiv` is a one-off smoke test, and
   the README's standing invitation is for contributed Fifth programs.
