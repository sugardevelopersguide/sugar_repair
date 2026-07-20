# sugar_repair

CLI Quick Repair and Rebuild for SugarCRM, without going through the web UI or a working `bin/sugarcrm` console (useful when the console itself is what's broken).

## Install

```
composer global require sugardevelopersguide/sugar_repair
```

Or as a dev dependency of a Sugar codebase:

```
composer require --dev sugardevelopersguide/sugar_repair
```

Either way you get a `sugar_repair` executable on your Composer bin path; run it against any Sugar installation:

```
sugar_repair [--cache] <path to sugar>
```

- No flags: runs `RepairAndClear::repairAndClearAll()` for all modules (database repair, extensions, tpls, js, theme cache, etc.) — equivalent to the admin panel's "Quick Repair and Rebuild".
- `--cache`: also clears the full object cache, rebuilds the autoloader cache, and does a basic warm-up (loads every module's language file and bean, rebuilds the API dictionaries) — equivalent to `bin/sugarcrm admin:qrr --cache`.

Tested against Sugar 25.
