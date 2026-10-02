# QGE Rate Portal — GitHub-Ready Package

**Qaiser Group of Electronics — Salesman Rate Portal**

This package is arranged for GitHub Pages. The included GitHub Actions workflow automatically installs the project dependencies, builds the Vite/React portal, adds `rates.xlsx` to the published site, and deploys it to GitHub Pages.

## Upload to GitHub

1. Create a new GitHub repository, for example `qge-rates`.
2. Upload **everything inside this package** to the **root** of that repository. Keep `.github/workflows/deploy.yml` in the same path.
3. In GitHub go to **Settings → Pages** and set **Source = GitHub Actions**.
4. Open **Actions** and make sure the workflow **Deploy QGE Rate Portal to GitHub Pages** runs successfully after your push to `main`.
5. The normal project-site address is `https://YOUR-USERNAME.github.io/qge-rates/`.

## Important: replace the included Excel file

The supplied `rates.xlsx` is only a **setup template**. It is not real selling-rate data.

Before giving the website to salesmen, replace it with your actual monthly rate file and keep the filename exactly:

`rates.xlsx`

The included workflow copies the root `rates.xlsx` into `dist/` before deployment, so every committed Excel update is published with the next workflow run.

## Excel columns

Use these headings (the portal also accepts several common heading variations):

| Company | Product | Model | Cash Rate | Installment Rate | Fix Rate | Remarks | Month | Year |
|---|---|---|---:|---:|---:|---|---|---|
| HAIER | LED TV | H43K800FX | 104500 | 127000 | 114000 | Free wall mount | September | 2026 |

`Fix Rate` is used for HAIER cards. `Remarks`, `Month`, and `Year` can be filled according to your monthly rate sheet. Old months may remain in the workbook; the portal is designed to show the latest period for each model.

## Monthly rate update

Open `rates.xlsx` in Excel, update the latest rates and period, save it, then replace the `rates.xlsx` file in the GitHub repository and commit. GitHub Actions will rebuild and redeploy the portal automatically.

## Login

The current project keeps the login values as SHA-256 hashes in `src/config.ts`, not as plain-text credentials.

This remains a **client-side/static login gate**. GitHub Pages sites are publicly accessible, and files published with the site can potentially be fetched directly. Do not use this setup for highly confidential rate data without adding a server-side authentication/backend layer.

## Local development

```bash
npm ci
npm run dev
```

Production build:

```bash
npm run build
```

The production build is generated in `dist/`.
