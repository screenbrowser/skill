# Example: Chatamatic — "Create your first keyword campaign"

Project: target_url `https://app.chatamatic.io/login`, login_success_selector `a[href="/campaigns"]`,
variables USERNAME / PASSWORD (a seeded demo account), allow_hosts none (single host app).

## auth guide

```
Go to https://app.chatamatic.io/login
If visible, click "Reject non-essential"
Type {USERNAME} into the field labelled "Email"
Type {PASSWORD} into the field labelled "Password"
Click selector button[type="submit"]
Wait for selector a[href="/campaigns"]
Confirm text "Campaigns" is visible
If visible, click "Reject non-essential"
```

## main guide

```
[HEADLINE "Create a keyword campaign"]
Go to https://app.chatamatic.io/campaigns
Confirm text "New campaign" is visible
If visible, click "Reject non-essential"
[CHAPTER "Create the campaign"]
[CAPTION "Let's create a keyword campaign that sends a link to anyone who comments a word on your Instagram post."] [PAUSE 0.6]
[HIGHLIGHT "text=New campaign"] [CAPTION "Click New campaign."]
Click "New campaign"
Confirm text "What should this campaign do?" is visible
[HIGHLIGHT "text=Reply privately"] [CAPTION "Choose Reply privately, so they get your message in a DM."]
Click "Reply privately"
Click "Continue"
Confirm text "Where it listens" is visible
[CAPTION "Give it a name."]
Type "Workbook giveaway" into selector #nc-name
[CAPTION "Then set the keyword people will comment. We'll use WORKBOOK."]
Type "WORKBOOK" into selector #nc-keyword
[HIGHLIGHT "text=Where it listens"] [CAPTION "Where it listens sets the channel. Instagram comments is the default."] [PAUSE 0.6]
[HIGHLIGHT "text=Create campaign"] [CAPTION "Click Create campaign."]
Click "Create campaign"
Confirm text "Edit campaign" is visible
Confirm the URL contains "/edit"
[CHAPTER "Write the reply"]
[CAPTION "The campaign opens in the editor."] [PAUSE 0.6]
[HIGHLIGHT "text=Connected posts"] [CAPTION "Connected posts is where you choose which posts to watch, once your account is linked."] [PAUSE 0.6]
[CAPTION "The reply goes out as a DM the moment someone comments. Paste your link straight into it."]
Type "Here's your free workbook: https://chatamatic.io" into the field labelled "Reply message"
[CAPTION "The preview shows exactly what the commenter sees."] [PAUSE 1.0]
[CHAPTER "Go live"]
[HIGHLIGHT "text=Save changes"] [CAPTION "Set the status to Active and save."]
Click selector [aria-label="Campaign status"] [role="radio"]:first-child
Click "Save changes"
Wait for the "Campaign updated" message
Confirm text "Campaign updated" is visible
[CONFETTI 1500] [CAPTION "Done. Add your posts and Chatamatic delivers the link every time someone comments WORKBOOK."] [PAUSE 1.0]
[CLEAR_OVERLAYS]
```

This produced a 1:13 video for 16.25 credits.

# Example: an invoicing app — "Send your first invoice"

Written in plain language, without selectors: every step names the thing as it appears on screen. This
is the form to start from. Project: target_url `https://app.example.com/login`, login_success_text
`Sign out`, variables USERNAME / PASSWORD.

## auth guide

```
Go to https://app.example.com/login
If visible, click "Accept all"
Type {USERNAME} into the field labelled "Email"
Type {PASSWORD} into the field labelled "Password"
Click "Sign in"
Confirm text "Sign out" is visible
```

## main guide

```
Go to https://app.example.com/invoices
[CAPTION "Let's send your first invoice."] [PAUSE 0.8]
Confirm text "New invoice" is visible

[HIGHLIGHT "text=New invoice"] [CAPTION "Start with New invoice."]
Click "New invoice"
Confirm text "Customer" is visible

[CAPTION "Pick the customer, then add what you're billing for."]
Type "Northwind" into the field labelled "Customer"
Press Enter
Type "Design work" into the field labelled "Description"
Type "1200" into the field labelled "Amount"

[ZOOM "label=Amount" 1.4] [CAPTION "The total updates as you type."] [PAUSE 0.6]
[ZOOM_OUT]

[HIGHLIGHT "text=Send invoice"] [CAPTION "When it looks right, send it."]
Click "Send invoice"
Wait for the "Invoice sent" message
Confirm text "Invoice sent" is visible

[TOAST "Invoice sent!"] [CAPTION "Your customer has it in their inbox."] [PAUSE 1.0]
[CLEAR_OVERLAYS]
```
