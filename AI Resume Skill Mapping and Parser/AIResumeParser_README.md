# 🚀 AI Resume Parser Workflow (n8n)

This project is an automated workflow built in **n8n** that reads resumes and extracts candidate details automatically.

## ✅ Supported Resume Formats

This workflow can process:

- PDF files  
- Word documents (DOCX)  
- Image resumes (JPG, PNG, scanned resumes)

## 📌 Extracted Candidate Information

The parser can return:

- Name *(if available)*
- Phone Number
- Skills
- Education
- Experience
- Date of Birth
- Gender
- Validation Flags
- Confidence Score

---

# 📌 What Problem Does This Solve?

Recruiters often manually read resumes and enter details into systems.

This workflow automates that process.

### Instead of:

Manually reviewing every resume one by one...

### You can now:

```text
Upload Resume
   ↓
Workflow Reads Resume
   ↓
Extracts Candidate Details
   ↓
Returns Structured Data Automatically
```

---

# 🛠 How It Works

## Step 1 — Resume Upload

A user uploads a resume through an API/Webhook.

The workflow receives the file.

---

## Step 2 — Detect Resume Type

The workflow checks whether the uploaded file is:

- PDF
- Word document
- Image / scanned resume

Then routes it to the correct parser.

---

## Step 3 — Extract Text

### For PDF files
- Reads text directly from PDF

### For DOCX files
- Converts Word file into readable text

### For Image resumes
Uses OCR (text recognition) to read:

- Scanned resumes
- Screenshots
- Photo resumes

---

## Step 4 — Clean Resume Text

Workflow cleans:

- Broken words  
- Extra spaces  
- OCR noise  
- Unwanted symbols  

This improves AI extraction quality.

---

## Step 5 — AI Extracts Candidate Data

The LLM analyzes the resume and extracts:

- Phone Number
- Experience
- Skills
- Education
- Date of Birth
- Gender

Returns structured JSON.

### Example Output

```json
{
  "phone_number": "+919999999999",
  "experience_years": 3,
  "skills": [
    "Python",
    "SQL"
  ],
  "education": []
}
```

---

## Step 6 — Validation Checks

Workflow performs checks for:

- Missing fields
- Incomplete education
- Multiple phone numbers
- Confidence score for extracted data

---

## Step 7 — Final API Response

Returns clean structured JSON response.

---

# 🧩 Workflow Components Used

| n8n Node | Purpose |
|---------|---------|
| Webhook | Receive resume |
| Switch | Detect file type |
| PDF Extraction | Read PDFs |
| HTTP Request | Convert DOCX / OCR |
| Code Nodes | Clean and process data |
| LLM Node | Parse resume using AI |
| Merge | Combine extracted text |
| Respond to Webhook | Return output |

---

# 📂 Services Required Before Setup

This workflow uses external services.

## 1. Groq API (LLM)

Used for AI resume parsing.

Get API key:

https://console.groq.com

Add it in n8n credentials.

---

## 2. OCR.Space API

Used for scanned/image resumes.

Get API key:

https://ocr.space/ocrapi

Replace key in:

```text
OCR Image Extractor node
```

---

## 3. ConvertAPI

Used for Word document parsing.

Get API key:

https://www.convertapi.com/

Replace secret in:

```text
DOCX Converter API node
```

---

## 4. Grafana Logs (Optional)

Used only for logging and monitoring.

Optional.

Can be removed if not needed.

---

# ⚙️ Setup Guide

## Step 1 — Clone / Download Repository

Download this repository.

---

## Step 2 — Import Workflow into n8n

Inside n8n:

- Click **Import**
- Upload workflow JSON file

---

## Step 3 — Configure Credentials

Update these credentials:

- Groq API Key  
- OCR.Space API Key  
- ConvertAPI Secret  

---

## Step 4 — Configure Webhook

Open node:

```text
Resume Upload Trigger
```

Copy generated webhook URL:

```text
https://your-n8n-instance/webhook/parser
```

---

## Step 5 — Activate Workflow

Turn workflow **ON**

Done ✅

---

# ▶️ Test the Workflow

Use Postman or curl:

```bash
curl -X POST \
-F "resume=@sample.pdf" \
https://your-webhook-url/parser
```

You should receive parsed resume data.

---

# 🔄 Workflow Flow Diagram

```text
Upload Resume
   ↓
Detect File Type
   ↓
Extract Text
   ↓
Clean Text
   ↓
AI Resume Parsing
   ↓
Validation
   ↓
Return JSON Response
```

---

# ❗Common Errors

## No text extracted from resume

Cause:
Unreadable file or poor scan.

Fix:
Use clearer resume file.

---

## OCR Failed

Cause:
OCR API issue.

Fix:
Check OCR.Space API key.

---

## Invalid JSON from LLM

Cause:
Malformed model response.

Fix:
Check model or prompt.

---

# 🧹 Optional Nodes You Can Remove

If you want a simpler workflow, you can remove:

- Grafana log nodes
- Confidence scorer
- Flags validator

Core parsing still works.

---

# 📈 Future Improvements

Possible upgrades:

- LinkedIn profile extraction  
- Resume scoring  
- Candidate ranking  
- ATS integration  
- Google Sheets / CRM sync  

---

# 🙋 Who Can Use This?

Useful for:

- Recruiters  
- HR Teams  
- Automation Builders  
- n8n Learners  
- ATS Startups  

---

# 📥 Import File

Workflow included in this repo:

```text
ai-resume-parser.json
```

Import and run.

---

If this workflow helps you, feel free to improve or contribute.
