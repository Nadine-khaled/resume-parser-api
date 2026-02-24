# Resume Parser API

A simple REST API that parses resumes (PDF & DOCX) and extracts:

- Skills
- Work experience (title, company, dates)
- Location

Powered by **spaCy** + a custom skill extraction model from Hugging Face + **Groq LLM** for cleaning and structuring the output.

## Features

- Accepts PDF and DOCX files
- Extracts skills using a dedicated skill NER model
- Uses Groq (Llama 3.3 70B) to clean and format the raw extracted data
- Returns clean JSON output

## API Endpoint

**POST** `/parse_resume`

**Content-Type:** `multipart/form-data`

**Form field:**
- `file` → the resume file (PDF or DOCX)

### Example Response (success)

```json
{
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
