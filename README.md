# ledelint

ledelint checks that each Knave file reads top-down, like a newspaper article. Readers meet broad concepts before details. It runs as a Knave task.

## Install

ledelint is built with Knave 0.21.0 and has no release yet, so pin a commit.

1. Add it to `libraries` in `project.knv`:

   ```knave
   (defproject shop :knave "0.21.0"
     (libraries (ledelint :git "https://github.com/KarlHarnois/ledelint" :rev "<commit>")))
   ```

2. Create `tasks/newspaper.knv`:

   ```knave
   (use ledelint run)

   (deftask ledelint (:path string ".")
     (run (as-str path)))
   ```

   Never name it `tasks/ledelint.knv`: Knave reserves that module path for the library.

3. Run `knave task ledelint`.
