# BobbyLoads Direct Shippers - Purchase Automation Setup

## Workflow Flow

```
Stripe Webhook (purchase on bobbyloads.com/direct-shippers)
    │
    ▼
Filter (checkout.session.completed only)
    │
    ▼
Extract Purchase Data (name, email, phone, amount)
    │
    ├──▶ Telegram: Send purchase alert to channel
    ├──▶ GoHighLevel: Create contact tagged as "purchased"
    └──▶ Resend: Email CSV file + thank you message
```

## API Keys Needed

### 1. Stripe (Webhook)
- Go to: https://dashboard.stripe.com/webhooks
- Click "Add endpoint"
- URL: `https://YOUR_N8N_URL/webhook/stripe-webhook`
- Events: Select `checkout.session.completed`
- Copy the **Signing Secret** (starts with `whsec_`)

### 2. Telegram Bot
- Message @BotFather on Telegram → `/newbot`
- Copy the **Bot Token**
- Add the bot to your channel as admin
- Get your **Channel ID** (use @userinfobot or format as `@yourchannel`)

### 3. GoHighLevel
- Go to: Settings → Business Profile → API Keys
- Generate and copy your **API Key**

### 4. Resend
- Go to: https://resend.com/api-keys
- Create and copy your **API Key**
- Verify your sending domain (bobbyloads.com)

### 5. CSV File
- Host your direct-shippers CSV file somewhere accessible (S3, Google Drive direct link, etc.)
- Update the `YOUR_CSV_FILE_URL_HERE` in the Resend node with the direct download URL

## How to Import

1. Open n8n (http://localhost:5678)
2. Click "Add workflow" or the + button
3. Click the three dots menu → "Import from file"
4. Select `stripe-purchase-automation.json`
5. Replace all placeholder values (YOUR_TELEGRAM_CHANNEL_ID, YOUR_GHL_API_KEY, etc.)
6. Set up credentials for Telegram and Resend in n8n's credential manager
7. Activate the workflow

## Placeholders to Replace

| Placeholder | Where | Replace With |
|-------------|-------|-------------|
| `YOUR_TELEGRAM_CHANNEL_ID` | Telegram node | Your channel ID (e.g., `-1001234567890`) |
| `TELEGRAM_CREDENTIAL_ID` | Telegram node | Created after adding Telegram credentials in n8n |
| `YOUR_GHL_API_KEY` | GoHighLevel node | Your GHL API key |
| `RESEND_CREDENTIAL_ID` | Resend node | Created after adding Resend HTTP Header Auth in n8n |
| `YOUR_CSV_FILE_URL_HERE` | Resend node | Direct URL to your CSV file |
| `noreply@bobbyloads.com` | Resend node | Your verified Resend sender address |
