# ledelint

ledelint is a Knave linter that enforces the newspaper metaphor. It runs as a Knave task.

## Install

ledelint needs Knave 0.21.1 or newer. ledelint has no release yet, so pin a commit.

1. Add it to `libraries` in `project.knv`:

   ```clj
   (defproject shop :knave "0.21.1"
     (libraries (ledelint :git "https://github.com/KarlHarnois/ledelint" :rev "<commit>")))
   ```

2. Run `knave task ledelint`. To lint one directory, run `knave task ledelint --path <directory>`.

## Usage

```text
Checks that each Knave file reads top-down, like a newspaper.

Usage: ledelint [OPTIONS] [PATH]

Arguments:
  [PATH]  The directory to lint [default: .]

Options:
  -h, --help  Print this help
```

## Development

```sh
knave fmt --check
knave lint
knave task ledelint
knave test
```
