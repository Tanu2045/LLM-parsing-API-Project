# ZIP-to-Representative Lookup — Civic Tracker Prototype

Lightweight prototype that returns U.S. elected officials for a given ZIP code using simple scraping, LLM-assisted extraction, and an SQLite-backed API built with Flask.

---

## Table of contents
- About
- Features
- Project structure
- Quickstart
- Environment
- Initialize database
- Run scraper (optional)
- Run the API
- API reference
- Database schema
- Scraper overview
- Notes & limitations
- Contributing
- Demo
- Author & License

---

## About
Civic Tracker is an educational prototype that demonstrates mapping U.S. ZIP codes to elected representatives (local, state, and federal). The project collects representative information via web scraping and optional Google Gemini LLM-assisted extraction, stores normalized data in SQLite, and exposes results through a minimal Flask API.

This repository is intended for experimentation and learning, not as a production civic data service.

---

## Features
- SQLite schema for geography and representatives
- Scraper combining BeautifulSoup with Google Gemini for extraction and normalization
- API endpoint: GET /representatives?zip=<ZIP>
- Setup scripts and example/demo data for quick testing
- Minimal external dependencies for easy local usage

---

## Project structure
- app.py                — Flask API for fetching representatives  
- scraper_gemini.py     — Scraper + LLM extraction + DB insertion  
- setup_database.py     — Creates SQLite DB & inserts initial seed data  
- civic_tracker.db      — SQLite database (auto-generated)  
- requirements.txt      — Python dependencies  
- README.md             — Project documentation

---

## Quickstart

1. Clone the repository
```bash
git clone https://github.com/sarth-04/civic-tracker-proto.git
cd civic-tracker-proto
```

2. (Optional) Create and activate a virtual environment
```bash
python -m venv venv
# macOS / Linux
source venv/bin/activate
# Windows
venv\Scripts\activate
```

3. Install dependencies
```bash
pip install -r requirements.txt
```

4. Create a `.env` file in the project root and add your Google Gemini API key if you plan to use LLM-assisted scraping:
```
GOOGLE_API_KEY=your_google_gemini_api_key_here
```

5. Initialize the database
```bash
python setup_database.py
```

6. (Optional) Run the scraper to populate or refresh representative data
```bash
python scraper_gemini.py
```

7. Start the API
```bash
python app.py
```

The API listens by default on http://127.0.0.1:5000

---

## API Reference

Endpoint
```
GET /representatives?zip=<ZIP_CODE>
```

Example request
```
GET http://127.0.0.1:5000/representatives?zip=11354
```

Example JSON response
```json
{
  "zip": "11354",
  "representatives": [
    { "name": "Grace Meng", "title": "U.S. House Rep, NY-6" },
    { "name": "Chuck Schumer", "title": "U.S. Senator, NY" },
    { "name": "Kathy Hochul", "title": "Governor, New York" }
  ]
}
```

Response fields
- zip: string — the ZIP code queried
- representatives: array — objects containing at minimum name and title (DB may include additional fields)

---

## Database schema (overview)
- geography: Stores ZIP, city, state, district (one row per ZIP)  
- representatives: Representative details (name, title, party, branch, etc.)  
- rep_geography_map: Many-to-many mapping between geography and representatives

See setup_database.py for the explicit schema and sample inserts.

---

## How the scraper works
- scraper_gemini.py uses Requests + BeautifulSoup to fetch and parse web pages.
- When helpful, it sends text to Google Gemini (via the GOOGLE_API_KEY) to assist in extracting and normalizing representative fields.
- Parsed records are inserted or merged into the SQLite database.

Notes:
- LLM usage is optional — you can adapt the scraper to pure parsing if desired.
- Always respect robots.txt and the terms of the sites you scrape.

---

## Notes & limitations
- Prototype scope: limited ZIP coverage (sample seed data only).
- Not production-ready: lacks authentication, rate limiting, caching, and robust error handling.
- Data accuracy depends on scraping rules and LLM outputs; verify before relying on results.
- For production use, consider a production database, caching, secure key management, and legal review for data sources.

---

## Contributing
Contributions welcome via issues and pull requests. Ideas:
- Expand ZIP coverage
- Add unit tests and CI
- Improve scraper resilience and parsing-first logic
- Add caching, auth, and production DB support

---

## Demo
Video demo: https://drive.google.com/file/d/1Fg7OYozHXtNWU2QfWw4aPktkLxYc2gDh/view?usp=sharing

---

## Author
Tanu2045
Email: tanupriya02525@gmail.com 
GitHub: https://github.com/Tanu2045

---

## License
Add your preferred license (e.g., MIT) here.
