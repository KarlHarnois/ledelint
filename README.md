# ledelint

ledelint checks that each Knave file reads top-down, like a newspaper article. A reader meets the broad concepts first and the details further down. ledelint is written in Knave and runs as a Knave task.

## Install

ledelint is built with Knave 0.21.0. ledelint has no release yet, so pin a commit.

1. Add ledelint to the `libraries` of your `project.knv`:

   ```knave
   (defproject shop :knave "0.21.0"
     (libraries (ledelint :git "https://github.com/KarlHarnois/ledelint" :rev "<commit>")))
   ```

2. Create `tasks/newspaper.knv` with this task:

   ```knave
   (use ledelint run)

   (deftask ledelint (:path string ".")
     (run (as-str path)))
   ```

   Never name the file `tasks/ledelint.knv`: Knave reserves the module path `ledelint` for the library.

3. Run `knave task ledelint`.
