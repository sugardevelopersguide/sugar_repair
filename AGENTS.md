# AGENTS.md

## What this is

`sugar_repair` is a standalone CLI PHP script that runs SugarCRM's Quick Repair
and Rebuild (`RepairAndClear`) from the command line, without going through
the web UI or a working `bin/sugarcrm` console — which matters because it's
sometimes the console itself that's broken and needs a repair to fix it.

It targets current SugarCRM versions (currently verified against Sugar 25).
It is not a Sugar module or plugin; it's meant to be copied/invoked against
any Sugar installation's root directory.

## Compatibility constraints

- Keep the script compatible with whatever minimum PHP version SugarCRM
  itself requires (check the target Sugar instance's own `composer.json` for
  `require.php` — historically 8.2+). Don't introduce syntax from a newer PHP
  version than that without an explicit decision to bump the floor.
- The script relies on Sugar internals (`RepairAndClear`, `LanguageManager`,
  `AdminWork`, the `Sugarcrm\Sugarcrm\Security\*` classes, etc.) that can
  change between major Sugar versions. When bringing this up to date for a
  new Sugar version, prefer cross-checking behavior against how Sugar's own
  `bin/sugarcrm` console entry point and any bundled `admin:qrr`-style
  console command bootstrap themselves, rather than assuming the old
  bootstrap sequence still holds.

## Testing changes

There's no automated test suite. To verify a change:

1. `php -l sugar_repair` for a syntax check.
2. Run it for real against a local Sugar install you have available:
   `php sugar_repair [--cache] /path/to/your/sugar/install`
   (never a production instance, and never commit any real install path
   into this repo — see below).

## Public repo — no private paths or org details

This repository is public. Do not commit real local filesystem paths,
internal hostnames, org/company names, credentials, or other
identifying details about any specific Sugar instance used for testing.
Keep examples generic (e.g. "a local Sugar install" or a placeholder path).

## Code style

Formatted with php-cs-fixer (Symfony-based ruleset, PHP 8.4-migration rules
enabled). If you don't have that exact config handy, match the existing
style by hand:

- 4-space indentation; opening brace of a function/control structure on its
  own line for functions, same line for `if`/`for`/etc.
- `//` comments, not `#`.
- `exit(...)` rather than `die(...)`.
- String concatenation with no spaces around `.`.
- A leading `\` when referencing a global-namespace **class** from inside a
  function body that also has `use` imports (e.g. `\RepairAndClear`,
  `\LanguageManager`) — but not for global functions (`translate(...)`,
  `chdir(...)`, etc.), which are left unqualified.
