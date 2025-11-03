# 🎨 Visual Comparison: Wrong vs Right Setup

## ❌ **WRONG SETUP** (What you had before)

```
Node Type: n8n-nodes-base.googleSheets
Icon: 📊 (regular Google Sheets icon)

Configuration:
├─ Operation: Read
├─ Document: [Your Sheet]
├─ Sheet: Pricing
└─ Filters: (trying to filter manually)

Connection:
AI Agent ──────► [main] Google Sheets Node
              (regular connection)
```

**Problem**: This is a regular Google Sheets node. It doesn't expose itself as a "tool" the AI can call. The AI can't use it.

---

## ✅ **CORRECT SETUP** (What you need)

```
Node Type: @n8n/n8n-nodes-langchain.toolGoogleSheets
Icon: ? (tool icon at bottom)

Configuration:
├─ Description Type: Manual
├─ Tool Description: [Full description telling AI when/how to use]
├─ Credential: Google Sheets account
├─ Document: 1ZvPGep1ep2zLH6ze9zgTuXjy54oYb8SdXQMtq68mnvo
├─ Sheet: Pricing
└─ Operation: Read

Connection:
AI Agent ◄──[ai_tool]── Pricing Data Tool
           (special AI tool connection from BOTTOM port)
```

**Why it works**: This is a Langchain-compatible tool node. The AI can see it, call it, and get results.

---

## 📸 Screenshot Reference

### **Step 1: Finding the Right Node**

When you search for nodes, you'll see TWO types of Google Sheets nodes:

```
Search: "google sheets"

Results:
┌─────────────────────────────────────┐
│ 📊 Google Sheets                    │  ← WRONG (regular node)
│    Read, update, and write data     │
│    Type: n8n-nodes-base             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ ? Google Sheets Tool                │  ← CORRECT (AI tool)
│    Use Google Sheets as an AI tool  │
│    Type: langchain.toolGoogleSheets │
└─────────────────────────────────────┘
```

**Choose the second one** (Google Sheets Tool)!

---

### **Step 2: Configuration Fields**

When you click on the **Google Sheets Tool** node, you should see:

```
┌─────────────────────────────────────────────────┐
│ Parameters                                      │
├─────────────────────────────────────────────────┤
│                                                 │
│ Description Type ▼                              │
│ ┌───────────────────────────────────────┐       │
│ │ ● Manual                              │       │
│ │ ○ Auto-generate                       │       │
│ └───────────────────────────────────────┘       │
│                                                 │
│ Tool Description                                │
│ ┌───────────────────────────────────────┐       │
│ │ Get UAE Business Setup Pricing Data   │       │
│ │                                       │       │
│ │ Purpose: Search and retrieve...       │       │
│ │ [multiline text box]                  │       │
│ └───────────────────────────────────────┘       │
│                                                 │
│ Credential to connect with ▼                    │
│ ┌───────────────────────────────────────┐       │
│ │ Google Sheets account           [✓]   │       │
│ └───────────────────────────────────────┘       │
│                                                 │
│ Document ▼                                      │
│ ┌───────────────────────────────────────┐       │
│ │ From list ▼                           │       │
│ │ ┌─────────────────────────────────┐   │       │
│ │ │ UAE Pricing Sheet               │   │       │
│ │ │ (1ZvPGep1ep2z...)               │   │       │
│ │ └─────────────────────────────────┘   │       │
│ └───────────────────────────────────────┘       │
│                                                 │
│ Sheet ▼                                         │
│ ┌───────────────────────────────────────┐       │
│ │ Pricing                                │       │
│ └───────────────────────────────────────┘       │
│                                                 │
│ Operation ▼                                     │
│ ┌───────────────────────────────────────┐       │
│ │ Read                                   │       │
│ └───────────────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

---

### **Step 3: Connection Ports**

The **Google Sheets Tool** node has special connection points:

```
         ┌─────────────────────┐
         │  Pricing Data Tool  │
         │                     │
    ●────┤ [MAIN OUTPUT]       │  ← Top/side port (main)
         │     (not used)      │     Leave this empty!
         │                     │
         │     [AI TOOL]       ├────● ← Bottom port (ai_tool)
         └─────────────────────┘       Connect THIS to AI Agent!
```

**Where to drag from**: The small dot at the **BOTTOM** of the node.

**Where to drag to**: The AI Agent node.

---

## 🔍 How to Check Your Setup

### ✅ **Correct Setup Checklist**

1. Node type shows: `@n8n/n8n-nodes-langchain.toolGoogleSheets` ✓
2. "Tool Description" field is filled with detailed instructions ✓
3. Document points to your pricing spreadsheet ✓
4. Sheet is set to "Pricing" ✓
5. Connection goes from **bottom port** (ai_tool) to AI Agent ✓
6. AI Agent has the tool icon showing in its connections ✓

---

## 🎯 Side-by-Side Comparison

| Feature | ❌ Regular Google Sheets | ✅ Google Sheets Tool |
|---------|--------------------------|----------------------|
| **Node Type** | `n8n-nodes-base.googleSheets` | `@n8n/n8n-nodes-langchain.toolGoogleSheets` |
| **AI Can See It** | No | Yes |
| **Tool Description** | Not available | Required field |
| **Connection Type** | Main only | ai_tool port |
| **Auto-querying** | No (manual execution) | Yes (AI decides when) |
| **Icon at Bottom** | None | ? (tool indicator) |
| **Use Case** | Manual workflows | AI-driven queries |

---

## 🧪 Test Your Setup

### Send this message to your bot:
```
"What's the price for DMCC with 2 visas?"
```

### ✅ **If setup is CORRECT**:
```
Bot response:
"Let me check that for you... 
The DMCC package with 2 visas is AED [price] 
and includes [details from your sheet]."
```

### ❌ **If setup is WRONG**:
```
Bot response:
"Our packages typically range from AED 15,000 
to AED 30,000..." (vague, no real data)
```

Or you see errors like:
- "Tool not found"
- "Cannot read pricing"
- AI never calls the tool

---

## 📞 Quick Fix Checklist

If the tool isn't working:

□ Is node type `@n8n/n8n-nodes-langchain.toolGoogleSheets`?
□ Is "Tool Description" filled?
□ Is Document ID correct: `1ZvPGep1ep2zLH6ze9zgTuXjy54oYb8SdXQMtq68mnvo`?
□ Is Sheet name exactly "Pricing" (capital P)?
□ Is connection from BOTTOM port (ai_tool)?
□ Is OpenAI model set to `gpt-4o` (not `chatgpt-4o-latest`)?
□ Is the workflow saved and activated?

---

## 🎓 Understanding the Flow

```
Customer: "Show me packages for 1 visa"
    ↓
WhatsApp Trigger receives message
    ↓
Extract Text → Unify Paths
    ↓
AI Agent receives: "Show me packages for 1 visa"
    ↓
AI reads system prompt: "Use Google Sheets tool for pricing"
    ↓
AI decides: "I need to query pricing data"
    ↓
AI calls: Pricing Data Tool (via ai_tool connection)
    ↓
Tool queries: Google Sheet → Pricing tab → filter visa_count=1
    ↓
Tool returns: [{package_key: "ifza_1v", zone: "IFZA", price_aed: 12500, ...}]
    ↓
AI formats response: "Here are the packages for 1 visa: IFZA at AED 12,500..."
    ↓
Response sent to customer
```

**Key point**: The AI makes the decision to call the tool based on:
1. The system prompt instructions
2. The tool description
3. The customer's question

---

That's why the **Tool Description** is so important! It's like giving the AI a manual on when to use each tool.

