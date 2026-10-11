# EduGenie

**EduGenie** is an AI-powered educational assistant that helps students
and self-learners study more effectively through question answering,
concept explanations, quiz generation, note summarization, and
personalized study roadmaps.

**Live application:** https://edu-genai.onrender.com

## Overview

EduGenie combines a FastAPI backend with AI model integrations to
provide learning support through a responsive web interface. It is
designed to make learning more accessible by adapting explanations to
different experience levels and helping learners organize their study
plans.

## Features

-   **Educational Q&A:** Get detailed answers to academic questions.
-   **Concept explanations:** Explore topics at beginner, intermediate,
    or advanced levels.
-   **Quiz generation:** Generate multiple-choice quizzes with answer
    options and explanations.
-   **Note summarization:** Convert study material into concise
    paragraphs, detailed outlines, or bullet-point summaries.
-   **Personalized study roadmaps:** Generate structured learning
    phases, suggested resources, timelines, and practice activities
    based on a learner's goals and skill level.
-   **Model coordination and fallback:** Route requests between the
    configured Google Gemini integration and a local Hugging Face
    LaMini-Flan-T5 pipeline, with fallback handling where supported.

## Technology Stack

  Area              Technologies
  ----------------- -------------------------------------------
  Backend           Python, FastAPI
  Data validation   Pydantic
  Cloud AI          Google Gemini API
  Local AI          Hugging Face Transformers, LaMini-Flan-T5
  Frontend          HTML5, CSS3, JavaScript
  API server        Uvicorn
  Configuration     `python-dotenv`
  Testing           `unittest`, FastAPI `TestClient`

## Architecture

The application separates API routing, education-related business logic,
prompt templates, and AI model clients.

``` mermaid
flowchart TD
    User[Student / User] --> UI[Web Interface]
    UI --> API[FastAPI Application]
    API --> Routes[API Routes]
    Routes --> Service[Education Service]
    Service --> Prompts[Prompt Templates]
    Service --> AI[AI Client Layer]
    AI --> Gemini[Google Gemini API]
    AI --> Local[LaMini-Flan-T5 Pipeline]
    Gemini --> Formatter[Response Formatting]
    Local --> Formatter
    Formatter --> Service
    Service --> UI
```

## Project Structure

``` text
EduGenie/
├── app/
│   ├── main.py
│   ├── config.py
│   ├── routes/
│   │   ├── qa.py
│   │   ├── explain.py
│   │   ├── quiz.py
│   │   ├── summarize.py
│   │   └── recommend.py
│   ├── services/
│   │   └── education_service.py
│   ├── ai/
│   │   ├── gemini.py
│   │   └── lamini.py
│   ├── prompts/
│   ├── models/
│   │   └── schemas.py
│   ├── utils/
│   ├── templates/
│   │   └── index.html
│   └── static/
│       ├── css/
│       └── js/
├── .env.example
├── .gitignore
├── requirements.txt
├── verify_endpoints.py
└── README.md
```

## Getting Started

### Prerequisites

-   Python 3.11 or later
-   pip
-   Git, if cloning the repository

### 1. Clone the repository

Replace `<repository-url>` with the URL of your GitHub repository.

``` bash
git clone <repository-url>
cd EduGenie
```

If you already have the project folder, open a terminal in that folder
and continue to the next step.

### 2. Create a virtual environment

``` bash
python -m venv .venv
```

### 3. Activate the virtual environment

**Windows PowerShell**

``` powershell
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt**

``` cmd
.venv\Scripts\activate.bat
```

**macOS or Linux**

``` bash
source .venv/bin/activate
```

### 4. Install dependencies

``` bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

The local model integration may download model files and additional
dependencies on first use.

### 5. Configure environment variables

Create a `.env` file from the provided example.

**Windows PowerShell**

``` powershell
Copy-Item .env.example .env
```

**macOS or Linux**

``` bash
cp .env.example .env
```

Add your Google Gemini API key to `.env` using the variable expected by
the application:

``` env
GEMINI_API_KEY=your_gemini_api_key
```

Get an API key from [Google AI Studio](https://aistudio.google.com/).

Keep credentials private. Do not commit `.env` or API keys to GitHub.
Configure environment variables separately in your deployment
environment.

### 6. Run the application locally

``` bash
python -m uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

Open the following URLs in your browser:

-   Web application: http://localhost:8000
-   Interactive API documentation: http://localhost:8000/docs
-   Health check: http://localhost:8000/health

## API Reference

The API accepts JSON request bodies for the endpoints below. Include
`Content-Type: application/json` when sending JSON requests.

  -----------------------------------------------------------------------
  Feature           Method            Endpoint          Purpose
  ----------------- ----------------- ----------------- -----------------
  Question          `POST`            `/qa`             Answer academic
  answering                                             questions

  Concept           `POST`            `/explain`        Explain a topic
  explanation                                           at a selected
                                                        level

  Quiz generation   `POST`            `/quiz`           Generate
                                                        multiple-choice
                                                        questions

  Note              `POST`            `/summarize`      Summarize study
  summarization                                         material in a
                                                        selected format

  Study roadmap     `POST`            `/recommend`      Create a learning
                                                        roadmap based on
                                                        goals

  Health check      `GET`             `/health`         Check application
                                                        status
  -----------------------------------------------------------------------

For exact request and response schemas, run the application and visit
`/docs`.

### Example request

Generate a beginner-level explanation of binary search:

``` bash
curl -X POST "http://localhost:8000/explain" \
  -H "Content-Type: application/json" \
  -d '{
    "concept": "Binary Search",
    "level": "Beginner",
    "model_preference": "gemini"
  }'
```

The response structure depends on the endpoint and configured model. Use
the interactive API documentation to inspect the current schemas.

## Testing

The repository includes an endpoint verification script named
`verify_endpoints.py`. The existing test suite uses Python's `unittest`
and FastAPI's `TestClient`, with mocks intended to reduce dependence on
external services.

Run the verification script with:

``` bash
python verify_endpoints.py
```

## Deployment

The application is available at:

**https://edu-genai.onrender.com**

The application's AI features may depend on correctly configured
environment variables and the availability of the configured model
providers. For a new deployment, configure secrets in the hosting
provider rather than committing them to the repository.

## Future Enhancements

Potential areas for future development include:

-   Persistent student profiles, learning progress, and quiz history.
-   Retrieval-augmented generation (RAG) for querying uploaded study
    notes.
-   Text-to-speech support for spoken explanations.

## Contributing

Contributions and suggestions are welcome.

1.  Fork the repository.
2.  Create a branch for your changes.
3.  Make and test your changes.
4.  Submit a pull request describing the update.

## License

Add a license file and specify the license here if the project is
distributed under an open-source license.
