# ledelint

ledelint checks that each Knave file reads top-down, like a newspaper article. A reader meets the broad concepts first and the details further down. ledelint is written in Knave and runs as a Knave task, locally and in GitHub Actions.

## File layout

ledelint expects each `.knv` file in this order:

1. `use` and `export` forms
2. constants, such as `(def base-url "https://shop.example")`
3. types: the `defstruct`, `defenum`, `deftype` and `deftrait` forms. Their `derive`, `display` and `show` forms belong here too.
4. functions: `main` first, then exported functions, then private functions

```knave
(export order-total main)

(def tax-rate 15)

(defstruct order
  :payment-status payment-status
  :line-items (vec line-item))

(defenum payment-status
  pending
  paid)

(defstruct line-item
  :cents int)

(defn main ()
  (println (order-total (->order :payment-status payment-status/paid :line-items (vec)))))

(defn order-total (order)
  (-> (:line-items order) (map line-item-total) sum))

(defn line-item-total (line-item)
  (:cents line-item))
```

## Rules

| Rule | Reports |
| --- | --- |
| `section-order` | a form that sits below a later section, such as a constant below a type |
| `public-first` | a function above `main`, or a private function above an exported one |
| `type-stepdown` | a type that sits above the first type that uses it |
| `constant-stepdown` | a constant that sits above the first constant that uses it |
| `function-stepdown` | a function that sits above the first function that calls it |
| `signature-adjacent` | a `(type name ...)` signature that is not directly above its definition |

The stepdown rules compare definitions inside one group. Exported types and private types form separate groups, and so do public and private functions. A private function that calls a public function is not reported, because `public-first` decides that order.

ledelint ignores these:

- a self-recursive or mutually recursive call, since no order puts both sides first
- `defmacro` and `defrule`, because Knave reads a macro before its uses
- a name that a parameter, a `let` binding or a `match` pattern binds
- a call to a name in another module

## Install

ledelint is built with Knave 0.21.0. ledelint has no release yet, so pin a commit.

1. Add ledelint to the `libraries` of your `project.knv`:

   ```knave
   (defproject shop :knave "0.21.0"
     (libraries (ledelint :git "https://github.com/<owner>/ledelint" :rev "<commit>")))
   ```

2. Create `tasks/newspaper.knv` with this task:

   ```knave
   (use ledelint run)

   (deftask ledelint (:path string ".")
     (run (as-str path)))
   ```

   Never name the file `tasks/ledelint.knv`: Knave reserves the module path `ledelint` for the library.

3. Run `knave task` and check that the list shows `ledelint`.

## Run locally

Run `knave task ledelint`. ledelint checks every `.knv` file under the project root, skipping `target/` and `.git/`.

- To check one folder or file, pass `--path`, as in `knave task ledelint --path src`.
- For each problem, ledelint prints one line in the form `src/shop.knv:12: function-stepdown: <message>`.
- If ledelint finds a problem, it exits with code 1. Otherwise, it prints nothing and exits with code 0.

## Run in GitHub Actions

Add this step to your workflow, after the step that installs Knave:

```yaml
- name: Check the newspaper order
  run: knave task ledelint
```

ledelint prints each problem to the job log. Exit code 1 fails the job.
