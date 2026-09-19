# PayrollTool

PayrollTool is a browser-based payroll-workflow application for turning attendance, absence, vacation, holiday, resignation, and permission-report workbooks into styled monthly outputs and review summaries.

The UI is a React/Vite application. Payroll calculations run in a Web Worker through Pyodide, using the Python modules in `src/py/`; workbook parsing and downloads happen in the browser with `xlsx` and `file-saver`.

## Capabilities

- Runs the five visible processing stages: permission preparation when needed, detailed calendar, attendance/rules fill, final calendar, and payroll review.
- Accepts `.xls` and `.xlsx` input workbooks through file selection or drag and drop.
- Detects the attendance period and exposes settings for Ramadan windows, lunch rules, special-rule pairs, hour reductions, abbreviations, and permission cutoffs.
- Preserves the supplied styled templates while filling the detailed, final, and review workbooks.
- Produces downloadable detailed and final workbooks, a payroll-review workbook, and a prepared permission workbook when applicable.
- Shows step logs, metrics, warnings, and summary cards without requiring a server.

## Privacy and data handling

Payroll files and generated workbooks are processed in the browser and are not uploaded to an application backend. The first run downloads Pyodide and its Python packages from the configured CDN, so an internet connection is needed for a cold start. Treat browser extensions, CDN dependencies, and the deployed site as part of your trust boundary.

Do not commit real employee reports, payroll outputs, private templates, credentials, tokens, or environment files. The repository contains code and configuration examples only; provide real workbooks at run time.

## Using the web app

1. Open the deployed app or run it locally.
2. Upload the required attendance, absence, vacation/transaction, and public-holiday workbooks.
3. Add the optional resignation workbook if needed.
4. Add either a raw permission-request report or a prepared permission-details workbook. A raw report is prepared first when both are present.
5. Upload the detailed and final styled templates required by the payroll flow.
6. Confirm the detected period and review Settings for month-specific rules.
7. Select **Run Payroll**, monitor Run Log, then download the outputs from the Outputs tab.

The file role is selected by the upload tile, so input filenames do not need to match a fixed name. The template workbooks must still contain the sheets, employee roster, formulas, and formatting expected by the Python processing modules.

## Local development

Requirements: Node.js 18 or newer.

```bash
npm install
npm run dev
```

Useful checks:

```bash
npm run test
npm run build
npm run lint
```

Tests live under `src/**/*.test.ts`. End-to-end workbook comparisons require sanitized test fixtures or private reports kept outside the repository.

## Architecture

- `src/App.tsx` — tabs, uploads, settings, run controls, logs, outputs, and summaries.
- `src/workers/pyodideWorker.ts` — loads the Python runtime and executes the workflow off the UI thread.
- `src/core/` — date, rule, type, and payroll-flow logic shared by the application.
- `src/io/` — workbook reading, input-file classification, and browser file handling.
- `src/py/` — Python workbook-processing stages and permission preparation.
- `src/test/` — unit and smoke tests.
- `.github/workflows/deploy.yml` — static build/deployment workflow when enabled for the repository.

The build is static; no application server is deployed. Pyodide packages and browser assets are loaded at runtime.

## License

See `LICENSE`.
