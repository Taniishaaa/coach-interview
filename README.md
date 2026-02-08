# AI Coach for Front-End Interview Preparation

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.0+-FF4B4B.svg)](https://streamlit.io)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

An AI-powered interview coaching platform designed to help candidates prepare for front-end web developer roles. The system simulates realistic interview scenarios, evaluates candidate responses, analyzes resumes and job descriptions, and provides ATS-style insights to improve hiring outcomes.

## 📋 Table of Contents

- [Features](#-features)
- [Demo](#-demo)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Module Documentation](#-module-documentation)
- [Contributing](#-contributing)
- [License](#-license)
- [Acknowledgments](#-acknowledgments)

## ✨ Features

- **Job Description Interview Coach**: Upload JD documents (PDF/images), extract text via OCR, and generate targeted interview questions with coaching feedback
- **Answer Scoring Engine**: Automated evaluation of candidate responses across multiple dimensions (content, clarity, communication, relevance)
- **Resume Architect**: AI-driven resume analysis with detailed feedback on strengths, weaknesses, and actionable improvement suggestions
- **ATS Matcher**: Compare resumes against job descriptions to generate ATS-style alignment insights and optimization recommendations
- **General Chat Assistant**: Open-ended AI chat interface for career and interview-related guidance
- **Custom Theming**: Consistent, professional UI with shared styling across all modules

## 🎥 Demo

[Add screenshots or GIF demonstrations of key features here]

## 🛠 Tech Stack

- **Language**: Python 3.9+
- **Framework**: [Streamlit](https://streamlit.io/) - Multi-page web application framework
- **LLM Provider**: [GROQ](https://groq.com/) - LLaMA 3.1 8B Instant model
- **OCR**: 
  - [EasyOCR](https://github.com/JaidedAI/EasyOCR) - Text extraction from images
  - [PyMuPDF (fitz)](https://pymupdf.readthedocs.io/) - PDF processing
- **Image Processing**: Pillow (PIL), NumPy
- **Environment Management**: python-dotenv

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- Python 3.9 or higher
- pip (Python package manager)
- Git
- A valid GROQ API key ([Get one here](https://console.groq.com/))

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Taniishaaa/coach-interview.git
cd coach-interview
```

### 2. Create Virtual Environment

**Windows:**
```bash
python -m venv .venv
.venv\Scripts\activate
```

**macOS/Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the project root directory:

```bash
GROQ_API_KEY=your_groq_api_key_here
```

**Important**: 
- Never commit your `.env` file to version control
- Each contributor must generate their own GROQ API key
- Ensure `.env` is listed in `.gitignore`

### Getting Your GROQ API Key

1. Visit [GROQ Console](https://console.groq.com/)
2. Sign up or log in to your account
3. Navigate to API Keys section
4. Generate a new API key
5. Copy the key to your `.env` file

## 💻 Usage

### Starting the Application

From the project root with your virtual environment activated:

```bash
streamlit run app.py
```

The application will launch in your default browser at `http://localhost:8501`

### Navigation

The application uses two navigation methods:

1. **UI Navigation**: Use the landing page buttons to navigate between features
2. **URL Parameters**: Direct access via query parameters (e.g., `?page=resume`, `?page=jd`)

### Typical Workflow

1. **Start** - Open the application and review the landing page
2. **Job Description Analysis** - Navigate to "JD Interview Coach" and upload a job description
3. **Interview Practice** - Answer generated questions based on the JD
4. **Answer Evaluation** - Use "Answer Scoring" to receive detailed feedback
5. **Resume Review** - Go to "Resume Architect" to upload and analyze your resume
6. **ATS Optimization** - Visit "ATS Matching" to compare resume against JD
7. **General Guidance** - Use "General Chat" for additional coaching

## 📁 Project Structure

```
coach-interview/
├── app.py                          # Main application router and entry point
├── landing_page.py                 # Landing page and navigation hub
├── general_chat.py                 # General AI chat interface
├── ocr_jd.py                       # Job description OCR and interview coach
├── resume_suggestion.py            # Resume architect and AI coach
├── ats.py                          # ATS scoring and matching module
├── answer_score.py                 # Interview answer scoring engine
├── ai_display.py                   # Shared UI components and display helpers
├── coach_v2_data.json              # Configuration and static data
├── styles.css                      # Global application styling
├── ai_text_enhancement.css         # Specialized AI output styling
├── requirements.txt                # Python dependencies
├── .env.example                    # Environment variable template
├── .gitignore                      # Git ignore rules
└── README.md                       # Project documentation
```

## 📚 Module Documentation

### `app.py` - Application Router

**Purpose**: Central entry point and navigation controller

**Responsibilities**:
- Imports all page functions from feature modules
- Initializes session state with default page ("landing")
- Synchronizes URL query parameters with session state
- Routes execution to appropriate page based on `st.session_state.page`
- Handles unknown routes with fallback messaging

**Key Design Pattern**: Single-responsibility router that delegates feature logic to specialized modules

---

### `landing_page.py` - Landing & Navigation Hub

**Purpose**: First impression and global navigation interface

**Responsibilities**:
- Displays application branding and description
- Provides CTA buttons for each major feature
- Updates session state and triggers navigation on user interaction
- Applies consistent styling from `styles.css`

**User Flow**: Entry point → Feature selection → Route to specialized module

---

### `general_chat.py` - General AI Chat

**Purpose**: Open-ended conversational AI interface

**Responsibilities**:
- Maintains chat history in `st.session_state`
- Displays conversation with role-specific styling (user/assistant)
- Processes user input through GROQ LLM (llama-3.1-8b-instant)
- Provides navigation back to landing page

**Use Cases**: Career advice, general interview questions, clarifications

---

### `ocr_jd.py` - JD OCR & Interview Coach

**Purpose**: Job description processing and interview question generation

**Responsibilities**:
- Multi-format JD input (text, PDF, images)
- OCR text extraction using PyMuPDF and EasyOCR
- Text cleaning and chunking for LLM processing
- Generates tailored interview questions with coaching context
- Styled presentation of results

**Pipeline**: Upload → OCR → Parse → Generate Questions → Display

---

### `resume_suggestion.py` - Resume Architect & AI Coach

**Purpose**: Comprehensive resume analysis and conversational coaching

**Key Components**:

#### Helper Functions
- `get_ocr_reader()` - Cached EasyOCR reader initialization
- `chunk_text(text, chunk_size=2000, overlap=200)` - Text chunking for LLM context limits

#### Main Function: `show_resume_suggestion()`

**Features**:
1. **Resume Upload & OCR**
   - Supports PDF, DOCX, DOC, TXT, PNG, JPG, JPEG
   - PDF processing via PyMuPDF with page-by-page rendering
   - Image processing via EasyOCR
   - Results stored in `st.session_state.temp_ocr_library`

2. **Architecture Report Generation**
   - Chunks extracted text
   - Sends to llama-3.1-8b-instant with structured prompt
   - Requests: score, strengths, weaknesses, 3 concrete improvements
   - Renders Markdown-formatted analysis

3. **Conversational AI Coach**
   - Context-aware chat using resume text
   - Maintains conversation history in session state
   - System prompt defines resume coach role
   - Real-time feedback and suggestions

**Best Practices Demonstrated**:
- Resource caching for expensive operations
- Separation of concerns (OCR, analysis, chat)
- Stateful UI for rich interactions
- Consistent error handling

---

### `ats.py` - ATS Scoring & Matching

**Purpose**: Applicant Tracking System perspective on candidate documents

**Responsibilities**:
- Accepts JD and resume text from session state
- Mimics ATS logic through LLM prompting
- Analyzes: keyword coverage, skills match, job title alignment, red flags
- Outputs: match percentage, matched/missing keywords, recommendations
- Structured, styled results presentation

**Value Proposition**: Helps candidates optimize resumes for automated screening systems

---

### `answer_score.py` - Interview Answer Scoring

**Purpose**: Structured evaluation of candidate responses

**Scoring Dimensions**:
- Technical correctness and depth
- Clarity and communication quality
- Relevance to question and JD

**Responsibilities**:
- UI for answer input/recording
- Packages questions and answers into evaluation prompts
- Receives structured LLM responses
- Displays numerical/qualitative scores with feedback
- Persists scores across sessions via session state

**Output Format**: Scores + written feedback + improvement tips

---

### `ai_display.py` - Shared UI Components

**Purpose**: Reusable display helpers for AI-generated content

**Responsibilities**:
- Consistent rendering of AI responses in styled cards
- Markdown rendering with enhanced formatting
- Bullet highlighting and code-block styling
- Optional animations, icons, loading states

**Design Principle**: Centralizes UI concerns so feature modules focus on business logic

---

### Styling Assets

#### `styles.css`
Global styling rules shared across all pages:
- Typography system
- Color palette and theming
- Layout utilities
- Card components
- Button styles

#### `ai_text_enhancement.css`
Specialized styling for AI output:
- Enhanced readability
- Key bullet emphasis
- Code block formatting

---

### Configuration & Utilities

#### `coach_v2_data.json`
Static configuration and sample data for coaching flows

#### Model Utilities
- `available_models.txt` - List of available GROQ models
- `check_models.py` - Model introspection script
- `check_models_log.py` - Model availability logger

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Development Guidelines

- Follow PEP 8 style guidelines for Python code
- Add docstrings to all functions and classes
- Update documentation for new features
- Test thoroughly before submitting PR
- Never commit API keys or sensitive data

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Streamlit](https://streamlit.io/) - For the excellent web framework
- [GROQ](https://groq.com/) - For high-performance LLM inference
- [EasyOCR](https://github.com/JaidedAI/EasyOCR) - For robust OCR capabilities
- [PyMuPDF](https://pymupdf.readthedocs.io/) - For PDF processing

---



**Made with ❤️ by the Coach Interview Team**
