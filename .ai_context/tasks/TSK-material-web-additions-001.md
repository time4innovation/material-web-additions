# Task #TSK-material-web-additions-001: Paginierungs-Icons von `md-data-table` sichtbar und klickbar
- **Status:** OPEN
- **Parent-Ticket-ID:** TKT-t4i-global-utilities-mui-components-002
- **Parent-Ticket-Titel:** Paginierung: Icons der Schaltflächen in Chrome erst nach dem ersten Blättern sichtbar
- **Parent-Ticket-Datei:** `.ai_context/tickets/open/TKT-t4i-global-utilities-mui-components-002/TKT-t4i-global-utilities-mui-components-002.md` (Workspace `t4i-workspace`)
- **Parent-Ticket-Ursprungsprojekt:** `t4i-global-utilities-mui-components` (Ember-Origin-Addon)
- **Betroffener Part (Hier liegt der Task):** `material-web-additions` (externes Paket, Fork `time4innovation/material-web-additions`, npm-Name `@maicol07/material-web-additions`)
- **Weitere Tasks desselben Tickets:** `TSK-t4i-global-utilities-mui-components-009` -> `t4i-global-utilities-mui-components` (Ember-Origin-Addon); `TSK-ecosoil-cms-ember-004` -> `ecosoil-cms` (Ember-Projekt, ab E2b)
- **Übergabe (Zustand, Entscheidungen):** `.ai_context/tickets/open/TKT-t4i-global-utilities-mui-components-002/TKT-t4i-global-utilities-mui-components-002_UEBERGABE.md` (Workspace `t4i-workspace`)

## Kontext
- **Betroffene Pfade und Abhängigkeiten:** `data-table/internal/data-table.ts`, `data-table/internal/_data-table.scss`. Bezogen per `github:time4innovation/material-web-additions` (Branch `main`) vom Ember-Origin-Addon `t4i-global-utilities-mui-components` und vom Ember-Projekt `ecosoil-cms`; pnpm baut das Paket beim Bezug (`prepare` → `pnpm run build`), die gebauten `.js` sind gitignoriert. `md-icon-button` stammt aus dem Paket `material` (material-esm).
- **Stand beim Start (Commit des Parts):** `Commit: 98191b3e5e89be8561932b5049c3673e8884f310` | *Datum: 2026-11-18 12:40*
- **Branch:** `Branch: TKT-t4i-global-utilities-mui-components-002` | *Ausgangs-Commit: 98191b3 (main)* | *angelegt: 2026-11-18 12:40*
- **Basis für die Statistik:** `Basis: 98191b3e5e89be8561932b5049c3673e8884f310` | *Grund: Stand beim Start, kein Ticket-Code im Part*

## Etappen
- [ ] Etappe 2a: Slot der vier Paginierungs-Icons in ein `span` ohne `slot`-Attribut packen, Ersatzinhalt und die vier `@queryAssignedElements` entfernen, `currentTarget` im Klick-Handler; lokaler Build zur Prüfung
- [x] Etappe 2b: Sicht-Test der Icons in Chrome und Firefox bestanden (2026-11-18; Commit `f60be23` auf `main`)
- [ ] Etappe 2d: leere Tabelle „0-0 von 0“, alle Schaltflächen gesperrt; Bezug und Sicht-Test

## Geänderte Dateien (Rückverfolgung, MANDATORY)
- `data-table/internal/data-table.ts` (Etappe 2a): `renderPagination` steckt den Slot jeder Schaltfläche in `<span class="mdc-data-table__pagination-button-icon">`, ohne Ersatzinhalt; die vier `@queryAssignedElements` `pagination…ButtonIcon` entfallen; `onPaginationButtonClicked` nimmt `event.currentTarget`. Warum: `md-icon-button` blendet mit `::slotted([slot])` in Chrome das geflachte, zugewiesene Icon-Element aus, und der Ersatzinhalt verschob die Light-DOM-Knoten in den Shadow-DOM; ein Klick auf das Icon fand die Schaltfläche über `closest` nicht.
- `data-table/internal/_data-table.scss` (Etappe 2a): Klasse `.mdc-data-table__pagination-button-icon` mit `display: inline-flex` für das neue `span`.
- `package.json` (Etappe 2a): Version 1.8.0 → 1.8.1; (Etappe 2d) 1.8.1 → 1.8.2.
- `data-table/internal/data-table.ts` (Etappe 2d): `renderPagination` nimmt bei `custom-pagination` nur `total-rows` (kein Rückfall auf `rows.length`), bei 0 Zeilen ist die erste Zeile 0. Warum: Die Leer-Zeile des Addons („keine Einträge“) ist ein `md-data-table-row` und zählte als Zeile; die leere Tabelle zeigte „1-0 von 1“ mit aktiven Weiter/Letzte (Sicht-Test des Entwicklers, 2026-11-18).
- `pnpm-lock.yaml` (Etappe 2a): Abhängigkeit `material` (`github:material-esm/material#v2.12.4`) nachgetragen, die Commit `c47d776` in `package.json` aufnahm, ohne die Lockdatei nachzuziehen; ohne sie scheiterte `pnpm install --frozen-lockfile` (Entscheidung des Entwicklers, 2026-11-18).
- `.ai_context/tasks/TSK-material-web-additions-001.md`, `commit-message.txt` (Etappe 2a): Rückverfolgung und Commit-Nachricht.

## Arbeitsablauf (Verweise, keine Wiederholung)
- **Freigabe vor jeder Umsetzung:** Kern, Abschnitt 2, Schritt 9.
- **Prüfung:** lokaler Build (`pnpm run build`); Sicht-Test im Browser nur durch den Entwickler (Kern, Abschnitt 5).
- **Dokumentation:** `commit-message.txt` des Parts laufend, Übergabe laufend, `changelog.md` beim Ticket-Abschluss (Kern, Abschnitt 5).

## Stand und Ergebnis
- 2026-11-18: Etappe 2a gebaut (Sitzung a963acf2). `pnpm install` (mit `prepare`) und `pnpm run build` mit Exit 0; gebaute `data-table.js` mit vier `span`, ohne Ersatzinhalt, mit `currentTarget`; `data-table.css.js` mit der neuen Klasse. ESLint des Forks läuft nicht (Konfiguration: `eslint-plugin-wc` ohne Default-Export, schon vorher). Offen: Commit, Push und Merge nach `main` durch den Entwickler, danach Etappe 2b.
- 2026-11-18: E2a gemergt als `f60be23` (`main`); Icons im Sicht-Test bestanden. E2d gebaut auf dem Ticket-Branch (Build Exit 0); offen: Commit, Push, Merge nach `main` durch den Entwickler, danach Bezug und Sicht-Test.
