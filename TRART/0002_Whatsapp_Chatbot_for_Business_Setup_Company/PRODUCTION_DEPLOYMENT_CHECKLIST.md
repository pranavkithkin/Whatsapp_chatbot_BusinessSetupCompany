# ✅ **PRODUCTION DEPLOYMENT CHECKLIST**
## WhatsApp Chatbot - Business Setup Company

---

## **PHASE 1: PRE-DEPLOYMENT** 🔍

### **1. Environment Variables Setup**

Set these in your n8n environment:

```bash
# Required
WEBHOOK_VERIFY_TOKEN=BSETUP_PROD_2024_SecureToken_xyz123
WHATSAPP_PHONE_NUMBER_ID=762171880324039
GOOGLE_SHEETS_LEADS_ID=1tBmut86wQXS22bzvXny5DnerXxKWozqr_KbQEvBE8rQ
GOOGLE_SHEETS_LEADS_URL=https://docs.google.com/spreadsheets/d/1tBmut86wQXS22bzvXny5DnerXxKWozqr_KbQEvBE8rQ

# Optional (for Slack notifications)
SLACK_CHANNEL_HOT_LEADS=C1234567890
```

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **2. Import Production Workflow**

1. Open n8n
2. Click **"Import from File"**
3. Select: `UAE_Business_Setup_Chatbot_PRODUCTION.json`
4. Verify all nodes imported correctly
5. Check for any credential warnings

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **3. Update Credentials**

Verify/update these credentials in n8n:

```
□ WhatsApp API (BSETUP PRODUCTION)
  - Permanent Access Token
  - Phone Number ID: 762171880324039
  
□ Google Sheets OAuth2
  - Connected to correct Google account
  - Has access to Leads sheet
  
□ OpenAI API
  - Valid API key
  - Sufficient credits
  
□ HTTP Header Auth (for audio downloads)
  - Bearer token for Meta API
  
□ Slack OAuth2 (Optional)
  - Connected to workspace
  - Has channel access
```

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **4. Get Webhook URL**

1. Open the workflow in n8n
2. Click on **"WhatsApp Webhook"** node (first node)
3. Click **"Execute Node"** or **"Listen for Test Event"**
4. Copy the webhook URL shown

**Your webhook URL:**
```
https://n8n.trart.uk/webhook/whatsapp-production
```

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

## **PHASE 2: META/FACEBOOK CONFIGURATION** 🔗

### **5. Access Meta Developer Console**

1. Go to: https://developers.facebook.com/apps/
2. Select your WhatsApp app
3. Navigate to: **WhatsApp** → **Configuration**

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **6. Configure Webhook**

In the **Webhook** section:

1. Click **"Edit"**
2. Enter:
   - **Callback URL:** `https://n8n.trart.uk/webhook/whatsapp-production`
   - **Verify Token:** `BSETUP_PROD_2024_SecureToken_xyz123` (must match your env var)
3. **IMPORTANT:** Make sure n8n workflow is **ACTIVE** before next step
4. Click **"Verify and Save"**

**Expected Result:** ✅ "Webhook verified successfully"

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **7. Subscribe to Webhook Fields**

In the **Webhook Fields** section:

1. Click **"Manage"** or **"Subscribe"**
2. Check these fields:
   - ☐ **messages** (REQUIRED)
   - ☐ **message_status** (Recommended)
   - ☐ **messaging_postbacks** (Optional)
3. Click **"Save"**

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **8. Verify Phone Number Connection**

1. Go to: https://business.facebook.com/wa/manage/phone-numbers/
2. Find: **+971 55 985 6798**
3. Verify:
   - ☐ Status: **Connected** (green)
   - ☐ Quality Rating: **High** (green)
   - ☐ Display Name: **Bridgewater Management Consultancies**

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

## **PHASE 3: TESTING** 🧪

### **9. Test Webhook Verification**

1. In Meta Developer Console → WhatsApp → Configuration
2. Find your webhook
3. Click **"Test"** button
4. Check n8n execution history for successful verification

**Expected:** n8n shows execution with verification challenge returned

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **10. Test Text Message Flow**

Send these test messages to **+971 55 985 6798**:

**Test 1: Simple greeting**
```
Message: "Hello"
Expected: Welcome message from AI agent
```
☐ Passed | ☐ Failed

**Test 2: Business inquiry**
```
Message: "I want to setup a business in Dubai"
Expected: Discovery questions about business type
```
☐ Passed | ☐ Failed

**Test 3: Pricing inquiry**
```
Message: "What are your prices for DMCC?"
Expected: AI uses pricing tool and returns DMCC packages
```
☐ Passed | ☐ Failed

**Test 4: Conversation memory**
```
Message 1: "My name is John"
Message 2: "What was my name?"
Expected: AI remembers and says "John"
```
☐ Passed | ☐ Failed

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **11. Test Audio Message (Optional)**

1. Send a voice message to **+971 55 985 6798**
2. Say: "I want to setup a business in Dubai"
3. Check:
   - ☐ Audio transcribed correctly
   - ☐ AI responds appropriately
   - ☐ Response sent back as text

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **12. Test Google Sheets Logging**

1. Send a complete conversation (5+ messages)
2. Open Google Sheets: https://docs.google.com/spreadsheets/d/1tBmut86wQXS22bzvXny5DnerXxKWozqr_KbQEvBE8rQ
3. Verify:
   - ☐ New row added
   - ☐ All fields populated correctly
   - ☐ Lead quality assigned (HOT/WARM/COLD)
   - ☐ Follow-up date set

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **13. Test HOT Lead Notification (If Slack enabled)**

1. Send a message indicating high intent:
   ```
   "I need to setup a trading company in DMCC with 2 visas. 
   My budget is AED 25,000 and I want to start immediately. 
   I have all documents ready."
   ```
2. Check:
   - ☐ Lead marked as HOT in Google Sheets
   - ☐ Slack notification sent to team channel
   - ☐ Notification contains all lead details

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

## **PHASE 4: MONITORING SETUP** 📊

### **14. Configure n8n Error Notifications**

1. In n8n → Settings → Error Workflows
2. Create error notification workflow (optional)
3. Configure email/Slack alerts for failed executions

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **15. Set Up Quality Monitoring**

Create calendar reminders for:

- ☐ **Daily:** Check Meta quality rating (stay GREEN)
- ☐ **Daily:** Review n8n execution history
- ☐ **Weekly:** Sample 20 conversations for quality
- ☐ **Weekly:** Review Google Sheets data accuracy
- ☐ **Monthly:** Analyze lead quality trends

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

## **PHASE 5: GO LIVE** 🚀

### **16. Soft Launch (Week 1)**

**Strategy:**
- Share number with 10-20 trusted contacts only
- Monitor every conversation manually
- Collect feedback
- Fix any issues before full launch

**Metrics to track:**
```
Target Metrics:
□ Total conversations: 50-100
□ Response time: < 30 seconds
□ Error rate: < 5%
□ Quality rating: HIGH (Green)
```

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **17. Full Launch (Week 2+)**

**Prerequisites:**
- ☐ 100+ successful test conversations
- ☐ Quality rating: HIGH (Green)
- ☐ No critical bugs
- ☐ Team trained on escalations
- ☐ All integrations working

**Announce on:**
- ☐ Website (add WhatsApp chat button)
- ☐ Social media
- ☐ Email signature
- ☐ Business cards
- ☐ Marketing materials

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

## **PHASE 6: BACKUP & ROLLBACK** 🔄

### **18. Create Backups**

Save these files securely:

```
□ UAE_Business_Setup_Chatbot_PRODUCTION.json (current)
□ UAE_Business_Setup_Chatbot_FINAL.json (backup with WhatsApp Trigger)
□ All credentials (in password manager)
□ Environment variables (in secure note)
□ Webhook verify token (in secure note)
```

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

### **19. Document Rollback Plan**

**If issues occur:**

1. **Disable webhook in Meta** (immediate stop)
2. **Deactivate n8n workflow**
3. **Investigate issue in test environment**
4. **Fix and re-test**
5. **Re-enable when ready**

**Emergency contact:**
- n8n support: https://community.n8n.io/
- Meta support: https://business.facebook.com/business/help

**Status:** ☐ Not Started | ☐ In Progress | ☐ Complete

---

## **TROUBLESHOOTING QUICK REFERENCE** ⚠️

### **Webhook Verification Fails**

```
□ Check n8n workflow is ACTIVE
□ Check webhook URL is correct
□ Check verify token matches exactly
□ Check SSL certificate is valid
□ Test URL in browser
□ Check n8n logs for incoming requests
```

---

### **Messages Not Received**

```
□ Check webhook fields subscribed (messages)
□ Check phone number connected to webhook
□ Check Meta webhook status is "Active"
□ Send test event from Meta dashboard
□ Check n8n execution history
□ Verify phone number ID in credentials
```

---

### **AI Not Responding**

```
□ Check OpenAI API key is valid
□ Check OpenAI credits available
□ Check AI Agent node configuration
□ Check system prompt is correct
□ Review n8n execution logs for errors
```

---

### **Google Sheets Not Logging**

```
□ Check Google Sheets credentials
□ Check sheet ID is correct
□ Check sheet name is "Leads"
□ Check permissions on sheet
□ Test Google Sheets node manually
```

---

## **FINAL CHECKLIST** ✅

Before announcing publicly:

```
TECHNICAL:
□ Webhook verified and working
□ All test messages passed
□ Google Sheets logging working
□ AI responses appropriate
□ Memory persisting correctly
□ Pricing tool returning data
□ Error handling working

BUSINESS:
□ Business profile complete on WhatsApp
□ Quality rating: HIGH (Green)
□ Team trained on escalations
□ Response templates approved
□ Backup plan documented
□ Monitoring schedule set

COMPLIANCE:
□ Privacy policy updated
□ Terms of service reviewed
□ Data handling compliant
□ Opt-out mechanism clear
```

---

## **SUPPORT RESOURCES** 📚

- **Migration Guide:** `WEBHOOK_MIGRATION_GUIDE.md`
- **Production Setup:** `WHATSAPP_PRODUCTION_SETUP_GUIDE.md`
- **Workflow File:** `UAE_Business_Setup_Chatbot_PRODUCTION.json`
- **n8n Community:** https://community.n8n.io/
- **Meta Developer Docs:** https://developers.facebook.com/docs/whatsapp

---

## **DEPLOYMENT STATUS**

**Overall Progress:** _____ / 19 steps complete

**Current Phase:** ☐ Pre-Deployment | ☐ Configuration | ☐ Testing | ☐ Monitoring | ☐ Live | ☐ Complete

**Go-Live Date:** _______________

**Deployed By:** _______________

**Notes:**
```
_____________________________________________
_____________________________________________
_____________________________________________
```

---

**🎉 Congratulations on deploying your production WhatsApp chatbot!**


