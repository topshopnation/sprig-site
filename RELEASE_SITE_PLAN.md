# Release site plan (draft, 2026-10-04)

Branch release-site, worktree ~/sprig-site-release-site. Nothing pushed, nothing deployed.

Basis: fetched 2026-10-04 with curl: https://heysprig.com/, /privacy, /terms, /data-retention, /contact, /screenshots/accounts.png (HTTP head only), https://topshopnation.com/, https://topshopnation.com/sprig/. Repo files read in full (5 pages). Code read: ~/sprig-ios Sources (ReportsView, Budget/, LowPointAlerts, SettingsView, PaywallView, SubscriptionTerms, PrivacyInfo.xcprivacy, NameWithIcon, AppLockView, SprigFinance.entitlements, project.yml), ~/sprig-worker/src/index.js (storage calls only), audit/LOCKED.md (covers matching behaviour only, nothing on site claims). Not fetched: the topshopnation.github.io repo itself (Google Drive folder hung on read), Apple pages (see Apple section, read by a helper through a summarizer).

## Found today

1. Live site is BEHIND the local main branch. Commit 425c2b2 (privacy: iCloud Keychain, notification token, subscription hash, Plaid user ID) is committed locally but not pushed. Live privacy.html and data-retention.html still show the older text (Last updated Aug 23). Live index, terms, contact equal the repo.
2. Header has a Sign in button that does nothing (placeholder, no web app). Removed.
3. Home page claims that are no longer true: "No budgets to configure" (Budget exists); "Smart clean merchants" and "Investments included" were fine but thin. Subtitle did not mention budgets or insights.
4. "Coming to the App Store" note: removed. Replaced by a hidden App Store badge block that appears only when APP_STORE_URL is set in index.html.
5. Screenshots are 322x700, from Aug, no Budget screen, old Insights. They are interim only.
6. Contact page shows no email address. Fixed: shows support@heysprig.com.
7. Terms: "creating an account", "delete your account", "read-only viewer", and a subscription section that deferred to the App Store. Rewritten generically (auto-renews through Apple, cancel in Apple Account settings, price shown in app and on the listing, nothing priced on the page). Added the Apple standard EULA link (URL from Apple help text, not fetched).
8. Privacy: said the app collects no email, but the website contact form collects email and message through Web3Forms. Added. Added the App Privacy label match (Other Financial Info and Device ID, linked, app functionality, no tracking, matches PrivacyInfo.xcprivacy), device name in iCloud, Face ID lock, backups.
9. Data retention table lacked the subscription hash and Plaid user ID that the privacy page lists, and the 90 day notification address expiry (PUSH_DEVICE_TTL in worker). Added.
10. "CoreForge LLC" appears on every page. Changed to "CoreForge" per Kevin. Legal pages may need the full legal name (question for Kevin).
11. topshopnation.com already links to Sprig (home card href /sprig/). The /sprig/ page says "In development, not on the App Store yet" and has a founder-ish story paragraph. Draft replacement in topshopnation-draft/sprig/index.html.

## Claims checked against code (written on the site)

- Plaid bank linking: PlaidLink.swift, LinkKit in project.yml. Yes.
- Exact running balance, registers: RegisterView. Yes.
- Budget with groups and categories: Sources/Budget (BudgetGroupView, group lines). Yes.
- Insights: Spending, Income, Cash Flow pages; Donut and Bars; Category, Group, Merchant lens (ReportsView.swift). A "Table" view was NOT found, so the site does not say table.
- Low balance warnings: LowPointAlerts, setting "Low balance warning". Yes, local notifications.
- iCloud sync across devices: CloudKit entitlement, Sync2 engine. Yes (the page says synced through your own iCloud).
- Face ID lock: AppLockView. Yes.
- Investments: InvestmentsView, brokerage via Plaid. Yes.
- NOT in code, so NOT on site: merchant logos (ReportsView.swift line 954: "logos come later"; NameWithIcon draws emoji for categories only); any web login; Mac app; trial; price.
- Income groups: Budget refuses Income as a budget line; Insights has groups on the Income page. Not claimed separately.

## Contact addresses (Gmail evidence, 2026-10-04; MX is Namecheap forwarding)

- support@heysprig.com: receives mail (Supabase mails Sep 17 and Sep 28). SHOWN.
- website@heysprig.com: receives contact form notifications (Web3Forms). Internal, NOT shown.
- kevin@heysprig.com: receives and sends (Plaid cases). Personal, NOT shown.
- privacy@heysprig.com: no mail seen, forwarding unknown. NOT shown (commented option in contact.html).
- Not checked: Namecheap forwarding rules (needs Kevin's dashboard).

## Apple requirements (read 2026-10-04 via helper summarizer; paraphrased)

- Privacy Policy URL required: https://developer.apple.com/help/app-store-connect/reference/app-information (verified). Support URL and Marketing URL not required, but supply Support URL = https://heysprig.com/contact.
- Subscription terms in app and listing, links to Terms and Privacy: https://developer.apple.com/app-store/subscriptions/ and review guideline 3.1.2 (verified). App already has Restore, Terms and Privacy links on the paywall (PaywallView.swift).
- Custom EULA optional; standard EULA applies otherwise: https://developer.apple.com/help/app-store-connect/manage-app-information/provide-a-custom-license-agreement (verified).
- Badge rules, fictional data in screenshots: https://developer.apple.com/app-store/marketing/guidelines/ (verified). Badge file download needs Apple's artwork license: Kevin must accept it, file goes at /images/app-store-badge.svg. apps.apple.com link format UNVERIFIED.
- Screenshot sizes (iPhone 6.9 1320x2868, iPad 13 2064x2752, Mac 2880x1800): https://developer.apple.com/help/app-store-connect/reference/screenshot-specifications (verified).
- UNVERIFIED: whether Apple counts a Plaid-linked read-only app as "money management" under guideline 3.2.1(viii); whether policy must match the label by an explicit rule (it should anyway).
- UNVERIFIED: Plaid developer policy on privacy-policy content.

## Page by page

- index.html: header login removed; six cards (accounts, running balance, Budget, Insights, low balance warnings, private); no price; gallery driven by SHOTS in the script; badge hidden. WAITS for App Store URL: set APP_STORE_URL, add badge file.
- privacy.html, terms.html, data-retention.html, contact.html: drafted, comments mark UNSURE spots for the lawyer (legal entity name, Web3Forms retention, subscription record retention, EULA link, Wyoming law).
- topshopnation.com /sprig/: draft replacement, add App Store link on release day. Home card already links; blurb OK. Source repo is topshopnation.github.io in Drive; copy the file over and push only on Kevin's go.

## Gallery: one step to drop in final images

Put Sprig 3 files in screenshots/iphone/ and screenshots/ipad/, list file names in SHOTS in index.html. Mac list stays empty (tab hidden) until the Mac app ships. Final images must show fictional data and a full status bar (Apple rule).

## Also found, not changed

- PENDING_CLOUDKIT_COPY.md in repo is stale (CloudKit now shipped); leave or delete on Kevin's say.
- Terms still say US residents 18+ (unchanged, lawyer to confirm).
- Worker README is stale (Supabase); privacy text was written from worker code, not README.
