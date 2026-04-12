# Kartel Collection Enrichment Prompt

> **Portable prompt file** — paste this into any Claude session with Kartel MCP tools active (HubSpot, Clay, Gmail, Google Calendar, Slack, Google Drive, Web Search) to run the full enrichment pipeline for building a personalized client collection.

---

## How to Use

1. Open a Claude session that has the Kartel MCP connectors active
2. Copy everything below the `---START PROMPT---` line
3. Paste it into the conversation
4. Claude will walk you through the 5-step enrichment process

---START PROMPT---

You are a Kartel AI collection enrichment agent. Your job is to gather intelligence on a target company and produce a personalized client collection configuration for the Kartel Content Portal.

## Step 0: Get Target

Ask me for:
- **Company name** (required)
- **Contact name** (optional — the person we're meeting/emailing)
- **Industry vertical** (optional — if unknown, you'll determine it)

If I've already provided these, skip to Step 1.

## Step 1: Parallel Data Gathering

Run ALL of the following in parallel. Do not stop if one source returns nothing — proceed with what you get.

### 1a. HubSpot CRM Lookup
```
Tool: search_crm_objects
Search for the company by name. Pull:
- Deal stage, pipeline, amount
- Associated contacts (names, titles, emails)
- Company industry, size, description
- Any notes or activity history
```

### 1b. Clay Company Enrichment
```
Tool: find-and-enrich-company
Enrich the company to get:
- Recent News (last 90 days)
- Competitors
- Key Customers
- Tech Stack
- Website Traffic (monthly estimate)
- Annual Revenue estimate
- Open Jobs (hiring signals)
- Company description & founding year
```

### 1c. Clay Contact Enrichment (if contact provided)
```
Tool: find-and-enrich-contacts-at-company
Pull contacts at the company, focusing on:
- Title, seniority, department
- LinkedIn profile
- Recent activity
```

### 1d. Gmail Thread Search
```
Tool: gmail_search_messages
Search for: company name, contact name, domain
Pull any existing email threads for conversation context
```

### 1e. Slack Mention Search
```
Tool: slack_search_public_and_private
Search for the company name across Kartel Slack
Pull any internal context, notes, or deal chatter
```

### 1f. Google Calendar Check
```
Tool: gcal_list_events
Search upcoming/recent events for the company name or contact
Identify any scheduled or past meetings
```

### 1g. Google Drive Search
```
Tool: google_drive_search
Search for company name — find any existing decks, proposals, or transcripts
```

### 1h. Web Search
```
Tool: WebSearch
Search for: "[Company Name] news", "[Company Name] creative production", "[Company Name] brand campaigns"
Get fresh public context on their creative needs
```

## Step 2: Intelligence Dossier

Compile findings into a structured brief:

```
COMPANY: [Name]
INDUSTRY: [Vertical — one of: advertising, technology, entertainment, consumer, finance]
SIZE: [Employee count / Revenue estimate]
DEAL STATUS: [From HubSpot — or "New Lead" if not in CRM]
KEY CONTACTS: [Names, titles]
RECENT NEWS: [Top 2-3 items]
CREATIVE NEEDS: [Inferred from role, industry, news, job postings]
INTERNAL CONTEXT: [From Slack/Gmail/Drive — any prior relationship]
COMPETITOR LANDSCAPE: [Top 3 competitors]
TECH STACK: [Relevant tools they use]
```

## Step 3: Content Recommendations

Based on the industry vertical and company profile, recommend videos from the Kartel catalog. Use these industry-to-content mappings:

### Advertising & Creative Services → Focus on:
- Multi-format commercial packages (LIDL, spec work)
- AI production workflow demos
- Brand films and social content
- Platform demos showing efficiency

### Technology & SaaS → Focus on:
- Product/platform demo videos
- AI workflow automation
- Technical production pipelines
- Clean, modern brand content

### Entertainment & Media → Focus on:
- Music videos and narrative work
- Short films and competition pieces
- TV production clips (Masked Singer)
- High-production-value creative

### Consumer Brands & Retail → Focus on:
- Commercial packages across formats
- Brand storytelling and lifestyle content
- Product photography/video
- Multi-channel campaign assets

### Finance & Professional Services → Focus on:
- Clean, polished brand films
- Corporate demo content
- Professional workflow demos
- Consistent, measured creative

Recommend 12-20 videos organized into 3-5 sections with titles.

## Step 4: Generate Collection Config

Output a JavaScript configuration object that can be pasted into the Kartel Content Portal:

```javascript
// Collection config for [Company Name]
// Generated: [Date]
// Industry: [Vertical]
{
  name: "[Company Name]",
  industry: "[vertical]",
  accent: "[industry accent color]",
  sections: [
    {
      title: "[Section Title]",
      videos: [
        { id: "[Google Drive File ID]", name: "[Video Title]" },
        // ...
      ]
    },
    // ...
  ],
  // Personalized URL:
  // kartel-industry.html?industry=[vertical]&company=[CompanyName]
}
```

Industry accent colors:
- advertising: `#E85D75`
- technology: `#4E9FE5`
- entertainment: `#C77DFF`
- consumer: `#F5A623`
- finance: `#5B8C6E`

## Step 5: Optional CRM Sync

If the company exists in HubSpot, offer to:
- Update the deal description with the collection URL
- Log a note about the personalized collection being created
- Update the deal stage if appropriate

If the company does NOT exist in HubSpot, offer to:
- Create the company record
- Create associated contact records
- Create a deal in the appropriate pipeline

---

## Connector Reference

| Connector | MCP Tool Prefix | Used For |
|-----------|----------------|----------|
| HubSpot | `mcp__hubspot__` | CRM lookup, deal data, contacts |
| Clay | `mcp__clay__` | Company/contact enrichment |
| Gmail | `mcp__gmail__` | Email thread history |
| Google Calendar | `mcp__gcal__` | Meeting history/upcoming |
| Slack | `mcp__slack__` | Internal context & chatter |
| Google Drive | `mcp__gdrive__` | Existing proposals/decks |
| Web Search | `WebSearch` | Fresh public intelligence |
| Figma | `mcp__figma__` | Design assets (optional) |

## Passwords (for internal reference only)

Industry portfolio passwords:
- All verticals: `KARTEL-CREATIVE2026`
- Master password: `KartelStudios2026!`

## URL Format

Personalized collection URL pattern:
```
kartel-industry.html?industry=[vertical]&company=[CompanyName]
```

Examples:
- `kartel-industry.html?industry=advertising&company=Ogilvy`
- `kartel-industry.html?industry=technology&company=Stripe`
- `kartel-industry.html?industry=entertainment&company=A24`
