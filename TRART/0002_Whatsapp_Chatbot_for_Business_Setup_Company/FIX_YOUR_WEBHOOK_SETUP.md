# 🔧 **FIX YOUR WEBHOOK SETUP**
## Correct Configuration for Message Router2

---

## **❌ PROBLEM: What You Have Now**

Your current setup:

```
Webhook POST (Messages)
    ↓
Has Message?1
    ↓
Extract Message Data1 (manually extracting fields)
    ↓
Send WhatsApp Reply1
```

**Issues:**
1. ❌ No verification handler (Meta requires this)
2. ❌ Manual data extraction doesn't match Message Router2 format
3. ❌ Missing `messages`, `contacts`, `metadata` structure
4. ❌ Can't handle audio messages
5. ❌ Doesn't connect to your existing Message Router2

---

## **✅ SOLUTION: What You Need**

Correct setup:

```
WhatsApp Webhook
    ↓
Is Verification Request?
    ├─ YES → Return Verification Challenge
    └─ NO → Normalize Webhook Data
              ↓
         Should Skip? (filters status updates)
              ├─ YES → Respond - Skipped
              └─ NO → Message Router2 (YOUR EXISTING NODE)
                        ↓
                   [Rest of your workflow unchanged]
```

---

## **📋 STEP-BY-STEP FIX**

### **Step 1: Delete Your Current Webhook Nodes**

Delete these nodes:
- ❌ Webhook POST (Messages)
- ❌ Has Message?1
- ❌ Extract Message Data1
- ❌ Send WhatsApp Reply1
- ❌ Respond - Success1
- ❌ Respond - Status Update1

**Keep everything else!** (Message Router2 and all downstream nodes)

---

### **Step 2: Import Correct Webhook Nodes**

1. **Copy this file:** `CORRECTED_WEBHOOK_NODES.json`
2. **In n8n:** Click **"Import from File"**
3. **Select:** `CORRECTED_WEBHOOK_NODES.json`
4. **Result:** You'll get 6 new nodes:
   - WhatsApp Webhook
   - Is Verification Request?
   - Return Verification Challenge
   - Normalize Webhook Data
   - Should Skip?
   - Respond - Skipped

---

### **Step 3: Connect to Your Message Router2**

**Find the connection point:**

The `Should Skip?` node has two outputs:
- **Output 1 (TRUE):** → Respond - Skipped (already connected)
- **Output 2 (FALSE):** → **Connect this to your Message Router2**

**How to connect:**
1. Click on `Should Skip?` node
2. Drag from the **bottom output** (FALSE)
3. Connect to your existing **Message Router2** node

---

### **Step 4: Update Message Router2 References**

Your Message Router2 should already be checking:
- `$json.messages[0].text.body` (for text)
- `$json.messages[0].audio.id` (for audio)

**This will now work!** Because `Normalize Webhook Data` creates exactly this structure.

---

## **🔍 WHAT THE NORMALIZE NODE DOES**

### **Input (from Meta webhook):**

```json
{
  "body": {
    "entry": [{
      "changes": [{
        "value": {
          "messaging_product": "whatsapp",
          "metadata": {
            "display_phone_number": "971559856798",
            "phone_number_id": "862042853657768"
          },
          "contacts": [{
            "profile": {
              "name": "John Doe"
            },
            "wa_id": "971559856798"
          }],
          "messages": [{
            "from": "971559856798",
            "id": "wamid.xxx",
            "timestamp": "1699276800",
            "type": "text",
            "text": {
              "body": "Hello"
            }
          }]
        }
      }]
    }]
  }
}
```

### **Output (for your Message Router2):**

```json
{
  "messages": [{
    "from": "971559856798",
    "id": "wamid.xxx",
    "timestamp": "1699276800",
    "type": "text",
    "text": {
      "body": "Hello"
    }
  }],
  "contacts": [{
    "profile": {
      "name": "John Doe"
    },
    "wa_id": "971559856798"
  }],
  "metadata": {
    "display_phone_number": "971559856798",
    "phone_number_id": "862042853657768"
  }
}
```

**Now your Message Router2 can access:**
- ✅ `$json.messages[0].text.body` → "Hello"
- ✅ `$json.messages[0].from` → "971559856798"
- ✅ `$json.contacts[0].profile.name` → "John Doe"
- ✅ `$json.metadata.phone_number_id` → "862042853657768"

---

## **🎤 AUDIO MESSAGES WORK TOO!**

### **When customer sends audio:**

Meta sends:
```json
{
  "messages": [{
    "from": "971559856798",
    "id": "wamid.xxx",
    "type": "audio",
    "audio": {
      "id": "audio_id_here",
      "mime_type": "audio/ogg; codecs=opus"
    }
  }]
}
```

Your Message Router2 checks:
```javascript
$json.messages[0].audio.id  // ✅ This exists!
```

So audio messages will route correctly to your audio processing path!

---

## **📊 COMPLETE FLOW DIAGRAM**

```
┌─────────────────────────────────────────────────────────────┐
│  CUSTOMER SENDS MESSAGE                                     │
└─────────────────────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────────┐
│  META SENDS TO YOUR WEBHOOK                                 │
│  POST https://n8n.trart.uk/webhook/whatsapp-production     │
└─────────────────────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────────┐
│  1. WhatsApp Webhook (receives POST)                        │
└─────────────────────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────────────────┐
│  2. Is Verification Request?                                │
│     Checks: hub.mode = "subscribe"?                         │
└─────────────────────────────────────────────────────────────┘
         ↓ TRUE              ↓ FALSE
         ↓                   ↓
┌──────────────────┐  ┌─────────────────────────────────────┐
│ Return Challenge │  │ 3. Normalize Webhook Data           │
│ (for Meta)       │  │    Extracts: messages, contacts,    │
└──────────────────┘  │              metadata               │
                      └─────────────────────────────────────┘
                                    ↓
                      ┌─────────────────────────────────────┐
                      │ 4. Should Skip?                     │
                      │    Filters status updates           │
                      └─────────────────────────────────────┘
                           ↓ FALSE (it's a message)
                           ↓
┌─────────────────────────────────────────────────────────────┐
│  5. Message Router2 (YOUR EXISTING NODE)                    │
│     Checks: Text or Audio?                                  │
└─────────────────────────────────────────────────────────────┘
         ↓ TEXT                    ↓ AUDIO
         ↓                         ↓
┌──────────────────┐      ┌──────────────────┐
│ Extract Text2    │      │ Download media2  │
└──────────────────┘      └──────────────────┘
         ↓                         ↓
         └─────────┬───────────────┘
                   ↓
┌─────────────────────────────────────────────────────────────┐
│  [REST OF YOUR WORKFLOW - UNCHANGED]                        │
│  • Unify Message Paths                                      │
│  • AI Agent                                                 │
│  • Parse AI Response                                        │
│  • Send Response                                            │
│  • Log Lead                                                 │
│  • etc.                                                     │
└─────────────────────────────────────────────────────────────┘
```

---

## **🔧 CONFIGURATION CHECKLIST**

### **In n8n:**

```
□ Delete old webhook nodes
□ Import CORRECTED_WEBHOOK_NODES.json
□ Connect "Should Skip?" (FALSE output) to "Message Router2"
□ Verify Message Router2 conditions unchanged
□ Activate workflow
□ Copy webhook URL
```

### **In Meta:**

```
□ Go to: https://developers.facebook.com/apps/
□ WhatsApp → Configuration → Webhook
□ Callback URL: https://n8n.trart.uk/webhook/whatsapp-production
□ Verify Token: BSETUP_PROD_2024_SecureToken
□ Click "Verify and Save"
□ Subscribe to "messages" field
```

---

## **🧪 TESTING**

### **Test 1: Verification**
1. Configure webhook in Meta
2. Click "Verify and Save"
3. **Expected:** ✅ "Webhook verified successfully"
4. **Check n8n logs:** Should show verification request received

### **Test 2: Text Message**
1. Send WhatsApp: "Hello"
2. **Check n8n execution:**
   - WhatsApp Webhook: ✅ Received
   - Is Verification Request?: ✅ FALSE
   - Normalize Webhook Data: ✅ Created structure
   - Should Skip?: ✅ FALSE
   - Message Router2: ✅ Routed to TEXT
   - Extract Text2: ✅ Extracted data
   - AI Agent: ✅ Generated response
   - Send Response: ✅ Sent to customer

### **Test 3: Audio Message**
1. Send voice message
2. **Check n8n execution:**
   - Message Router2: ✅ Routed to AUDIO
   - Download media2: ✅ Got audio URL
   - Transcribe: ✅ Converted to text
   - [Rest of flow continues]

---

## **🐛 TROUBLESHOOTING**

### **Issue: "Cannot read property 'messages' of undefined"**

**Cause:** Message Router2 not receiving correct format

**Fix:**
1. Check `Normalize Webhook Data` node output
2. Should see `messages`, `contacts`, `metadata`
3. If not, check the code in that node

### **Issue: "Webhook verification fails"**

**Cause:** Workflow not active or verify token mismatch

**Fix:**
1. Make sure workflow is ACTIVE (toggle at top)
2. Check verify token in "Is Verification Request?" node
3. Must match exactly what you enter in Meta

### **Issue: "Messages not routing to audio path"**

**Cause:** Audio check condition wrong

**Fix:**
1. Check Message Router2 audio condition
2. Should be: `$json.messages[0].audio.id` exists
3. Or: `$json.messages[0].type` equals "audio"

---

## **📝 VERIFY YOUR MESSAGE ROUTER2**

Your Message Router2 should have these conditions:

### **TEXT Output:**
```javascript
Condition: $json.messages[0].text.body exists
```

### **AUDIO Output:**
```javascript
Condition: $json.messages[0].audio.id exists
OR
Condition: $json.messages[0].type equals "audio"
```

---

## **✅ FINAL CHECKLIST**

Before testing:

```
WEBHOOK NODES:
□ Old nodes deleted
□ New nodes imported
□ Connected to Message Router2
□ Workflow activated

MESSAGE ROUTER2:
□ Text condition: $json.messages[0].text.body
□ Audio condition: $json.messages[0].audio.id
□ Unchanged from original

DOWNSTREAM NODES:
□ Extract Text2: Still references $json.messages[0]...
□ Download media2: Still references $json.messages[0].audio.id
□ All other nodes: Unchanged

META CONFIGURATION:
□ Webhook URL configured
□ Verify token matches
□ Webhook verified
□ "messages" field subscribed
```

---

## **🎯 KEY POINTS**

1. **Don't manually extract data** - Let the Normalize node do it
2. **Match the expected format** - Message Router2 needs `messages`, `contacts`, `metadata`
3. **Handle verification** - Meta requires this before sending messages
4. **Filter status updates** - Not all webhooks are messages
5. **Keep existing logic** - Everything after Message Router2 stays the same

---

## **📞 QUICK REFERENCE**

**Your webhook URL:**
```
https://n8n.trart.uk/webhook/whatsapp-production
```

**Your verify token:**
```
BSETUP_PROD_2024_SecureToken
```

**Connection point:**
```
Should Skip? (FALSE output) → Message Router2
```

**What Message Router2 expects:**
```javascript
{
  messages: [...],
  contacts: [...],
  metadata: {...}
}
```

---

## **🚀 NEXT STEPS**

1. **Import** `CORRECTED_WEBHOOK_NODES.json`
2. **Connect** to Message Router2
3. **Configure** Meta webhook
4. **Test** with real messages
5. **Monitor** execution logs

**You'll be live in 15 minutes!** 🎉

---

**Need more help?** Check the execution logs in n8n to see exactly what data is flowing through each node.


