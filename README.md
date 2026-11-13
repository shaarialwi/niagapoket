# NiagaPoket
Phone-first pasar malam POS. Plain HTML, CSS and JavaScript; no build dependencies.

## Current functionality
Products, quick sale entry, cash and change, pending QR payments, manual payment confirmation, cancellation, full refund records, daily sales, CSV export, JSON backup/restore, seller QR image upload.

This version has NO TNG API connection. It neither checks a wallet nor transfers/refunds money. A seller must verify receipt in TNG before manually confirming. Sample products are included; no sample transactions are included.

## Put the project in GitHub
1. Create a repository named `niagapoket` in your GitHub account. A private repository is suitable for development.
2. Extract this ZIP. Upload its contents (index.html, README.md, TNG-INTEGRATION.md and .gitignore) to the repository root, not the ZIP itself.
3. Commit the files. Edit index.html locally or in GitHub as you develop.
4. For local preview, run `python3 -m http.server 8080` from this directory and open http://localhost:8080.
5. Use an appropriate application host for the real POS. GitHub stores the source; hosting can be connected to the repository separately. No build step is required for this version; index.html is the entrypoint.

GitHub Pages is not intended or allowed for sites primarily facilitating commercial transactions or commercial SaaS. Do not use Pages for the production POS.
https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits

## Data and moving from the previous URL
Data lives in localStorage on each browser and origin. A different domain does not inherit it.
Before moving: Settings/Tetapan > download JSON backup on the old site.
On the new site, on the same or another device: Tetapan > restore the backup.
Restore replaces existing local records. Back up those records first.
Never commit sales exports, backups, personal QR images, payment secrets or customer information to GitHub.

No sign-in, central database, multi-seller isolation, cross-device syncing or provider payment verification is implemented. A real multi-seller service requires these before rollout.

## Reports
Dates use Asia/Kuala_Lumpur. Pending/cancelled payments are excluded. Full refunds revise the original sale day's totals; this is a sales report, not a settlement/cash-movement ledger. Fees and settlement reconciliation are not implemented.

## Validation already completed
JavaScript syntax; browser testing of pending QR to manual confirmation, cash/change, combined totals; logic checks for pending exclusion, refunds, backup schema and Malaysia date boundaries.

