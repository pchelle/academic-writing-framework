---
name: content-access
description: Legal methods for accessing paywalled and geo-blocked content. Use when researching behind paywalls, accessing academic papers, bypassing geographic restrictions, or finding open access alternatives. Covers Unpaywall, library databases, VPNs, and ethical access strategies for journalists and researchers.
source: https://github.com/jamditis/claude-skills-journalism/tree/main/content-access
---

# Content access methodology

Ethical and legal approaches for accessing restricted web content for journalism and research.

## Access hierarchy (most to least preferred)

```
┌─────────────────────────────────────────────────────────────────┐
│              CONTENT ACCESS DECISION HIERARCHY                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. FULLY LEGAL (Always try first)                              │
│     ├─ Library databases (PressReader, ProQuest, JSTOR)         │
│     ├─ Open access tools (Unpaywall, CORE, PubMed Central)     │
│     ├─ Author direct contact                                    │
│     └─ Interlibrary loan                                        │
│                                                                  │
│  2. LEGAL (Browser features)                                    │
│     ├─ Reader Mode (Safari, Firefox, Edge)                      │
│     ├─ Wayback Machine archives                                 │
│     └─ Google Scholar "All versions"                            │
│                                                                  │
│  3. GREY AREA (Use with caution)                               │
│     ├─ Archive.is for individual articles                       │
│     ├─ Disable JavaScript (breaks functionality)                │
│     └─ VPNs for geo-blocked content                            │
│                                                                  │
│  4. NOT RECOMMENDED                                             │
│     ├─ Credential sharing                                       │
│     ├─ Systematic scraping                                      │
│     └─ Commercial use of bypassed content                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Open access tools for academic papers

### Unpaywall browser extension

Unpaywall finds free, legal copies of 20+ million academic papers.

```python
# Unpaywall API (free, requires email for identification)
import requests

def find_open_access(doi: str, email: str) -> dict:
    """Find open access version of a paper using Unpaywall API.

    Args:
        doi: Digital Object Identifier (e.g., "10.1038/nature12373")
        email: Your email for API identification

    Returns:
        Dict with best open access URL if available
    """
    url = f"https://api.unpaywall.org/v2/{doi}?email={email}"

    response = requests.get(url, timeout=30)

    if response.status_code != 200:
        return {'error': f'Status {response.status_code}'}

    data = response.json()

    if data.get('is_oa'):
        best_location = data.get('best_oa_location', {})
        return {
            'is_open_access': True,
            'oa_url': best_location.get('url_for_pdf') or best_location.get('url'),
            'oa_status': data.get('oa_status'),  # gold, green, bronze, hybrid
            'host_type': best_location.get('host_type'),  # publisher, repository
            'version': best_location.get('version')  # publishedVersion, acceptedVersion
        }

    return {
        'is_open_access': False,
        'title': data.get('title'),
        'journal': data.get('journal_name')
    }

# Usage
result = find_open_access("10.1038/nature12373", "researcher@example.com")
if result.get('is_open_access'):
    print(f"Free PDF at: {result['oa_url']}")
```

### CORE API (295M papers)

```python
# CORE API - requires free API key from https://core.ac.uk/

import requests

class CORESearch:
    def __init__(self, api_key: str):
        self.api_key = api_key
        self.base_url = "https://api.core.ac.uk/v3"

    def search(self, query: str, limit: int = 10) -> list:
        """Search CORE database for open access papers."""

        headers = {'Authorization': f'Bearer {self.api_key}'}
        params = {
            'q': query,
            'limit': limit
        }

        response = requests.get(
            f"{self.base_url}/search/works",
            headers=headers,
            params=params,
            timeout=30
        )

        if response.status_code != 200:
            return []

        data = response.json()
        results = []

        for item in data.get('results', []):
            results.append({
                'title': item.get('title'),
                'authors': [a.get('name') for a in item.get('authors', [])],
                'year': item.get('yearPublished'),
                'doi': item.get('doi'),
                'download_url': item.get('downloadUrl'),
                'abstract': item.get('abstract', '')[:500]
            })

        return results

    def get_by_doi(self, doi: str) -> dict:
        """Get paper by DOI."""
        headers = {'Authorization': f'Bearer {self.api_key}'}

        response = requests.get(
            f"{self.base_url}/works/{doi}",
            headers=headers,
            timeout=30
        )

        return response.json() if response.status_code == 200 else {}
```

### Semantic Scholar API (214M papers)

```python
# Semantic Scholar API - free, no key required for basic use

import requests

def search_semantic_scholar(query: str, limit: int = 10) -> list:
    """Search Semantic Scholar for papers with open access links."""

    url = "https://api.semanticscholar.org/graph/v1/paper/search"
    params = {
        'query': query,
        'limit': limit,
        'fields': 'title,authors,year,abstract,openAccessPdf,citationCount'
    }

    response = requests.get(url, params=params, timeout=30)

    if response.status_code != 200:
        return []

    results = []
    for paper in response.json().get('data', []):
        oa_pdf = paper.get('openAccessPdf', {})
        results.append({
            'title': paper.get('title'),
            'authors': [a.get('name') for a in paper.get('authors', [])],
            'year': paper.get('year'),
            'citations': paper.get('citationCount', 0),
            'open_access_url': oa_pdf.get('url') if oa_pdf else None,
            'abstract': paper.get('abstract', '')[:500] if paper.get('abstract') else ''
        })

    return results

def get_paper_by_doi(doi: str) -> dict:
    """Get paper details by DOI."""
    url = f"https://api.semanticscholar.org/graph/v1/paper/DOI:{doi}"
    params = {
        'fields': 'title,authors,year,abstract,openAccessPdf,references,citations'
    }

    response = requests.get(url, params=params, timeout=30)
    return response.json() if response.status_code == 200 else {}
```

## Library database access

### Checking library access programmatically

```python
# Most library databases require authentication
# This shows how to structure library API access

class LibraryAccess:
    """Access pattern for library databases."""

    # Common library database endpoints
    DATABASES = {
        'pressreader': {
            'base': 'https://www.pressreader.com',
            'auth': 'library_card',
            'content': '7000+ newspapers/magazines'
        },
        'proquest': {
            'base': 'https://www.proquest.com',
            'auth': 'institutional',
            'content': 'news, dissertations, documents'
        },
        'jstor': {
            'base': 'https://www.jstor.org',
            'auth': 'institutional',
            'content': 'academic journals, books'
        },
        'nexis_uni': {
            'base': 'https://www.nexisuni.com',
            'auth': 'institutional',
            'content': 'legal, news, business'
        }
    }

# Interlibrary Loan (ILL) workflow
def request_via_ill(paper_info: dict, library_email: str) -> str:
    """Generate interlibrary loan request.

    ILL is free through most libraries and can get almost any paper.
    Turnaround: typically 3-7 days.
    """

    request = f"""
    INTERLIBRARY LOAN REQUEST

    Title: {paper_info.get('title')}
    Author(s): {paper_info.get('authors')}
    Journal: {paper_info.get('journal')}
    Year: {paper_info.get('year')}
    DOI: {paper_info.get('doi')}
    Volume/Issue: {paper_info.get('volume')}/{paper_info.get('issue')}
    Pages: {paper_info.get('pages')}

    Requested by: {library_email}
    """

    return request.strip()
```

## Direct author contact

### Email template for paper requests

```python
def generate_paper_request_email(paper: dict, requester: dict) -> str:
    """Generate professional email requesting paper from author.

    Authors are typically happy to share their work.
    Success rate: Very high (70-90%).
    """

    template = f"""
Subject: Request for paper: {paper['title'][:50]}...

Dear Dr./Prof. {paper['author_last_name']},

I am a {requester['role']} at {requester['institution']}, researching
{requester['research_area']}.

I came across your paper "{paper['title']}" published in
{paper['journal']} ({paper['year']}), and I believe it would be
highly relevant to my work on {requester['specific_project']}.

Unfortunately, I don't have access through my institution. Would you
be willing to share a copy?

I would be happy to properly cite your work in any resulting publications.

Thank you for your time and for your contribution to the field.

Best regards,
{requester['name']}
{requester['title']}
{requester['institution']}
{requester['email']}
"""

    return template.strip()
```

## Wayback Machine for historical access

```python
def get_wayback_article(url: str) -> str:
    """Get article from Wayback Machine.

    100% legal - the Internet Archive is a recognized library.
    May have older versions of articles (before paywall implemented).
    """

    # Check availability
    api_url = f"http://archive.org/wayback/available?url={url}"

    try:
        response = requests.get(api_url, timeout=10)
        data = response.json()

        snapshot = data.get('archived_snapshots', {}).get('closest', {})

        if snapshot.get('available'):
            return snapshot['url']

        return None
    except Exception:
        return None
```

## Access strategy by content type

### Academic papers

```markdown
## Academic paper access strategies (in order)

1. **Unpaywall extension** - Check first, automatic
2. **Google Scholar** - Click "All versions", look for [PDF]
3. **Author's website** - Check their academic page
4. **Institutional repository** - Search university library
5. **Preprint servers** - arXiv, SSRN, bioRxiv, medRxiv
6. **ResearchGate/Academia.edu** - Author-uploaded copies
7. **CORE.ac.uk** - 295M open access papers
8. **PubMed Central** - For biomedical papers
9. **Contact author directly** - High success rate
10. **Interlibrary Loan** - Free, gets almost anything
```

### Books and reports

```markdown
## Book/report access strategies

1. **Library digital lending** - Internet Archive, OverDrive
2. **Google Books** - Often has preview or full text
3. **HathiTrust** - Academic library consortium
4. **Project Gutenberg** - Public domain books
5. **OpenLibrary** - Internet Archive's book lending
6. **Publisher open access** - Some chapters/reports free
7. **Author/organization website** - Reports often available
8. **Interlibrary Loan** - Physical books, scanned chapters
```

## Legal and ethical framework

### Best practices for researchers

```markdown
## Ethical content access guidelines

### DO:
- Use library resources first (supports the ecosystem)
- Try open access tools before circumvention
- Contact authors directly (they want citations)
- Cite properly regardless of how you accessed content
- Budget for subscriptions to frequently-used sources

### DON'T:
- Share login credentials
- Systematically download entire databases
- Use bypassed content for commercial purposes
- Redistribute paywalled content
- Rely solely on bypass methods
```
