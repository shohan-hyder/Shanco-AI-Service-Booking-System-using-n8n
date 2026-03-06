# Shanco-AI-Service-Booking-System-using-n8n

This project is an n8n workflow that runs a Telegram bot for:
- Home services booking
- Product ordering (home service related products)
- Auto saving bookings to Google Sheets
- Auto sending confirmation email via Gmail

✅ Simple, beginner-friendly, and client-ready.

---

## Deliverables
- Fully functional n8n workflow implementing the Telegram AI booking system
- Configured Telegram bot capable of handling customer conversations
- Google Sheet for product/service listings
- Google Sheet for booking records
- Automated email confirmation system
- Git repository containing:
  - Exported n8n workflow JSON
  - Setup documentation (this README)

---

## What you need (accounts)
1) Telegram (BotFather)
2) n8n (cloud or self-hosted)
3) Google account (Google Sheets + Gmail)
4) Groq API key

---

## Google Sheets setup (must be ready)
You need TWO Google Sheets files:

### 1) Catalog Sheet (Products + Services)
Tab name: `Catalog`

Required columns:
- Product Name
- Description
- Price (BDT)
- Availability
- Service Time

Rules:
- Service Time > 0 = Service
- Service Time = 0 = Product
- Availability “Out of Stock” means do not offer the item

### 2) Booking Sheet (Booking records)
Tab name: `Booking`

Required columns:
- Booking Reference
- Customer Name
- Email
- Product/Service
- Date/Time
- Timestamp

(Recommended extra columns if you want cleaner product orders: Quantity, Item Type)

---

## Step 1: Create Telegram bot (BotFather)
1. Open Telegram → search `@BotFather`
2. Send `/newbot`
3. Set name + username
4. Copy the bot token (keep private)

Optional commands:
`start, services, products, cancel, help`

---

## Step 2: Import workflow into n8n
1. Open n8n → Workflows
2. Click **Import from File**
3. Select:
`Telegram AI Service Booking System with Automated Confirmation.json`
4. Save workflow

---

## Step 3: Create credentials in n8n
Go to **Credentials → Add credential**

### A) Telegram
- Add Telegram credential
- Paste bot token
- Save

### B) Google Sheets
- Add Google Sheets OAuth
- Sign in to Google
- Save

### C) Gmail
- Add Gmail OAuth
- Sign in to the sender Gmail
- Save

### D) Groq API key
In the **HTTP Request** node that calls Groq:
- If you see credential creation there, use Header Auth:
  - Name: Authorization
  - Value: Bearer YOUR_GROQ_KEY

If you don’t see credential options, do the simple method:
- Authentication: None
- Add Headers:
  - Authorization: Bearer YOUR_GROQ_KEY
  - Content-Type: application/json

---

## Step 4: Connect your Google Sheets inside the nodes
After importing, open the workflow and update these nodes:

### 1) Product Data Tool (or Catalog reader)
- Set the correct Google Sheet document (your Catalog Sheet)
- Select tab: `Catalog`

### 2) Save Booking to Sheet
- Set the correct Google Sheet document (your Booking Sheet)
- Select tab: `Booking`

---

## Step 5: Activate and run
1. Click **Execute Workflow** once to test
2. Message your Telegram bot: `hello` or `/start`
3. Try product order:
   - “show products”
   - select a product name
   - give name + email
   - give quantity
   - type: `confirm`
4. Check results:
   - Booking row added in Booking sheet
   - Email confirmation received
   - Telegram confirmation message received

When everything is working:
✅ Toggle workflow **Active = ON**

---

## Example Telegram interaction flow (simple)
### Product order
User: show products  
Bot: shows products  
User: chooses product  
Bot: asks name → email → quantity  
Bot: shows summary  
User: confirm  
Bot: booking saved + email sent

### Service booking
User: show services  
Bot: shows services  
User: chooses service  
Bot: asks name → email → preferred date/time  
Bot: summary  
User: confirm  
Bot: booking saved + email sent
