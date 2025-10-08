# 🧠 Cybersecurity Outreach Workflow – Navneet Gupta

## Overview
This n8n workflow automates the process of identifying companies affected by **cybersecurity breaches in 2023**, finding their **relevant security contacts**, and drafting **personalized outreach emails**.  
It integrates public APIs like **HaveIBeenPwned** and **Hunter.io**, processes and enriches the data using **JavaScript Code nodes**, and outputs the final results into a **Google Sheet** for review.

---

## Workflow Logic & Design

### 1. Start Workflow (Manual Trigger)
- Begins the process when manually executed.

### 2. Get All Breaches from HaveIBeenPwned API
- Fetches breach data from `https://haveibeenpwned.com/api/v3/breaches` using an API key and user-agent header.

### 3. Filter 2023 Breaches (Code Node)
- Filters breaches by year (2023) and ensures only valid cybersecurity-related entries (breach, leak, compromise, etc.) are retained.

### 4. Enrich Breach Data (Code Node)
- Extracts company name, domain, and a clean summary of each breach.
- Prepares data for external API calls by removing HTML and normalizing text.

### 5. Check Valid Company (IF Node)
- Verifies if a company name exists.
- Invalid entries are redirected to a fallback handler.

### 6. Wait for Rate Limit (Wait Node)
- Introduces a 1-minute delay to avoid exceeding Hunter.io API rate limits.

### 7. Hunter.io – Find Security Contacts (HTTP Request Node)
- Queries the Hunter.io API for each company domain to fetch relevant contact details.

### 8. Clean Data Structure (Code Node)
- Normalizes the Hunter.io response into a consistent JSON structure (name, email, position, confidence, department).

### 9. Score & Select Best Contact (Code Node)
- Applies a **custom scoring function** that prioritizes CISOs, Heads of Security, and CTOs.
- Scores based on title, department, seniority, and confidence level.
- Selects **top 2 contacts** per company for outreach.

### 10. Draft Personalized Email (Code Node)
- Generates personalized, human-like outreach emails referencing the breach name, company, and year.
- The email tone is professional and empathetic.

### 11. Invalid Company Fallback (Code Node)
- Handles missing or invalid company data gracefully to prevent workflow failure.

### 12. Write Results to Google Sheets
- Saves the final output to Google Sheets with columns:
  - Company Name  
  - Breach Name  
  - Breach Date  
  - Contact Name  
  - Contact Position  
  - Contact Email  
  - Email Subject  
  - Draft Email  
  - Status

---

## Key Highlights

### ✅ APIs Used
- **HaveIBeenPwned** → Source for breach data  
- **Hunter.io** → Security contact discovery  
- **Google Sheets API** → Output storage  

### ✅ Error & Fallback Handling
- Invalid company data redirected to fallback node  
- Wait node prevents API throttling  
- Default safe values ensure workflow never breaks

### ✅ Enhancements
- Contact scoring logic for prioritizing key decision-makers  
- Personalized email generation and dynamic subject lines  
- Structured data enrichment and cleanup for reliability

---

## Sample Output Fields

| Company Name | Breach Name | Contact Name | Contact Email | Email Subject | Draft Email | Status |
|---------------|--------------|---------------|----------------|----------------|---------------|---------|
| Example Corp | Example Data Breach 2023 | John Doe | john@example.com | Post-breach security insights for Example Corp | (Personalized Email Content) | Ready for Outreach |

---

## Execution Flow Summary

