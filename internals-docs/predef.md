Predef
======

Ammonite has the concept of a "Predef": Code you can run before entering the 
REPL, and provides all sorts of useful things:

- Import the implicits necessary for the REPL to run at all (pretty-printing 
  code and types)
  
- Loading "common" modules that you want available: `ammonite-shell` to make the
  REPL behave like a systems shell,
   
- Initialize common definitions *the end-user* of Ammonite desires every time 
  the REPL is opened
  
The Predef itself is made of several parts:

- `hardcodedPredef`: imports the minimal implicits for the REPL to run.
  This is mostly just the implicits necessary for pretty-printing values and 
  types.

- `defaultPredefString`: brings in the default set of imports that 
  Ammonite starts with. This brings in Ammonite Ops' various `|` `|?` `|>`
  pipes, `ammonite.tools` like `grep` `time` and `browse`, implicits to
  add SBT-style syntax for loading stuff from `load.ivy`, and importing 
  everything from the `repl` object. This is loaded by default, but can
  be disabled by a `--no-default-predef` flag, or `defaultPredef = false` to
  `Main`

- The assignment of any arguments to the `debug` entrypoint to local `val`s

- Any `commandLinePredef` string you pass in via `predef = "..."` or `--predef "..."`

- Any predef file, which defaults `~/.ammonite/predef.sc` for the REPL and
  `~/.ammonite/predefScript.sc` for scripts, but can be set manually via
  `--predef-file ...`. This split allows you to e.g. put REPL-specific setup
  code inside `predef.sc`, without it messing up scripts. Both the REPL and
  scripts also run `~/.ammonite/predefShared.sc` for setup code you want to
  apply to both

Unlike script files, anything that is run in the predef is automatically made
available to every executed script without needing to `load.module` or import 
it manually. This includes any files being loaded in the predef itself.
  
