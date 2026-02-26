# Resume Parser API

A simple, lightweight REST API that parses resumes (PDF & DOCX) and extracts:

- **Skills**
- **Work experience** (title, company, dates)
- **Location**

Powered by **spaCy** + custom skill extraction model from Hugging Face + **Groq LLM** (Llama 3.3 70B) for cleaning and structuring the output.

## Live API (Online & Ready to Use)

**Main Endpoint (POST):**
https://nadinekhaled500-cv-parser-api.hf.space/parse_resume
text**How to send a request:**

- Method: POST
- Content-Type: multipart/form-data
- Form field: `file` (type: File) — upload PDF or DOCX resume

### curl Example
```bash
curl -X POST \
  -F "file=@/path/to/your/resume.pdf" \
  https://nadinekhaled500-cv-parser-api.hf.space/parse_resume
Postman Example

Method → POST
URL → https://nadinekhaled500-cv-parser-api.hf.space/parse_resume
Body → form-data
Key: file → change type to File
Select your CV file → Send

Success Response Example:
JSON{
  "skills": [
    "Python",
    "Machine Learning",
    "Deep Learning",
    "Computer Vision",
    "Pandas",
    "NumPy",
    "Scikit-learn"
  ],
  "experience": [
    {
      "title": "Machine Learning Engineer Intern",
      "company": "DEPI",
      "startDate": "Jun 2025",
      "endDate": "present"
    },
    {
      "title": "AI Trainee",
      "company": "National Telecommunication Institute (NTI)",
      "startDate": "Aug 2024",
      "endDate": "Sep 2024"
    }
  ],
  "location": {
    "city": "Fayoum",
    "country": "Egypt"
  }
}
Note: The first request may take 10–40 seconds (cold start + model loading). After that it's fast.
Local Installation & Development
Bash# 1. Clone the repository
git clone https://github.com/Nadine-khaled/resume-parser-api.git
cd resume-parser-api

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download the large English spaCy model (needed only once)
python -m spacy download en_core_web_lg

# 4. Create .env file and add your Groq API key
echo GROQ_API_KEY=your_groq_api_key_here > .env

# 5. Run the server
python app.py
# or with gunicorn (recommended for production-like testing)
# pip install gunicorn
gunicorn --bind 0.0.0.0:5000 app:app
The API will be available locally at:
http://127.0.0.1:5000/parse_resume
Important Notes

First run will download the skill-extractor model from Hugging Face (~466 MB) — may take several minutes depending on your internet speed.
Groq API key must be provided via environment variable GROQ_API_KEY.
Date parsing might occasionally produce future or inaccurate dates — known limitation, may need post-processing.
Courses/short trainings sometimes appear in experience — filter on client side if needed.

Tech Stack

Backend: Flask
PDF/DOCX parsing: pdfplumber + python-docx
NER & Skills extraction: spaCy + amjad-awad/skill-extractor
LLM cleaning: Groq (Llama 3.3 70B)
Production server recommendation: Gunicorn

**Author:** Nadine Khaled  
[GitHub](https://github.com/Nadine-khaled) | [LinkedIn](https://www.linkedin.com/in/nadine-khaled-6abb68253/)
