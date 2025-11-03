# UAE Business Setup Chatbot - Deployment Guide

A production-ready WhatsApp chatbot for UAE business setup consultancy, powered by n8n and OpenAI. Handles text and voice messages, qualifies leads, and automates follow-ups.

## 🎯 Features

- **Conversational AI**: Natural, context-aware conversations using GPT-4 with memory
- **Voice Support**: Transcribes voice messages and responds with audio
- **Lead Qualification**: Automatically classifies leads as HOT/WARM/COLD based on conversation
- **Business Data Extraction**: Captures activity, budget, timeline, visa needs, target market
- **Smart Routing**: HOT leads trigger immediate Slack notifications
- **Automated Follow-ups**: Scheduled follow-ups for WARM and HOT leads
- **Google Sheets Logging**: All conversations logged with structured business data

## 📋 Prerequisites

### Required Accounts & Credentials

1. **WhatsApp Business API**
   - WhatsApp Business Account with API access
   - Phone Number ID
   - Access Token

2. **OpenAI API**
   - API Key with access to:
     - GPT-4 (chatgpt-4o-latest)
     - Whisper (audio transcription)
     - TTS-1-HD (text-to-speech)

3. **Google Sheets**
   - Google account with Sheets API enabled
   - OAuth2 credentials

4. **Slack** (Optional)
   - Slack workspace
   - Incoming webhook or OAuth2 app

5. **n8n**
   - Self-hosted or cloud instance (n8n.io)
   - Version 1.0+

## 🚀 Quick Start

### Step 1: Set Up Google Sheets

1. Create a new Google Sheet named "UAE Business Setup Leads"
2. Add the following column headers in the first row:

```
Timestamp | Full Name | Phone Number | Business Activity | Target Market | Budget Range | Visa Needs | Timeline | Customer Message | AI Response | Lead Quality | Status | Follow Up Date | Next Action | Needs Escalation | Response Source | Message ID
```

3. Note the Sheet ID from the URL:
   ```
   https://docs.google.com/spreadsheets/d/[SHEET_ID]/edit
   ```

### Step 2: Configure Environment Variables

In your n8n instance, set these environment variables:

```bash
# Google Sheets
GOOGLE_SHEETS_LEADS_ID=your_sheet_id_here
GOOGLE_SHEETS_LEADS_URL=https://docs.google.com/spreadsheets/d/your_sheet_id_here/edit

# Slack (Optional)
SLACK_CHANNEL_HOT_LEADS=your_slack_channel_id
```

### Step 3: Set Up Credentials in n8n

#### WhatsApp Business API
1. Go to Settings → Credentials → New
2. Select "WhatsApp OAuth2 API"
3. Enter your credentials:
   - Access Token
   - Phone Number ID
4. Test the connection

#### OpenAI API
1. Settings → Credentials → New
2. Select "OpenAI API"
3. Enter your API key
4. Save

#### Google Sheets OAuth2
1. Settings → Credentials → New
2. Select "Google Sheets OAuth2 API"
3. Follow OAuth flow to authenticate
4. Grant necessary permissions

#### Slack (Optional)
1. Settings → Credentials → New
2. Select "Slack OAuth2 API" or "Slack Webhook"
3. Configure based on your Slack app setup

### Step 4: Import Workflow

1. In n8n, go to Workflows → Import from File
2. Select `UAE_Business_Setup_Chatbot.json`
3. The workflow will be imported with all nodes

### Step 5: Update Credential References

For each node that requires credentials:

1. **WhatsApp Trigger2**: Assign your WhatsApp OAuth2 credential
2. **Download media2**: Assign WhatsApp API credential
3. **Send Response**: Assign WhatsApp API credential
4. **Send Voice Response**: Assign WhatsApp API credential
5. **OpenAI Chat Model**: Assign OpenAI credential
6. **Transcribe a recording**: Assign OpenAI credential
7. **Generate audio**: Assign OpenAI credential
8. **Log Lead**: Assign Google Sheets OAuth2 credential
9. **Read Leads**: Assign Google Sheets OAuth2 credential
10. **Update Lead Status**: Assign Google Sheets OAuth2 credential
11. **Notify Team (HOT)**: Assign Slack credential (optional)
12. **Download Audio**: Create HTTP Header Auth credential with your WhatsApp media download token

### Step 6: Update Phone Number ID

Search for `762171880324039` in the workflow and replace with your WhatsApp Business Phone Number ID in these nodes:
- Send Response
- Send Voice Response
- Send Follow-Up

### Step 7: Activate Workflow

1. Click the toggle switch to activate the workflow
2. The WhatsApp Trigger will generate a webhook URL
3. Configure this webhook URL in your WhatsApp Business API settings

## 🔧 Configuration

### AI Agent Customization

The AI Agent system prompt is in the "AI Agent" node. Customize:
- Consultant name (default: "Ahmed")
- Pricing tiers
- Discovery questions
- Objection handling scripts
- Lead qualification criteria

### Lead Quality Criteria

Edit in "Parse AI Response" node if needed:

- **HOT**: Immediate timeline, budget confirmed, documentation ready
- **WARM**: 1-month timeline, budget identified, asking detailed questions
- **COLD**: Exploring, budget concerns, no clear timeline

### Follow-Up Schedule

Default: Every 48 hours. Edit "Follow-Up Schedule" node to change frequency.

### Human Delay

Default: 8 seconds. Edit "Human Delay" node to adjust response timing.

## 📊 Google Sheets Structure

### Column Definitions

| Column | Type | Description |
|--------|------|-------------|
| Timestamp | DateTime | Message received time |
| Full Name | Text | Customer's full name |
| Phone Number | Text | WhatsApp number (without @s.whatsapp.net) |
| Business Activity | Text | Type of business (e.g., "E-commerce", "Trading") |
| Target Market | Text | "Mainland", "Free Zone", or "Both" |
| Budget Range | Text | "Under 15K", "15-25K", "25-30K", "Above 30K" |
| Visa Needs | Number | Number of visas required |
| Timeline | Text | "Immediate", "1 Month", "Exploring" |
| Customer Message | Text | Original customer message |
| AI Response | Text | Bot's response (cleaned, no markers) |
| Lead Quality | Text | "HOT", "WARM", or "COLD" |
| Status | Text | Current lead status |
| Follow Up Date | DateTime | Next follow-up scheduled |
| Next Action | Text | "NURTURE", "QUOTE", "ESCALATE" |
| Needs Escalation | Text | "Yes" or "No" |
| Response Source | Text | "ai-agent" |
| Message ID | Text | WhatsApp message ID |

## 🧪 Testing

### Test Checklist

- [ ] Send a text message to WhatsApp number → Receive AI response
- [ ] Send a voice message → Receive voice response
- [ ] Verify conversation memory (bot remembers previous messages)
- [ ] Check Google Sheets logging (new row created)
- [ ] Trigger HOT lead → Verify Slack notification
- [ ] Wait for follow-up schedule → Verify automated message
- [ ] Test all discovery questions flow
- [ ] Verify business data markers extraction
- [ ] Test objection handling responses
- [ ] Confirm escalation logic

### Sample Test Conversations

See `Sample_Conversations.md` for detailed examples of HOT, WARM, and COLD lead interactions.

### Test Messages

**HOT Lead Test:**
```
Customer: I want to start an e-commerce business in Dubai Free Zone
Bot: [Will ask discovery questions]
Customer: [Answer with: immediate timeline, 20K budget, documentation ready]
Bot: [Should mark as HOT, trigger Slack notification]
```

**Voice Message Test:**
```
Send a voice note saying: "Hi, I need help setting up a consulting business"
Bot should: Transcribe → Process → Reply with voice
```

## 🔍 Monitoring & Debugging

### Node-Level Debugging

Each Code node includes console.log statements. View in n8n execution logs:

1. Click on workflow execution
2. Select individual nodes
3. Check "Code" tab for logs

### Common Issues

**Issue: WhatsApp webhook not receiving messages**
- Verify webhook URL in Meta Business Suite
- Check WhatsApp API credentials
- Ensure phone number is verified

**Issue: Voice messages not transcribed**
- Verify OpenAI Whisper API access
- Check "Download Audio" node credentials
- Confirm MIME type conversion in "Code in JavaScript" node

**Issue: Google Sheets not logging**
- Check Google Sheets credential permissions
- Verify Sheet ID in environment variables
- Ensure "Prepare Lead Data" node formatting matches columns

**Issue: Slack notifications not sent**
- Verify Slack channel ID in environment variables
- Check Slack OAuth2 scope includes `chat:write`
- Confirm "Is HOT Lead?" condition is met

**Issue: Memory not working (bot forgets conversation)**
- Check "Simple Memory" node configuration
- Verify sessionKey is using `from` field (WhatsApp number)
- Ensure contextWindowLength is set (default: 20)

## 🎨 Customization

### Adding New Business Setup Markers

1. Update AI Agent system prompt with new marker (e.g., `[NATIONALITY: text]`)
2. Add extraction logic in "Parse AI Response" node:
   ```javascript
   let nationality = '';
   // In the for loop:
   else if (line.includes('[NATIONALITY:')) {
     const match = line.match(/\[NATIONALITY:\s*([^\]]+)\]/i);
     if (match) nationality = match[1].trim();
   }
   ```
3. Add to output object:
   ```javascript
   nationality: nationality,
   ```
4. Update "Prepare Lead Data" node to include in Google Sheets:
   ```javascript
   'Nationality': input.nationality || 'Not specified',
   ```
5. Add column to Google Sheets

### Changing Response Timing

Edit "Human Delay" node:
- Default: 8 seconds
- Shorter for faster responses
- Longer for more "human-like" feel

### Adding More Discovery Questions

Update AI Agent system prompt:
1. Add question to "Discovery Questions Flow" section
2. Include purpose/context
3. Add corresponding marker if tracking data

### Custom Pricing Packages

Update AI Agent system prompt "Package Recommendations" section:
- Modify price ranges
- Add new tiers
- Update escalation thresholds

## 🔐 Security Best Practices

1. **Never commit credentials**: Use environment variables
2. **Restrict API access**: Limit OpenAI API keys to necessary models
3. **Google Sheets permissions**: Use least-privilege principle
4. **WhatsApp verification**: Enable two-factor authentication
5. **Webhook security**: Use webhook signature verification if available
6. **Data retention**: Implement Google Sheets cleanup for old leads

## 📈 Scaling Considerations

### High Volume (100+ messages/day)

1. **OpenAI Rate Limits**: Monitor usage, consider GPT-4o-mini for non-critical paths
2. **Google Sheets**: Consider migrating to database (PostgreSQL, MongoDB)
3. **n8n Performance**: Scale horizontally with multiple n8n instances
4. **WhatsApp API**: Ensure business tier supports volume

### Multi-Language Support

1. Add language detection in "Unify Message Paths" node
2. Update AI Agent prompt with multilingual capabilities
3. Consider separate workflows per language
4. Update TTS model for language-specific voices

## 🛠️ Maintenance

### Regular Tasks

**Weekly:**
- Review Google Sheets for data quality
- Check HOT leads follow-up status
- Monitor OpenAI API usage/costs

**Monthly:**
- Analyze lead quality distribution
- Review and update AI Agent prompts based on conversations
- Clean up old leads from Google Sheets
- Update pricing in system prompt if changed

**Quarterly:**
- Audit conversation quality
- Test all workflow paths
- Update dependencies (n8n version)
- Review and optimize API costs

## 📞 Support & Troubleshooting

### Debug Mode

Enable detailed logging:
1. Edit each Code node
2. Ensure `console.log` statements are present
3. Run test messages
4. Check execution logs in n8n

### Workflow Execution History

n8n keeps execution history:
1. Workflows → Select workflow → Executions
2. Filter by status (Success, Error, Waiting)
3. Click execution to see detailed path and data

### Getting Help

- **n8n Community**: https://community.n8n.io/
- **WhatsApp Business API Docs**: https://developers.facebook.com/docs/whatsapp
- **OpenAI API Docs**: https://platform.openai.com/docs/

## 📝 License

This workflow is provided as-is for business use. Modify and adapt as needed for your consultancy.

## 🎯 Next Steps

After deployment:

1. ✅ Test with real conversations
2. ✅ Monitor first 50 leads for quality
3. ✅ Adjust AI prompts based on feedback
4. ✅ Train team on Slack notification handling
5. ✅ Set up Google Sheets dashboards for analytics
6. ✅ Document any custom modifications

---

**Built for UAE Business Setup Consultancies** | Powered by n8n + OpenAI | Ready for Production 🚀

