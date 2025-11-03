# Quick Start Guide - UAE Business Setup Chatbot

Get your chatbot running in 15 minutes. ⚡

---

## Before You Begin

Have these ready:
- ✅ n8n instance (cloud or self-hosted)
- ✅ WhatsApp Business API account + Phone Number ID
- ✅ OpenAI API key
- ✅ Google account
- ✅ Slack workspace (optional)

---

## Step 1: Create Google Sheet (2 minutes)

1. Open Google Sheets
2. Create new sheet: "UAE Business Setup Leads"
3. Add these headers in Row 1:

```
Timestamp | Full Name | Phone Number | Business Activity | Target Market | Budget Range | Visa Needs | Timeline | Customer Message | AI Response | Lead Quality | Status | Follow Up Date | Next Action | Needs Escalation | Response Source | Message ID
```

4. Copy the Sheet ID from URL:
   ```
   https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit
   ```

---

## Step 2: Import to n8n (3 minutes)

1. Open n8n
2. Go to **Workflows** → **Import from File**
3. Select `UAE_Business_Setup_Chatbot.json`
4. Click **Import**

---

## Step 3: Add Credentials (5 minutes)

### WhatsApp Business API
1. Settings → Credentials → **+ Add Credential**
2. Search "WhatsApp"
3. Select **WhatsApp OAuth2 API**
4. Enter:
   - Access Token: `[Your WhatsApp token]`
   - Phone Number ID: `[Your phone number ID]`
5. Save

### OpenAI
1. Settings → Credentials → **+ Add Credential**
2. Search "OpenAI"
3. Enter API Key
4. Save

### Google Sheets
1. Settings → Credentials → **+ Add Credential**
2. Select **Google Sheets OAuth2 API**
3. Click **Connect my account**
4. Authorize access
5. Save

### HTTP Header Auth (for WhatsApp media)
1. Settings → Credentials → **+ Add Credential**
2. Select **HTTP Header Auth**
3. Name: "Whatsapp Media Download"
4. Header Name: `Authorization`
5. Header Value: `Bearer YOUR_WHATSAPP_TOKEN`
6. Save

---

## Step 4: Assign Credentials (3 minutes)

In the workflow, click each node and assign credentials:

**WhatsApp Nodes:**
- WhatsApp Trigger2 → WhatsApp OAuth2
- Download media2 → WhatsApp API
- Send Response → WhatsApp API
- Send Voice Response → WhatsApp API
- Send Follow-Up → WhatsApp API

**OpenAI Nodes:**
- OpenAI Chat Model → OpenAI
- Transcribe a recording → OpenAI
- Generate audio → OpenAI

**Google Sheets Nodes:**
- Log Lead → Google Sheets OAuth2
- Read Leads → Google Sheets OAuth2
- Update Lead Status → Google Sheets OAuth2

**HTTP Node:**
- Download Audio → HTTP Header Auth

---

## Step 5: Update Variables (2 minutes)

### Environment Variables (n8n Settings)
```bash
GOOGLE_SHEETS_LEADS_ID=your_sheet_id_here
GOOGLE_SHEETS_LEADS_URL=https://docs.google.com/spreadsheets/d/your_sheet_id_here/edit
SLACK_CHANNEL_HOT_LEADS=your_slack_channel_id  # Optional
```

### Phone Number ID
Search for `762171880324039` in workflow and replace with your Phone Number ID in:
- Send Response node
- Send Voice Response node
- Send Follow-Up node

---

## Step 6: Activate & Test (5 minutes)

1. Click **Active** toggle (top right)
2. Workflow is now live! 🎉

### Test It

Send to your WhatsApp number:
```
Hi, I want to start a business in Dubai
```

Expected response within 10 seconds:
```
Hello! Interested in setting up a business in the UAE? 
What type of business are you thinking about?
```

### Test Voice Message

Send a voice note:
```
"I need help setting up an e-commerce company"
```

Bot should reply with voice message.

---

## Troubleshooting

### "No response received"
- Check WhatsApp webhook URL in Meta Business Suite
- Verify WhatsApp credentials in n8n
- Check workflow is **Active**

### "Voice not working"
- Verify OpenAI API key has Whisper access
- Check HTTP Header Auth credential
- Review "Download Audio" node logs

### "Google Sheets not logging"
- Confirm Sheet ID in environment variables
- Check Google Sheets credential permissions
- Verify column headers match exactly

---

## What's Next?

✅ **Test with sample conversations** → See `Sample_Conversations.md`  
✅ **Customize AI responses** → Edit "AI Agent" node system prompt  
✅ **Set up Slack notifications** → Add Slack credential + channel ID  
✅ **Review full docs** → Read `README.md` for advanced configuration  

---

## Quick Reference

### Key Nodes to Customize

1. **AI Agent** - Change consultant name, pricing, questions
2. **Parse AI Response** - Add/modify business data markers
3. **Prepare Lead Data** - Change Google Sheets column mapping
4. **Notify Team (HOT)** - Customize Slack notification format
5. **Human Delay** - Adjust response timing (default: 8 seconds)

### Default Settings

- **Response Delay:** 8 seconds
- **Memory:** 20 messages per customer
- **Follow-Up Schedule:** Every 48 hours
- **Lead Quality:** HOT/WARM/COLD auto-classification

---

## Need Help?

- 📖 Full Guide: `README.md`
- 💬 Sample Conversations: `Sample_Conversations.md`
- 📊 Technical Details: `IMPLEMENTATION_SUMMARY.md`

---

**You're all set!** Start chatting with your UAE Business Setup Bot. 🚀

