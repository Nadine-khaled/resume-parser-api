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
```

## Installation & Local Development

```bash
# 1. Clone the repository
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
```

The API will be available at:
```
http://127.0.0.1:5000
```

## Important Notes

- The first run will download the skill-extractor model from Hugging Face (~466 MB) — this may take several minutes depending on your internet speed.
- The Groq API key must be provided via environment variable GROQ_API_KEY.
- Date parsing might occasionally produce future or inaccurate dates — this is a known limitation and may require additional post-processing.
- Courses and short trainings sometimes appear in the experience section — consider filtering them on the client side if needed.

## Tech Stack

- Backend: Flask
- PDF/DOCX parsing: pdfplumber + python-docx
- NER & Skills extraction: spaCy + amjad-awad/skill-extractor
- LLM cleaning: Groq (Llama 3.3 70B)
- Production server recommendation: Gunicorn + Nginx (or any WSGI server)

## License

MIT License (or choose whatever fits your project)

**Author:** Nadine Khaled  
[GitHub](https://github.com/Nadine-khaled) | [LinkedIn](https://www.linkedin.com/in/nadine-khaled-6abb68253/)
