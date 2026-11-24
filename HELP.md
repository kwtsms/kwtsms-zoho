# kwtSMS for Zoho CRM: setup and help

This guide shows how to install and set up kwtSMS for Zoho CRM, send SMS from CRM records, and automate SMS with workflow rules. It also lists every function the extension adds.

## How it is built

kwtSMS for Zoho CRM is a Zoho CRM extension built on the Zoho Developer Platform (Sigma). It runs fully inside your Zoho CRM:

| Part | What it does |
|---|---|
| Web tab widget **kwtSMS** | Login, settings, status and help, in English and Arabic |
| Custom button **Send SMS** | Opens a send window on Contact, Lead and Deal records |
| 9 Deluge functions | Login, send, validate, refresh and settings (see [Functions](#functions)) |
| CRM variables (namespace `kwtsmsforzohocrm`) | Store the settings; the API username and password are hidden variables |
| Connector `kwtsms_api` | The Deluge functions call the kwtSMS API (https://www.kwtsms.com/API/) over HTTPS |

No external server is used: Zoho CRM talks directly to kwtSMS.

## Requirements

- Zoho CRM **Professional** edition or above (extensions, widgets and custom functions).
- A kwtSMS account with API access. [Create a free account](https://www.kwtsms.com/signup/).
- Your kwtSMS **API username and API password**. These are not your website login. Find them in your kwtSMS account under API.
- At least one Sender ID on your kwtSMS account.

## 1. Install

1. Install **kwtSMS for Zoho CRM** from the Zoho Marketplace.
2. Choose the users or profiles that may use it.
3. Open the new **kwtSMS** tab in Zoho CRM.

## 2. Log in to kwtSMS

1. In the **kwtSMS** tab, enter your **API Username** and **API Password**.
2. Click **Login**. The extension checks the login with kwtSMS and loads your balance, Sender IDs and the countries your account can send to.
3. The status area shows your balance and the last refresh time.

To use another kwtSMS account, click **Logout** and log in again. Logout clears the stored API username and password.

## 3. Settings

| Setting | What it does |
|---|---|
| Sender ID | The default sender name for every SMS. Only Sender IDs on your kwtSMS account are listed. |
| Default country | Added to local numbers without a country code (965 = Kuwait by default). |
| Enable gateway | SMS are sent only when this is on. |
| Test mode | On by default. kwtSMS accepts the message but does not deliver it to the phone, so you can test safely. Turn it off when you are ready to send real SMS. |
| Debug logging | Writes extra details to the function logs, for support. |

Click **Save** after changes. Click **Reload** to refresh the balance, Sender IDs and countries from kwtSMS.

## 4. Send an SMS from a record

1. Open a Contact, Lead or Deal.
2. Click **Send SMS** (under the record's more actions).
3. The number is filled in from the record (Mobile first, then Phone; for a Deal, from its Contact). You can edit it or add more numbers, separated by commas.
4. Type the message. The counter shows the characters and how many SMS pages the message uses (English: 160 characters for 1 page, 153 per page after that; Arabic: 70, then 67).
5. Click **Send**. The result shows the message ID and the balance after sending.

## 5. Automate SMS with workflow rules

Use Zoho CRM workflow rules to send SMS on any CRM event, for example:

- A new Lead is created: send a welcome SMS.
- A Deal stage changes: tell the customer.
- A Contact's birthday: send a greeting.

Steps:

1. Go to **Setup > Automation > Workflow Rules** and create a rule (module, trigger, conditions).
2. Under **Instant Actions**, choose **Function** and pick **kwtsms_send** from the kwtSMS extension.
3. Map the arguments:
   - `mobile`: the phone field, for example the Lead's Mobile.
   - `message`: your text. You can insert merge fields, for example the Lead's First Name.
   - `sender_id`: leave empty to use the default Sender ID from the settings.
4. Save the rule. Keep test mode on until you have checked a few records.

## 6. Keep the balance up to date (optional)

The balance, Sender IDs and countries refresh when you log in or click **Reload**. To refresh them every day, create a schedule in **Setup > Automation > Schedules** that runs the function **kwtsms_sync** once a day.

## Functions

| Function | Arguments | What it does |
|---|---|---|
| kwtsms_login | username, password | Checks the API login, stores it in hidden variables, loads balance, Sender IDs and countries |
| kwtsms_logout | none | Clears the stored login and resets the settings |
| kwtsms_get_config | none | Returns the settings, balance and last refresh time. Never returns the username or password |
| kwtsms_save_config | settings (JSON) | Saves Sender ID, default country and the on/off switches. Refuses username or password |
| kwtsms_send | mobile, message, sender_id | Sends an SMS to one or more numbers (comma separated); sends in batches of 200 numbers |
| kwtsms_validate | mobile | Checks numbers with kwtSMS without sending |
| kwtsms_sync | none | Refreshes balance, Sender IDs and countries from kwtSMS |
| kwtsms_normalize | mobile, default_country | Turns a number into international format: Arabic digits to Latin, removes spaces, +, 00 and leading 0, adds the default country |
| kwtsms_clean_message | message | Removes HTML, emoji and hidden characters that can stop delivery |

`kwtsms_send` returns a result with `status` OK or ERROR. On OK it includes the message ID, the number of numbers sent and the balance after sending.

## Troubleshooting

| Message | What to do |
|---|---|
| Invalid credentials / Authentication failed | Use the API username and password from your kwtSMS account (API section), not your website login. Log in again in the kwtSMS tab. |
| SMS sending is not enabled | Turn on **Enable gateway** in the settings and click Save. |
| No sender ID specified | Choose a default Sender ID in the settings, or pass one to kwtsms_send. |
| Sender ID not listed | Only Sender IDs registered on your kwtSMS account appear. Click Reload after a new one is approved. |
| No valid phone numbers / No numbers have coverage | Check the number format and that your account can send to that country. |
| Insufficient balance | Add credits to your kwtSMS account, then click Reload. |
| Message sent but not received | Check that test mode is off. In test mode, messages are not delivered. |

## Support

- kwtSMS website: https://www.kwtsms.com
- API documentation: https://www.kwtsms.com/api-documentation.html
- Support: https://www.kwtsms.com/support.html
