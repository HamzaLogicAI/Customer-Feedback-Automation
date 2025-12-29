# 📖 Installation & Setup Guide

This guide will walk you through setting up the Customer Feedback Automation system step by step.

## 🎯 Prerequisites

Before you begin, ensure you have:
- [ ] n8n instance (cloud account or self-hosted)
- [ ] Cohere AI account
- [ ] Airtable account
- [ ] Gmail account
- [ ] Twilio account

## 🚀 Step-by-Step Installation

### 1. n8n Setup

#### Cloud Version (Recommended for beginners)
1. Sign up at [n8n.cloud](https://n8n.cloud)
2. Create a new workflow
3. Go to Settings → Import and upload `Config.json`

#### Self-Hosted Version
```bash
# Using Docker
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -e WEBHOOK_URL=http://localhost:5678/ \
  n8nio/n8n

# Using npm
npm install n8n -g
n8n start
```

### 2. API Credentials Setup

#### 2.1 Cohere AI Configuration
1. Visit [Cohere](https://cohere.ai)
2. Sign up and get your API key
3. In n8n: Settings → Credentials → Add Credential
4. Select "Cohere API" and add your API key

#### 2.2 Airtable Setup
1. Create Airtable account at [airtable.com](https://airtable.com)
2. Create a new base called "Customer Feedback"
3. Create three tables with identical schema:

**Table Names:**
- `Complain`
- `Complement` 
- `Feature Addition Request`

**Table Schema (for each table):**
```
Field Name        | Field Type
------------------|------------
Full Name         | Single line text
Email Address     | Email
Contact Number    | Phone number
Feedback          | Long text
```

4. Get your Airtable API token:
   - Go to [airtable.com/create/tokens](https://airtable.com/create/tokens)
   - Create new token with "data.records:write" scope
   - Add to n8n credentials

#### 2.3 Gmail API Setup
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create new project or select existing one
3. Enable Gmail API
4. Create OAuth 2.0 credentials:
   - Go to APIs & Services → Credentials
   - Create OAuth 2.0 Client ID
   - Add authorized redirect URIs (get from n8n)
5. Add credentials to n8n

#### 2.4 Twilio Configuration
1. Sign up at [Twilio](https://www.twilio.com)
2. Get a WhatsApp-enabled phone number
3. Note your Account SID and Auth Token
4. Add to n8n credentials

### 3. Workflow Configuration

#### 3.1 Import Workflow
1. In n8n, go to Workflows
2. Click "Import"
3. Upload the `Config.json` file
4. The workflow will be imported with all nodes

#### 3.2 Update Node Configurations

**Form Trigger Node:**
- Already configured with customer feedback form
- Note the webhook URL for your form

**AI Agent Node:**
- Verify Cohere credentials are connected
- Prompt is pre-configured

**Airtable Nodes:**
- Update base IDs and table IDs for your Airtable setup
- Verify field mappings match your table schema

**Twilio Nodes:**
- Update the "To" phone numbers with your team's WhatsApp numbers
- Update the "From" number with your Twilio WhatsApp number

**Gmail Nodes:**
- Verify Gmail credentials are connected
- Customize email templates if needed

### 4. Testing the Workflow

#### 4.1 Test Setup
1. Activate the workflow in n8n
2. Get the form webhook URL
3. Create a test form or use the webhook directly

#### 4.2 End-to-End Testing
1. Submit test feedback through the form
2. Check that feedback is classified correctly
3. Verify Airtable records are created
4. Confirm WhatsApp notifications are sent
5. Check email delivery (for complaints and feature requests)

### 5. Production Deployment

#### 5.1 Environment Setup
- Use production credentials for all services
- Set up proper error handling
- Configure logging for monitoring

#### 5.2 Form Integration
Create your customer-facing feedback form that sends data to the n8n webhook:

```html
<form action="YOUR_N8N_WEBHOOK_URL" method="POST">
    <input type="text" name="Full Name" required>
    <input type="email" name="Email Address" required>
    <input type="tel" name="Contact Number">
    <textarea name="Feedback" required></textarea>
    <button type="submit">Submit Feedback</button>
</form>
```

### 6. Monitoring & Maintenance

#### 6.1 Monitoring
- Check n8n execution logs regularly
- Monitor API usage limits
- Set up alerts for failed executions

#### 6.2 Maintenance
- Regularly backup workflow configuration
- Update API credentials before expiry
- Review and optimize AI prompts based on performance

## 🔧 Customization Options

### Modify AI Classification
Update the prompt in the "AI Agent" node to change classification logic:
```
Your role is to determine if feedback is:
1. Complain - negative feedback about products/services
2. Complement - positive feedback or praise  
3. Feature Addition Request - suggestions for new features

Respond with only one option.
```

### Add New Categories
1. Add new condition in Switch node
2. Create corresponding Airtable table
3. Add new WhatsApp notification flow
4. Create email template if needed

### Change Notification Channels
- Replace Twilio with Slack notifications
- Add Microsoft Teams integration
- Use Discord webhooks

## 🆘 Troubleshooting

### Common Issues

**Workflow Not Triggering**
- Check if workflow is activated
- Verify webhook URL is correct
- Check form submission format

**AI Classification Not Working**
- Verify Cohere API key is valid
- Check API usage limits
- Review prompt formatting

**Airtable Errors**
- Confirm base and table IDs are correct
- Check field names match exactly
- Verify API token permissions

**Email Not Sending**
- Check Gmail API quotas
- Verify OAuth credentials
- Test with simple email first

**WhatsApp Messages Failing**
- Confirm Twilio account is active
- Check phone number formatting
- Verify WhatsApp sandbox setup

### Getting Help
- Check n8n execution logs for detailed errors
- Review API documentation for credential setup
- Use n8n community forum for workflow-specific questions

## 📞 Support

If you encounter issues during setup:
1. Check the troubleshooting section above
2. Review the execution logs in n8n
3. Create an issue in this repository with:
   - Error message
   - Steps to reproduce
   - n8n version and setup type

Good luck with your automation! 🚀
