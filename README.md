# ledelint

ledelint is a Knave linter that enforces the newspaper metaphor. It runs as a Knave task.

## Install

ledelint is built with Knave 0.21.0 and has no release yet, so pin a commit.

1. Add it to `libraries` in `project.knv`:

   ```knave
   (defproject shop :knave "0.21.0"
     (libraries (ledelint :git "https://github.com/KarlHarnois/ledelint" :rev "<commit>")))
   ```

2. Create `tasks/newspaper.knv`:

   ```knave
   (use ledelint ->settings print-problems run)

   (deftask ledelint (:path string ".")
     (let (problems (run (->settings :root (str path))))
       (print-problems problems)
       (when (not (empty? problems))
         (exit 1))))
   ```

   Never name it `tasks/ledelint.knv`: Knave reserves that module path for the library.

3. Run `knave task ledelint`.
