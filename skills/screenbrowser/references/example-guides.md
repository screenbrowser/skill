# Example: Chatamatic — "Create your first keyword campaign"

Project: target_url `https://app.chatamatic.io/login`, login_success_url_contains `/dashboard`,
variables USERNAME / PASSWORD (a seeded demo account), allow_hosts none (single host app).

## auth guide

```
Go to https://app.chatamatic.io/login
Type {USERNAME} into selector input[name="email"]
Type {PASSWORD} into selector input[name="password"]
Click selector button[type="submit"]
Confirm text "Dashboard" is visible
Click "Accept all"
Wait 1 second
After login, confirm the sidebar link a[href="/campaigns"] exists
```

## main guide

```
Go to https://app.chatamatic.io/campaigns
[CAPTION "In this tutorial, we'll create your first keyword campaign in Chatamatic."] [PAUSE 1.0]
Confirm text "New campaign" is visible

[HIGHLIGHT "text=New campaign"] [CAPTION "Click New campaign to get started."] [PAUSE 0.6]
Click "New campaign"
Confirm text "Listens to" is visible

[HIGHLIGHT "css=#nc-name"] [CAPTION "Give the campaign a name."] [PAUSE 0.4]
Type "Spring promo" into selector #nc-name

[HIGHLIGHT "css=#nc-keyword"] [CAPTION "Choose the keyword people will comment to receive your lead magnet."] [PAUSE 0.4]
Type "SPRING" into selector #nc-keyword

[HIGHLIGHT "text=Create campaign"] [CAPTION "Click Create campaign."] [PAUSE 0.6]
Click "Create campaign"
Confirm text "Campaign created" is visible
Confirm text "Edit campaign" is visible

[TOAST "Campaign created!"] [CAPTION "Your campaign is ready. Next, set up the reply message."] [PAUSE 1.5]
Confirm the URL contains "/edit"
[CLEAR_OVERLAYS] [PAUSE 0.5]
```

This produced a 21-second video for 3.5 credits.
