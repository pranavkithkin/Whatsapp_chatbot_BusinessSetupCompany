# ⚡ **QUICK PRODUCTION SETUP**
## 5-Minute Reference Card

---

## **YOUR WEBHOOK URL**

```
https://n8n.trart.uk/webhook/whatsapp-production
```

**Copy this - you'll need it in Meta!**

---

## **YOUR VERIFY TOKEN**

```
BSETUP_PROD_2024_SecureToken_xyz123
```

**Create your own secure token and use it consistently!**

---

## **3 SIMPLE STEPS TO GO LIVE** 🚀

### **STEP 1: Import Workflow (2 minutes)**

1. Open n8n
2. Click **"Import from File"**
3. Select: `UAE_Business_Setup_Chatbot_PRODUCTION.json`
4. Click **"Activate"** toggle (top right)
5. Click on **"WhatsApp Webhook"** node
6. Copy the webhook URL shown

---

### **STEP 2: Configure Meta (2 minutes)**

1. Go to: https://developers.facebook.com/apps/
2. Select your app → **WhatsApp** → **Configuration**
3. Find **"Webhook"** section → Click **"Edit"**
4. Enter:
   - **Callback URL:** `https://n8n.trart.uk/webhook/whatsapp-production`
   - **Verify Token:** `BSETUP_PROD_2024_SecureToken_xyz123`
5. Click **"Verify and Save"** → Should see ✅ "Verified"
6. Scroll to **"Webhook fields"** → Click **"Manage"**
7. Check: ✅ **messages** → Click **"Save"**

---

### **STEP 3: Test (1 minute)**

1. Send WhatsApp message to: **+971 55 985 6798**
2. Message: `"Hello"`
3. Should receive AI response within 5 seconds
4. Check n8n execution history → Should see successful run
5. Check Google Sheets → Should see new lead logged

**✅ If all 3 work → YOU'RE LIVE!**

---

## **ENVIRONMENT VARIABLES NEEDED**

Add to n8n (Settings → Environment Variables):

```bash
WEBHOOK_VERIFY_TOKEN=BSETUP_PROD_2024_SecureToken_xyz123
WHATSAPP_PHONE_NUMBER_ID=762171880324039
GOOGLE_SHEETS_LEADS_ID=1tBmut86wQXS22bzvXny5DnerXxKWozqr_KbQEvBE8rQ
```

---

## **CREDENTIALS CHECKLIST**

Make sure these are configured in n8n:

```
□ WhatsApp API (BSETUP PRODUCTION)
□ Google Sheets OAuth2
□ OpenAI API
□ HTTP Header Auth (for audio)
```

---

## **TROUBLESHOOTING**

### **Webhook Verification Fails**

```
✓ Is n8n workflow ACTIVE?
✓ Is webhook URL correct?
✓ Does verify token match exactly?
✓ Is SSL certificate valid?
```

### **Messages Not Received**

```
✓ Is "messages" field subscribed in Meta?
✓ Is phone number connected to webhook?
✓ Is webhook status "Active" in Meta?
✓ Check n8n execution history for errors
```

### **AI Not Responding**

```
✓ Is OpenAI API key valid?
✓ Are OpenAI credits available?
✓ Check n8n logs for errors
```

---

## **WHAT CHANGED FROM TEST?**

**Only 3 nodes added at the start:**

1. **Webhook** (replaces WhatsApp Trigger)
2. **Is Verification Request?** (required by Meta)
3. **Normalize Webhook Data** (formats data)

**Everything else is EXACTLY THE SAME!**

---

## **KEY URLS**

```
Meta Developer Console:
https://developers.facebook.com/apps/

WhatsApp Manager:
https://business.facebook.com/wa/manage/phone-numbers/

Your Google Sheets:
https://docs.google.com/spreadsheets/d/1tBmut86wQXS22bzvXny5DnerXxKWozqr_KbQEvBE8rQ

Your n8n:
https://n8n.trart.uk
```

---

## **DAILY MONITORING**

```
□ Check Meta quality rating (stay GREEN)
□ Review n8n execution history
□ Check for failed messages
□ Verify Google Sheets logging
```

---

## **EMERGENCY ROLLBACK**

If something breaks:

1. **Meta:** Webhook → Remove
2. **n8n:** Deactivate production workflow
3. **n8n:** Activate old test workflow
4. **Fix issue** → Re-deploy

---

## **SUPPORT DOCS**

- **Detailed Guide:** `WEBHOOK_MIGRATION_GUIDE.md`
- **Full Checklist:** `PRODUCTION_DEPLOYMENT_CHECKLIST.md`
- **Comparison:** `TEST_VS_PRODUCTION_COMPARISON.md`

---

## **TEST MESSAGE IDEAS**

```
1. "Hello"
2. "I want to setup a business in Dubai"
3. "What are your prices for DMCC?"
4. "I need 2 visas and have AED 25,000 budget"
```

---

## **SUCCESS INDICATORS**

```
✅ Webhook verified in Meta
✅ Test message received and responded
✅ AI response appropriate
✅ Lead logged in Google Sheets
✅ n8n execution shows success
✅ Quality rating stays GREEN
```

---

**🎉 That's it! You're production-ready!**

**Questions? Check the detailed guides above.**


