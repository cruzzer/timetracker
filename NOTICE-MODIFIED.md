# This is a modified build of Anuko Time Tracker

**It is not an official Anuko release, and Anuko has not reviewed, endorsed or
published it.**

Anuko International Ltd. ceased development of Time Tracker on 2023-12-28. The
upstream project's final commit is `b6711df`, version `1.22.22.5818`. No further
official releases are expected — see the "Terminal Illness of the Owner" section
of `README.md`.

This build exists so that operators who still run Time Tracker have somewhere to
get security fixes. It is maintained by the community, on a best-effort basis,
with no warranty of any kind.

## What was modified

Base: upstream `b6711df` (`1.22.22.5818`), unchanged except as listed here.

| Area | Change |
|---|---|
| `WEB-INF/lib/I18n.class.php` | Translation keys are resolved by walking the `$keys` array instead of by generating and `eval()`ing PHP source. Four call sites. |
| `WEB-INF/lib/common.lib.php` | Translation key format is validated before the key is looked up. |
| `WEB-INF/lib/ttReportHelper.class.php` | Report user and project id lists are built from validated integers. |
| `WEB-INF/lib/ttFavReportHelper.class.php` | Favorite report lookup takes an integer id. |
| `WEB-INF/lib/ttGroupHelper.class.php` | Checkbox group input that is not an array is rejected. |
| `WEB-INF/lib/ttClientHelper.class.php`, `ttTaskHelper.class.php`, `ttUserHelper.class.php`, `ttTimesheetHelper.class.php`, `ttOrgImportHelper.class.php` | Numeric ids and attributes are cast before use in SQL. |
| `report.php` | Record ids are cast to integers; the mark-approved, mark-paid, assign-invoice and assign-timesheet handlers re-check rights on the server. |
| `charts.php` | Favorite report selection is scoped to the acting user. |
| `client_add.php`, `client_edit.php`, `task_add.php`, `task_edit.php`, `user_add.php`, `timesheet_view.php` | Input validation added or ids cast. |
| `initialize.php` | `APP_VERSION` carries a `+security.N` suffix so a modified build is not mistaken for an upstream one. |

No feature was added, removed or changed. No translation file was edited. The
database schema is untouched, so this build runs against an existing Time
Tracker database with no migration.

## Versioning

This build is `1.22.23.1`. It continues upstream's own numbering scheme from
upstream's final `1.22.22.5818`.

**The version string therefore does not, on its own, tell you that this is a
modified build** — and the page footer renders it next to the Anuko name and
copyright. This file, the release notes and the release tag message are what
record the modification. If you redistribute this further, carry them with it.

## Verification

Each release is checked before tagging:

- Every shipped language file is loaded through both the original and the
  modified `I18n` implementation and the resulting key arrays compared. They are
  byte-identical, so no translation string changes.
- Unit assertions cover the key lookup, the write path and the language-file
  escaping.
- Every changed PHP file is checked with `php -l`.

## Licensing and attribution

Copyright in the original work remains with Anuko International Ltd. Upstream's
copyright notices and license text are retained unaltered in every file. This
file exists to satisfy the requirement, stated both in the per-file license
headers and in `license.txt`, that a redistributed modified version indicate
clearly that the modifications are not the work of the original author.

Note that the project ships two conflicting license statements — the per-file
headers describe a "Liberal Freeware License" while `license.txt` is the Server
Side Public License v1. That inconsistency is upstream's and predates this
build; anyone redistributing further should form their own view of which
applies.
