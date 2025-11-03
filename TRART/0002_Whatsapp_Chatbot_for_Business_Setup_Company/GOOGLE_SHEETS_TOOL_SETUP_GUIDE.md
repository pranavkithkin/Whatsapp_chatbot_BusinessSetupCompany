# 📘 Step-by-Step Guide: Setting Up Google Sheets Tool for Pricing

## ✅ What You Need
- Your pricing spreadsheet ID: `1ZvPGep1ep2zLH6ze9zgTuXjy54oYb8SdXQMtq68mnvo`
- Sheet tab name: `Pricing`
- Your Google Sheets credentials connected in n8n

---

## 🎯 Step-by-Step Configuration

### **Step 1: Import the Workflow**
1. Import `UAE_Business_Setup_Chatbot_FINAL.json` into n8n
2. The workflow will have a node called **"Pricing Data Tool"** (with a `?` icon at the bottom)

---

### **Step 2: Click on the "Pricing Data Tool" Node**
You'll see a configuration panel with several fields to fill.

---

### **Step 3: Configure Each Field**

#### **Field 1: Description Type**
- **Setting**: `Manual` (should already be selected)
- ✅ This allows you to write a custom description for the AI

---

#### **Field 2: Tool Description** (MOST IMPORTANT!)
Copy and paste this exact text:

```
Get UAE Business Setup Pricing Data

Purpose: Search and retrieve current pricing for UAE business setup packages from the live pricing database.

What you can search for:
- Specific package names (e.g., "IFZA", "DMCC")
- Number of visas (e.g., "1 visa", "2 visas")
- Free zone names
- Price ranges
- Package features

When to use:
- Customer asks about pricing
- Customer wants package recommendations
- Customer compares zones or packages
- Customer asks "how much" or "what's the cost"

The tool will return package details including: package_key, zone, visa_count, price_aed, description, setup_days, and what's included.
```

**Why this matters**: This description tells the AI **when** and **how** to use the tool. The AI reads this and decides if it should query your pricing sheet.

---

#### **Field 3: Credential to connect with**
- **Click the dropdown**
- **Select**: `Google Sheets account` (your existing credential)
- If you don't see it, click "Create New Credential" and connect your Google account

---

#### **Field 4: Document**
- **Click the dropdown**: You'll see two options: "From list" or "By ID"
- **Select**: `From list`
- **Then**: A list of your Google Sheets will appear
- **Find and select**: The sheet with ID `1ZvPGep1ep2zLH6ze9zgTuXjy54oYb8SdXQMtq68mnvo`
  - OR if you can't find it, choose "By ID" and paste: `1ZvPGep1ep2zLH6ze9zgTuXjy54oYb8SdXQMtq68mnvo`

---

#### **Field 5: Sheet (Tab Name)**
- **This dropdown appears AFTER you select the document**
- **Click the dropdown**: You'll see all tabs in your spreadsheet
- **Select**: `Pricing`

---

#### **Field 6: Operation** (Optional, should be default)
- **Setting**: `Read` (default)
- ✅ This allows the AI to query/read the data

---

### **Step 4: Connect the Tool to AI Agent**

**CRITICAL**: The tool needs to be connected via a special `ai_tool` connection (not a regular connection).

1. **Look for a small dot/port at the BOTTOM** of the "Pricing Data Tool" node
   - This is the `ai_tool` output (different from the main output)
2. **Drag a connection** from this bottom port to the "AI Agent" node
3. **The connection should appear as a thin line** (not a thick main line)

---

## 🔍 Visual Checklist

Your "Pricing Data Tool" node should look like this:

```
┌─────────────────────────────────────────────────┐
│ Pricing Data Tool                               │
│ Type: @n8n/n8n-nodes-langchain.toolGoogleSheets │
├─────────────────────────────────────────────────┤
│ Description Type: Manual                        │
│                                                 │
│ Tool Description:                               │
│ Get UAE Business Setup Pricing Data...          │
│ (full description here)                         │
│                                                 │
│ Credential: Google Sheets account ✓             │
│                                                 │
│ Document: [Your Pricing Sheet] ✓                │
│                                                 │
│ Sheet: Pricing ✓                                │
│                                                 │
│ Operation: Read                                 │
└─────────────────────────────────────────────────┘
         │ (main output - empty)
         │
         ▼ (ai_tool output - to AI Agent) ─────►
```

---

## ✅ How to Test

1. **Save the workflow**
2. **Activate it**
3. **Send a WhatsApp message** like:
   - "Show me packages for 1 visa"
   - "What are the IFZA options?"
   - "How much for DMCC?"

4. **The AI should**:
   - Call the Google Sheets tool automatically
   - Retrieve pricing data
   - Present it to the customer

---

## 🚨 Common Mistakes to Avoid

❌ **Wrong Node Type**: Using `n8n-nodes-base.googleSheets` instead of `@n8n/n8n-nodes-langchain.toolGoogleSheets`
  - ✅ Make sure it says **"toolGoogleSheets"** in the node type

❌ **Missing Tool Description**: Leaving the description empty
  - ✅ The AI needs this to understand when to use the tool

❌ **Wrong Connection Type**: Connecting via "main" output instead of "ai_tool"
  - ✅ Use the bottom port (ai_tool output)

❌ **Sheet Name Mismatch**: Typing "pricing" instead of "Pricing" (case-sensitive)
  - ✅ Match the exact tab name in your spreadsheet

---

## 🎯 Expected Behavior

When a customer asks: **"What's the cheapest option for 2 visas?"**

**Behind the scenes**:
1. AI Agent receives the message
2. AI reads its system prompt: "Use Google Sheets tool for pricing"
3. AI decides: "I need pricing data" → calls tool
4. Tool queries your Pricing sheet: filters for `visa_count = 2`
5. Tool returns data to AI
6. AI formats the response: "The cheapest option for 2 visas is [package] at AED [price]..."
7. Customer receives the answer

---

## 🆘 Troubleshooting

### Error: "Tool not found" or "Tool is undefined"
- **Fix**: Check the `ai_tool` connection (not main connection)

### Error: "Sheet not found"
- **Fix**: Verify the sheet tab is named exactly "Pricing" (case-sensitive)

### AI doesn't call the tool
- **Fix**: Check the "Tool Description" field - make sure it's filled with clear instructions

### Error: "Permission denied"
- **Fix**: Your Google Sheets credential might not have access to the spreadsheet

---

## 📊 Your Pricing Sheet Structure

Make sure your "Pricing" tab has these columns (in any order):
- `package_key`
- `zone`
- `visa_count`
- `price_aed`
- `description`
- `setup_days`
- `includes`

---

## ✨ That's It!

Once configured, the AI will automatically:
- ✅ Detect pricing questions
- ✅ Query your live sheet
- ✅ Return current prices
- ✅ Never quote outdated information

**No more hardcoded prices!** 🎉

