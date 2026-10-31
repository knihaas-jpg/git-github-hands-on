Accessibility Audit Tool for a Mini CMS
A Django-based tool that scans web pages for common accessibility issues, explains what it found, suggests fixes, and produces a page-wise accessibility report. It is built to help content editors catch problems before they publish.
Note: This tool supports accessibility reviews. It does not certify WCAG compliance. Automated findings are reported as potential issues unless a rule can reliably establish a failure, and some checks (keyboard usability, link meaning in context) need human judgment.
Author: Koppula Sai Nihaas Reddy
Programme: Polaris School of Technology, OJT, Open Source Developer track
Mentor: Rohit Gupta
Status: In development (4-week OJT project)
Table of Contents
Problem Statement
Features
Tech Stack
How It Works
Accessibility Rules
Getting Started
Usage
Project Structure
Testing
Security and Reliability
Limitations
Roadmap
References
Contributing
Problem Statement
Content editors, publishers, and marketing teams often publish pages without realising they contain accessibility problems: images without alternative text, broken heading hierarchy, vague link text, and insufficient colour contrast. Manual checking is slow, inconsistent, and hard to scale across many pages.
This project provides automated checks with practical recommendations, helping teams build more inclusive pages and align better with the Web Content Accessibility Guidelines (WCAG).
Features
Core
Web Page Accessibility Scanner: accepts a page URL or raw HTML and analyses images, headings, and links.
Inline Issue Reporting and Quick-Fix Suggestions: highlights issues in the results with actionable suggestions (add meaningful alt text, fix heading order, replace vague link labels).
Colour Contrast Checker: enter foreground and background colours, get the contrast ratio, and see pass/fail against WCAG AA for normal and large text.
Keyboard Navigation Checklist: a manual checklist covering Tab navigation, visible focus indicators, logical focus order, and keyboard access to interactive elements.
Page-wise Accessibility Reports: per-page reports with findings, severity, recommendations, contrast results, checklist status, and WCAG references.
Audit History and Storage: audited pages, results, and reports are stored with Django models (SQLite) so previous audits can be reviewed.
Stretch Goals
Reusable Python module / standalone package, structured for possible integration into a Django CMS or a Wagtail-related contribution
85%+ test coverage, GitHub Actions CI, and linting
Downloadable HTML/CSV reports, severity filtering, and multi-page compliance summaries
Tech Stack
Area
Tools
Backend
Python 3, Django
Frontend
HTML, CSS, JavaScript
Database
SQLite
HTML parsing
Beautiful Soup (beautifulsoup4)
HTTP
requests (with timeouts, error handling, URL validation)
Contrast
Custom Python utility using WCAG relative luminance
Testing
Django test framework, unittest
Tooling
Git, GitHub, VS Code, Browser DevTools
How It Works
User Input → Input Validation → HTML Retrieval/Parsing → Accessibility Checks
          → Findings and Suggestions → Database Storage → Report Dashboard
​
Validate the input and handle invalid URLs, inaccessible pages, timeouts, and malformed HTML.
Fetch the page HTML or parse the supplied HTML.
Extract images, headings, links, and relevant text or colour information.
Run rules for alt text, heading hierarchy, and link text.
Calculate contrast ratios for user-provided colour pairs.
Attach recommendations, severity levels, and WCAG references to each finding.
Save audit results and checklist responses to the database.
Show a dashboard with issue totals, severity, detailed findings, and the final report.
Accessibility Rules
Check
What it looks for
Notes
Image alt text
Missing or empty alt attributes
Empty alt="" is valid for decorative images, so it is not automatically an error
Heading hierarchy
Skipped levels (e.g. H2 → H4) and questionable structure
Unconventional structure is flagged for review, not as a definite failure
Link text
Empty or generic labels (e.g. "click here")
Context-dependent clarity may need human review
Colour contrast
WCAG contrast ratio of foreground vs background
4.5:1 for normal text, 3:1 for large text (AA)
Keyboard navigation
Manual checklist
Cannot be fully verified by static analysis
Contrast formula:
Contrast Ratio = (L1 + 0.05) / (L2 + 0.05)
​
where L1 is the relative luminance of the lighter colour and L2 of the darker one.
Each rule is mapped to its relevant WCAG success criterion in the report.
Getting Started
Prerequisites
Python 3.10+
Git
pip (and ideally venv)
Installation
# 1. Clone the repository
git clone <https://github.com/><your-username>/<repo-name>.git
cd <repo-name>

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Apply database migrations
python manage.py migrate

# 5. (Optional) Create an admin user
python manage.py createsuperuser

# 6. Run the development server
python manage.py runserver
​
Then open http://127.0.0.1:8000/.
Usage
Open the app and choose New Audit.
Enter a public page URL, or paste HTML content.
Select the checks to run (or use the defaults).
Optionally enter foreground and background colours for the contrast checker.
Review the dashboard: issue counts, severity, findings, and suggested fixes.
Complete the keyboard navigation checklist for the page.
Open the page-wise report, or revisit it later from Audit History.
Project Structure
A planned layout, which may change as development progresses:
.
├── manage.py
├── requirements.txt
├── config/                 # Django project settings and URLs
├── audits/                 # Main Django app: models, views, forms, templates
│   ├── models.py           # Page, AuditSession, Finding, ChecklistResponse
│   ├── views.py
│   ├── forms.py
│   └── templates/
├── auditor/                # Reusable audit logic, independent of Django views
│   ├── fetcher.py          # Safe URL fetching and validation
│   ├── parser.py           # HTML extraction
│   ├── contrast.py         # Luminance and contrast calculations
│   └── rules/              # alt_text, headings, links
├── samples/                # Sample HTML pages used for demos and tests
├── tests/
└── docs/
​
Keeping the audit rules separate from the Django views is intentional, so the logic can later be packaged as a standalone module or plugin.
Testing
python manage.py test
​
With coverage:
pip install coverage
coverage run manage.py test
coverage report
​
Test plan
Unit: missing, empty, and meaningful alt text; valid and skipped heading sequences; empty, generic, and descriptive links; contrast calculations for known colour pairs; malformed HTML and empty pages.
Integration: views, audit logic, models, and templates working together; findings saved and displayed correctly; checklist responses linked to the right page.
End-to-end: submit a sample page and confirm the expected issues, suggestions, WCAG references, and final report.
Error handling and performance: unreachable URLs, HTTP errors, timeouts, unsupported content, repeat-audit consistency, and scan duration on small and medium pages.
Targets: 85%+ coverage (stretch), and under 3 seconds for local HTML parsing and rule execution on representative pages (excluding network latency).
Security and Reliability
Fetching external URLs carries risk, so the tool is designed to:
Validate URLs and reject unsafe targets
Apply request timeouts and response size limits
Handle HTTP errors, unsupported content types, and malformed HTML gracefully
Limitations
Static HTML analysis cannot judge everything. Keyboard usability and the meaning of link text in context require manual review.
Pages that render content with JavaScript may not be fully captured by a static fetch.
Automated results are potential issues, not a compliance verdict.
Roadmap
Week
Focus
1
Requirements, WCAG research, repo and environment setup, database schema
2
HTML parsing, alt/heading/link checks, contrast calculation, models, basic result views
3
Inline findings and suggestions, page-wise reports, keyboard checklist, error handling, expanded tests
4
Documentation, full test run, coverage, GitHub Actions, demo and final submission
References
WCAG 2.2
WAI: Images Tutorial
WAI: Headings Tutorial
WAI: Link Purpose (In Context)
WAI: Contrast (Minimum)
WAI-ARIA APG: Keyboard Interface
Django Documentation
Wagtail Documentation
Contributing
Contributions, issues, and suggestions are welcome. Please open an issue to discuss larger changes first, keep commits focused with meaningful messages, and include tests for new rules.
License
Add your chosen license here (for example, MIT) and include a LICENSE file in the reposito
