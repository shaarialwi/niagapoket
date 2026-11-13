# Connecting NiagaPoket to TNG: implementation plan

Status: proposed, not implemented. No production API credentials or confirmed in-person QR contract have been supplied.

## 1. Merchant onboarding
A personal TNG wallet is not an established integration credential. Register the pilot seller as a merchant through the TNG app's Merchant / Individual Traders route where eligible.
https://merchantsupport.tngdigital.com.my/hc/en-us/articles/15599572760601-How-do-I-sign-up-to-be-a-Touch-n-Go-eWallet-merchant
Registration alone does not grant API access.

## 2. Request the correct integration
Use TNG's official payment gateway business page and choose Enquire now for other business setups:
https://www.tngdigital.com.my/business/enterprise/business-solutions/payment-gateway/
Explain that this is an in-person, multi-merchant POS, not only online checkout.
Ask TNG to confirm:
- Whether their in-person merchant-presented DuitNow QR integration supports this use case.
- Whether each independent seller can be onboarded with payments settled to that seller.
- Whether Individual Trader merchants are eligible for integration.
- Whether notifications for existing static QR payments can be exposed; if not, dynamic QR per order.
- Sandbox access, API specification, credentials, callback signature verification, transaction status lookup, expiry and refunds.
- Transaction fees, settlement timings, testing and production approval requirements.
An online TNG payment gateway or Mini Program API is not proof of access to existing static QR transaction notifications. Do not implement undocumented endpoints.

## 3. Proposed architecture
GitHub: source/version control.
Application host: mobile user interface.
Secure backend (for example a Cloudflare Worker): authenticated orders, payment requests, provider callbacks, status queries.
Database: sellers, products, orders, payment attempts, callback deduplication and audit records.
TNG or an approved acquiring partner: payment processing and merchant settlement.

The backend requests a supported payment QR tied to an order. Customer scans and pays. Provider sends a signed callback. Backend validates the signature, seller, order reference, currency and exact amount; records the payment once; the phone fetches the updated status and displays provider-confirmed payment.

Requirements:
- Keep credentials in backend secrets, never index.html, browser storage or a public repository.
- Authorise every request per seller; one seller must never see another's orders or QR.
- Enforce unique provider transaction IDs and idempotent callbacks.
- Treat unknown/timeout status as pending, not success or an automatic retry charge.
- Reconcile via provider status lookup and settlement records; handle late callbacks and reversals.
- Preserve manual confirmations as a distinct audit category, not provider-verified payments.
- Match by unique order/payment reference, not amount alone.
- Verify sandbox duplicate, delayed, wrong-amount, invalid-signature, refund and failure scenarios before production.

## 4. Migration
Introduce authenticated central storage and import local backups with per-seller ownership and deduplication. The current localStorage data is not automatically uploaded or synced. Validate totals before enabling live payment collection.
