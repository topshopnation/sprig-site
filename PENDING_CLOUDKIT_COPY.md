# Pending copy changes: two gates, do not apply either yet

**Status: DO NOT APPLY YET.** There are two independent gates in this file.
They can be released separately, and GATE 1 will almost certainly come first.

Written 2026-08-14. Every "Replace" block below matches the live file as it
stands at that date, so each gate can be applied by search and replace.

---

## GATE 1: the device backup change, blocked on Build 81 shipping

Everything under GATE 1 says the user's data is inside their iPhone backup. As
of 2026-08-14 that is **not true of any build a user can install**.

Verified in `~/sprig-ios`:

- `git log --all -S "includeInBackups"` returns **nothing**. The change exists
  only as uncommitted working-tree edits, across six modified files, in another
  agent's tree. Zero commits contain it.
- HEAD is `d4b4e40`, "Build 80". There is no Build 81 commit.
- `BUILDS.md`'s newest entry is `1.0 (75)`.

Until Build 81 is committed, built, and actually released, every shipped build
still calls `excludeFromBackups` on the live store, so the current live wording
("exists only on your device", "deleting the app removes all of it") **is
accurate and must stay**.

**Release GATE 1 only when:** Build 81 is shipped and installable, confirmed by
a commit in `~/sprig-ios` plus the ASC API showing that build. Not by reading a
memory file, and not by the presence of uncommitted code.

If Build 81 ships **without** the backup change (reverted, or dropped after
testing), delete this GATE 1 section entirely. Do not apply it on the
assumption that it landed.

### G1-1. `privacy.html` section 1

**Replace:**

```html
    <h2>1. Our core privacy principle: your data stays on your device</h2>
    <p>Sprig does not use accounts, sign-in, or a central database. There is nothing tying your identity to your financial data anywhere but your own device. Your transaction history, account balances, and investment holdings are retrieved from your financial institution through Plaid, delivered to your device, and never written to any server or database we operate, not encrypted, not temporarily, not in any form. The connection credential that lets your device retrieve your data is stored only on your device, in Apple's Keychain, the same secure storage iOS uses for passwords. It is not stored on our backend in any form.</p>
```

**With:**

```html
    <h2>1. Our core privacy principle: your data stays with you</h2>
    <p>Sprig does not use accounts, sign-in, or a central database. Your transaction history, account balances, and investment holdings are retrieved from your financial institution through Plaid, delivered to your device, and never written to any server or database we operate, not encrypted, not temporarily, not in any form.</p>
    <p>Your data is kept in two places, and both of them are yours:</p>
    <ul>
      <li><strong>On your iPhone.</strong> The app saves your data on the device so it is there when you open it.</li>
      <li><strong>In your iPhone's backup.</strong> If you back up your iPhone, to iCloud or to a computer, your Sprig data is part of that backup, the same way your Messages and Health data are. This is so you can get your work back if your phone is lost or replaced.</li>
    </ul>
    <p>We do not receive either one. Sprig runs no server that holds your financial data, so there is no copy of it on our side to lose.</p>
    <p>The connection credential that lets your device retrieve your data is stored only on your device, in Apple's Keychain, the same secure storage iOS uses for passwords. It is not stored on our backend in any form.</p>
```

Note the deleted sentence "There is nothing tying your identity to your
financial data anywhere but your own device." Once the data is in an iCloud
backup it is under the user's Apple Account, so that sentence must not survive.

### G1-2. `privacy.html` section 2, the customizations bullet

**Replace:**

```html
      <li><strong>Data you enter on your device:</strong> manual transactions, memos, renamed merchants, categories, and other customizations are stored only on your device.</li>
```

**With:**

```html
      <li><strong>Data you enter on your device:</strong> manual transactions, memos, renamed merchants, categories, and other customizations are stored on your device and are included in your iPhone's backup, as described in Section 1. None of it is stored on any server we operate.</li>
```

### G1-3. `privacy.html` section 6, deletion

**Replace:**

```html
    <p>Because we do not store your financial data on any server, there is nothing to retain or delete on our end when you disconnect a bank or delete the app. Deleting the app from your device removes everything, immediately and completely, since your device was the only place any of it ever lived. When you disconnect a linked institution or delete your data from within the app, we also tell Plaid to release that connection, which stops any further access to that account. Full detail is available in our <a href="/data-retention">Data Retention and Disposal Policy</a>.</p>
```

**With:**

```html
    <p>Because we do not store your financial data on any server, there is nothing to retain or delete on our end when you disconnect a bank or delete the app. When you disconnect a linked institution or delete your data from within the app, we also tell Plaid to release that connection, which stops any further access to that account.</p>
    <p><strong>Deleting the app does not delete everything.</strong> It removes the copy on your iPhone. It does not remove the copy inside any backup you have already made. That backup is in your Apple Account or on your own computer, not with us, so only you can remove it. To remove all of your Sprig data:</p>
    <ul>
      <li><strong>Delete the app</strong> from your iPhone. That clears the copy on the device itself.</li>
      <li><strong>Delete or replace your backups.</strong> Any backup taken while you were using Sprig still holds your Sprig data, and it keeps holding it until that backup is deleted or replaced by a newer one. For iCloud backups, open Settings, tap your name, tap iCloud, then Manage Account Storage, then Backups, and delete the backup for that device. If you back up to a Mac or a PC, delete the backup there.</li>
    </ul>
    <p>If you want to keep backing up your iPhone but leave Sprig out of it, open Settings, tap your name, tap iCloud, then Manage Account Storage, then Backups, choose your device, and turn Sprig off in the list. That stops Sprig from going into future backups. It does not remove Sprig data from backups already taken.</p>
    <p>Full detail is available in our <a href="/data-retention">Data Retention and Disposal Policy</a>.</p>
```

### G1-4. `data-retention.html` section 1, end of the design-principle paragraph

**Replace:**

```html
 so there is no financial data at rest on our systems to retain or dispose of.</p>
```

**With:**

```html
 so there is no financial data at rest on our systems to retain or dispose of. Copies of your data are kept on your own iPhone and in your iPhone's backup. Those copies are yours, they sit on your own device and in your own Apple Account or computer rather than with us, and only you can remove them. Section 4 explains how.</p>
```

### G1-5. `data-retention.html` retention table, the transactions row

**Replace:**

```html
        <td>Your device only</td>
        <td>Until you delete the app or remove the data yourself</td>
```

**With:**

```html
        <td>Your iPhone and your iPhone's backup. Never on any server we operate</td>
        <td>Until you remove it yourself, see Section 4</td>
```

### G1-6. `data-retention.html` section 4

**Replace:**

```html
    <h2>4. On-device data</h2>
    <p>The data that makes up your actual financial picture in Sprig, including transaction history, balances, holdings, and any notes or edits you have made, exists only on your device. Deleting the app from your device removes all of it immediately and completely, since your device is the only place it was ever stored.</p>
```

**With:**

```html
    <h2>4. Your own copies, and how to remove them</h2>
    <p>The data that makes up your actual financial picture in Sprig, including transaction history, balances, holdings, and any notes or edits you have made, is kept in two places. Both belong to you:</p>
    <ul>
      <li><strong>Your iPhone.</strong> The app saves your data on the device so it is there when you open it.</li>
      <li><strong>Your iPhone's backup.</strong> If you back up your iPhone, to iCloud or to a computer, your Sprig data is part of that backup, the same way your Messages and Health data are. This is so you can get your work back if your phone is lost or replaced.</li>
    </ul>
    <p>Neither copy is on a server we operate, and we never receive either one.</p>
    <p><strong>Deleting the app does not delete everything.</strong> It removes the copy on your iPhone. It does not remove the copy inside any backup you have already made. To remove all of your Sprig data:</p>
    <ul>
      <li><strong>Delete the app</strong> from your iPhone. That clears the copy on the device itself.</li>
      <li><strong>Delete or replace your backups.</strong> Any backup taken while you were using Sprig still holds your Sprig data, and it keeps holding it until that backup is deleted or replaced by a newer one. For iCloud backups, open Settings, tap your name, tap iCloud, then Manage Account Storage, then Backups, and delete the backup for that device. If you back up to a Mac or a PC, delete the backup there.</li>
    </ul>
    <p>If you want to keep backing up your iPhone but leave Sprig out of it, open Settings, tap your name, tap iCloud, then Manage Account Storage, then Backups, choose your device, and turn Sprig off in the list. That stops Sprig from going into future backups. It does not remove Sprig data from backups already taken.</p>
```

### G1-7. `terms.html` section 9

**Replace:**

```html
On termination, the data deletion process described in our Privacy Policy applies.
```

**With:**

```html
Deleting the app does not remove the copy of your data held in your iPhone's backups. Our <a href="/privacy">Privacy Policy</a> explains how to remove that.
```

Also bump `terms.html`'s `Last updated` to the release date. It is currently
July 20, 2026 and is deliberately untouched by the 2026-08-14 corrections,
because nothing in `terms.html` changed then.

---

## GATE 2: the iCloud copy, blocked on CloudKit shipping

Everything under GATE 2 describes an encrypted copy of the user's data held in
the user's own private iCloud storage. As of 2026-08-14 that copy **does not
exist**. Verified against `~/sprig-ios` at commit `d4b4e40` (Build 80): no
`CKContainer`, `CKRecord`, `CloudKit` or `NSPersistentCloudKit` reference
anywhere in `Sources/`, and no iCloud entitlement in
`Sources/SprigFinance.entitlements`.

**GATE 2 assumes GATE 1 has already been applied**, because its lists are the
three-place version that includes the backup. If CloudKit somehow ships first,
merge by hand rather than pasting.

### Before applying GATE 2, confirm all four

1. CloudKit is merged and shipping in the build that is going to users, not
   sitting on a branch or in an uncommitted tree. Check this the same way
   GATE 1 says to check Build 81.
2. The container is the **private** database only (`privateCloudDatabase`).
   If it is public or shared, none of this copy is correct and it is also an
   App Store privacy label change. See the label note at the bottom.
3. The data is encrypted **on the device, before it is sent**, and CoreForge
   holds no key that can decrypt it. If any key escrow exists on our side, the
   phrase "we cannot read it or reach it" must come out.
4. Whether an in-app control deletes the CloudKit copy. If one ships, it should
   become step 1 of the deletion list and is much easier for users than the
   Settings path. If it does not ship, leave the Settings path as written.

### G2-1. `index.html` line 165, the "Private by design" card

**Replace:**

```html
        <div class="feature-desc">We never store your financial data. It lives on your iPhone, not on our servers.</div>
```

**With:**

```html
        <div class="feature-desc">Your financial data stays on your device and in your own iCloud account. Sprig never stores it.</div>
```

This is the owner's chosen line and matches the in-app wording exactly.

### G2-2. `privacy.html` opening paragraph

**Replace:**

```html
your financial data lives on your own device, on purpose, and is never stored on any server we operate.
```

**With:**

```html
your financial data lives on your own device and in your own iCloud account, on purpose, and is never stored on any server we operate.
```

### G2-3. `privacy.html` section 1, the two-item list (as left by G1-1)

**Replace:**

```html
    <p>Your data is kept in two places, and both of them are yours:</p>
```

**With:**

```html
    <p>Your data is kept in three places, and all three of them are yours:</p>
```

And insert this as a third `<li>`, after the backup bullet:

```html
      <li><strong>In your own iCloud account.</strong> Sprig keeps a copy in the private iCloud storage that belongs to your Apple Account. It is scrambled on your iPhone before it is sent, so Apple holds only scrambled data. It is not in an account we control, and we cannot read it or reach it.</li>
```

And change `We do not receive either one.` to `We do not receive any of them.`

### G2-4. `privacy.html` section 2, the customizations bullet (as left by G1-2)

**Replace:**

```html
are stored on your device and are included in your iPhone's backup, as described in Section 1.
```

**With:**

```html
are stored on your device, included in your iPhone's backup, and copied in scrambled form to your own iCloud account, as described in Section 1.
```

### G2-5. `privacy.html` section 6 deletion list (as left by G1-3)

Change the lead sentence:

```html
    <p><strong>Deleting the app does not delete everything.</strong> It removes the copy on your iPhone. It does not remove the copy in your iCloud account, and it does not remove the copy inside any backup you have already made. Those are in your Apple Account or on your own computer, not with us, so only you can remove them. To remove all of your Sprig data:</p>
```

Insert this bullet between "Delete the app" and "Delete or replace your backups":

```html
      <li><strong>Delete the iCloud copy.</strong> Open Settings, tap your name, tap iCloud, then Manage Account Storage. Find Sprig in the list and delete its data. Deleting the app does not do this step for you.</li>
```

And add this paragraph directly after the list:

```html
    <p>Turning off iCloud for Sprig, or signing out of iCloud, stops new copies from being saved. It does not delete copies that are already there. You have to delete those yourself using the steps above.</p>
```

If an in-app "delete everything" control ships with CloudKit, put it ahead of
the bullets as the one-step option, and keep the Settings steps below it for
anyone who has already deleted the app.

### G2-6. `data-retention.html` section 1 (as left by G1-4)

**Replace:**

```html
Copies of your data are kept on your own iPhone and in your iPhone's backup. Those copies are yours, they sit on your own device and in your own Apple Account or computer rather than with us, and only you can remove them.
```

**With:**

```html
Copies of your data are kept on your own iPhone, in your iPhone's backup, and in your own iCloud account. Those copies are yours, they sit under your own Apple Account rather than ours, and only you can remove them.
```

### G2-7. `data-retention.html` retention table (as left by G1-5)

**Replace:**

```html
        <td>Your iPhone and your iPhone's backup. Never on any server we operate</td>
```

**With:**

```html
        <td>Your iPhone, your iPhone's backup, and your own iCloud account. Never on any server we operate</td>
```

### G2-8. `data-retention.html` section 4 (as left by G1-6)

Apply the same three edits as G2-3 and G2-5: "two places / Both belong to you"
becomes "three places / All three belong to you", add the iCloud `<li>`, change
"Neither copy is on a server we operate, and we never receive either one." to
"None of these copies is on a server we operate, and we never receive them.",
add the "Delete the iCloud copy" bullet, and add the "Turning off iCloud"
paragraph.

### G2-9. `terms.html` section 9 (as left by G1-7)

**Replace:**

```html
Deleting the app does not remove the copy of your data held in your iPhone's backups. Our <a href="/privacy">Privacy Policy</a> explains how to remove that.
```

**With:**

```html
Deleting the app does not remove the copies of your data held in your own iCloud account and in your iPhone's backups. Our <a href="/privacy">Privacy Policy</a> explains how to remove those.
```

### G2-10. `privacy.html` section 7, the Apple bullet

**Replace:**

```html
      <li><strong>Apple Inc.</strong>: subscription billing (if applicable) and delivery of a content-free notification signal, as described in Section 5</li>
```

**With:**

```html
      <li><strong>Apple Inc.</strong>: subscription billing (if applicable), delivery of a content-free notification signal as described in Section 5, and storage of the scrambled copy held in your own iCloud account as described in Section 1</li>
```

---

## After applying either gate

Bump the `Last updated` date on every file you touched, then re-run the
no-dash check, which is an absolute standing rule of the owner's:

```
python3 -c "
bad=[chr(0x2013),chr(0x2014),chr(0x2012),chr(0x2015)]
for f in ['index.html','privacy.html','terms.html','data-retention.html']:
    for i,l in enumerate(open(f,encoding='utf-8'),1):
        if any(c in l for c in bad): print('DASH',f,i)
print('checked')
"
```

## App Store privacy label note

Apple's definition of "collect" is transmitting data off device **and** making
it accessible to the developer beyond the current session.

- **GATE 1, device backup:** an OS-level backup of the user's own device is not
  developer collection. No label change expected.
- **GATE 2, CloudKit private database:** a private database the developer
  cannot read has historically not counted as collection either, so the
  expected answer is also **no label change**.

That answer flips if GATE 2 confirmation item 2 or 3 fails. A public or shared
database, or any developer-held decryption key, makes this collection of
Financial Info and it must be declared.

Separately, shipping CloudKit requires the iCloud capability and a CloudKit
container on the App ID. That is a provisioning change, not a label change, and
it has to be in place before submission.
