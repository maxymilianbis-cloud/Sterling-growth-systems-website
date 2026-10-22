# AI first-reply for new enquiries

When a potential client emails you, this replies within about 10 minutes, in your voice. The reply:

- confirms what they asked about
- gives a short, relevant rundown of how Sterling would help
- invites them to book a free 15-minute call

Claude reads each new email and decides whether it is a real business enquiry. Spam, newsletters, invoices, receipts, existing conversations and anything from your own domain are skipped. If Claude isn't confident, the reply is saved as a draft for you to check instead of being sent.

It runs as a Google Apps Script inside your Google Workspace account. Gmail labels show what it did:

| Label | Meaning |
|---|---|
| `AI/Replied` | A reply was sent |
| `AI/Draft for review` | A reply is waiting in Drafts for you to send |
| `AI/Skipped` | Not a new enquiry, so left alone |

## Setup (about 15 minutes)

### 1. Create the 15-minute booking page

In Google Calendar, click **Create → Appointment schedule**:

- **Duration:** 15 minutes
- **Availability:** your hours
- **Video conferencing:** Google Meet

Save it, open **Share → Booking page**, and copy the link.

### 2. Get a Claude API key

Go to https://console.anthropic.com, open **API Keys**, create a key and add a little credit. Each reply costs roughly a cent or two.

### 3. Create the script

1. Signed in as the inbox that receives enquiries, go to https://script.google.com and click **New project**. Name it "Enquiry auto-reply".
2. Delete the sample code and paste in everything from `Code.gs`.
3. Click the gear icon (**Project Settings**). Under **Script Properties**, add:

| Property | Value |
|---|---|
| `ANTHROPIC_API_KEY` | your key from step 2 |
| `BOOKING_LINK` | your booking page link from step 1 |
| `SENDER_NAME` | the name to sign with, e.g. `Max` |
| `AUTO_SEND` | `false` for now, while you test |

### 4. Test it

1. In the editor, choose `testWithSampleEmail` from the function menu and click **Run**.
2. Approve the permissions Google asks for.
3. Open **Execution log** to read the sample reply. If you want a different tone, edit `SYSTEM_PROMPT` in the code.

### 5. Turn it on

1. Choose `install` from the function menu and click **Run**. The inbox is now checked every 5 minutes.
2. For the first few days, leave `AUTO_SEND` as `false`. Replies land in Drafts, so you can check them before anything goes out.
3. When you're happy with them, change `AUTO_SEND` to `true` in Script Properties.

To stop it, run `uninstall`.

## Good to know

- **It waits about 3 minutes before replying.** This keeps replies from arriving suspiciously fast. You can change it with `minAgeMinutes`.
- **Only first contact gets a reply.** It never replies in an ongoing thread, so once you answer, the conversation is yours.
- **Edit the business facts if they change.** The text in `BUSINESS_FACTS` is all it knows about Sterling. It's told not to invent anything beyond it.
- **Emails can't steer it.** Instructions written inside an incoming email are ignored, and the only thing it can do is send one reply in the same thread.
