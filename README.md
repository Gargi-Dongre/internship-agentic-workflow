# 🚀 Internship Automation Hub  
### *An Agentic AI System Built in n8n for Automating Job Applications*

The **Internship Automation Hub** is an end-to-end agentic AI workflow built in **n8n** that automates job parsing, resume tailoring, match scoring, cover-letter generation, and Google Drive uploads. It is designed to save students hours during internship applications while demonstrating real multi-agent orchestration.

---

## 📌 Features

### Job Data Integration
- Reads job rows from a **Google Sheet** containing job links and metadata.  
- Filters rows that are **not processed**.

### Resume Ingestion
- Loads the user’s **resume PDF** from local disk.  
- Converts it into structured JSON for all LLM agents.

### Job–Resume Match Score
A dedicated LLM agent compares the job description with your resume to produce:
- Match percentage  
- Aligned skills  
- Missing skills  
- Fit summary  

The score is written directly into Google Sheets.

### Resume Tailoring Suggestions
A second LLM agent analyzes the job description and:
- Suggests tailored bullet points  
- Highlights skills to emphasize  
- Identifies keywords to add  
- Generates a short tailored summary  

These suggestions are stored in the sheet per job.

###  Automated Cover Letter Generation
A third LLM agent creates a **personalized cover letter** for each job.  
The system:
1. Generates clean JSON output  
2. Saves the cover letter as a `.txt` file  
3. Uploads it to **Google Drive**  
4. Updates the sheet with the Google Drive link  

### Automatic File Creation
A custom tool, **FileWriterTool**, converts LLM text into a file and returns binary data for Google Drive upload.

### End-to-End Processing
After all tasks run, the workflow marks each row as **PROCESSED**.

---

## System Architecture

### Controller Agent
- Orchestrates the entire workflow  
- Routes job rows to specialized agents  
- Manages inputs/outputs  
- Ensures gated execution using merge nodes  
- Handles fallback defaults and errors  

### Specialized Agents

#### **1. Job Parsing Agent**
- Extracts job title, role, seniority, tech stack, keywords  
- Computes the match score  
- Converts raw descriptions into structured fields  

#### **2. Resume Tailoring Agent**
- Receives job object + resume JSON  
- Generates tailored content  
- Uses code nodes for JSON parsing and sanitization  

#### **3. Cover Letter Agent**
- Produces a personalized cover letter  
- Outputs JSON containing `row_number` + letter text  

---

## Built-in Tools Used
1. **Google Sheets** → read/write job rows  
2. **Google Drive** → upload cover letter files  
3. **LLM Tool** → text generation & transformation  
4. **Code Node** → JSON parsing, data shaping  
5. **Filesystem Node** → read resume from disk  

---

## 🛠️ Custom Tool: FileWriterTool
- Converts LLM text into a `.txt` file  
- Auto-names the file using `row_number`  
- Returns correct binary for upload  
- Ensures encoding consistency  
- Adds validation and error handling  

---

## Workflow Orchestration

**Trigger → Load resume → Get rows → Filter unprocessed →  
Parse job → Tailor resume → Generate cover letter →  
Write file → Upload to Drive → Update Sheet → Mark PROCESSED**

- Parallel branches merge before downstream execution  
- Row metadata is passed through all nodes  
- Resume JSON merges item-wise with job entries  

---

## Challenges & Solutions

| Challenge | Solution |
|----------|----------|
| LLM produced broken JSON | Regex + sanitization + fallback parsing |
| Escape characters in LLM output | Cleaned newline + quote patterns |
| Wrong binary format during Drive upload | FileWriterTool ensures correct encoding |
| Lost metadata like row_number | Added structured propagation through nodes |
| Multi-row handling errors | Item-wise merge transformations |

---

## Evaluation & Testing
- Handles missing job fields gracefully  
- Fixes malformed LLM JSON automatically  
- Retries failed Drive uploads  
- Successfully processes multiple jobs  
- 100% correct file upload and sheet updates  

---

## Metrics
- ✔ 100% file upload success  
- ✔ Tailored resume suggestions aligned with job keywords  
- ✔ End-to-end workflow validated for multiple entries  

---

## Limitations
- LLM consistency may vary  
- Not optimized for very large job batches  
- Requires stable Google API access  

---

## Future Improvements
- RL-based reward signal for tailoring quality  
- Better match_score ranking using embeddings  
- Auto-email recruiter outreach  
- Cover letter PDF formatting  
- Advanced analytics dashboard  

---

## 🏁 Conclusion
The **Internship Automation Hub** is a fully automated agentic AI workflow for internship applications.  
It integrates multiple LLM agents, Google services, and custom tools to deliver a practical, time-saving system that enhances job search efficiency and demonstrates strong automation engineering.
<img width="1393" height="595" alt="Screenshot 2025-11-23 at 12 49 02 PM" src="https://github.com/user-attachments/assets/eaebbc32-4df1-414c-a157-3c36717335ac" />

