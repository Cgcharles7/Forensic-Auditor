CookieForensics: The "Browsergate" Auditor 🕵
CookieForensics is a Python-based standalone auditor designed to uncover hidden, high-entropy, and long-lived tracking identifiers that websites leave behind.

Unlike standard browser tools, this script uses Shannon Entropy and Persistence Heuristics to distinguish between "strictly necessary" session cookies and "excessive" tracking "residue" that mimics malware behavior.

The Mission
Modern web tracking has evolved beyond simple "analytics." As seen in recent industry controversies, companies often use cryptic, high-entropy strings to "fingerprint" users. This tool allows developers to:

Quantify Hostility: Measure how much "data exhaust" a site generates.

Detect "Immortal" Cookies: Identify trackers set to expire decades into the future.

Analyze Entropy: Mathematically prove if a cookie value is a simple toggle or a unique tracking UUID.

How it Works
The auditor launches a headless browser via Playwright, simulates a real user visit, and performs a multi-point inspection on every piece of data dropped:

1. Entropy Analysis (Randomness Check)
We calculate the randomness of every cookie value.

Low Entropy (< 3.0): Likely functional (e.g., theme=dark, lang=en).

High Entropy (> 4.5): Likely a unique identifier/UID.

2. Persistence Check (The "Greed" Metric)
The script calculates the lifespan of the cookie relative to the current time.

Any cookie living longer than 2 years is flagged as Aggressive.

Any cookie living longer than 20 years is flagged as Immortal/Malicious.

3. Naming Heuristics
Identifies patterns common in tracking pixels and "Shadow Storage" (LocalStorage/IndexedDB) that are designed to survive a standard cookie clearing.

Installation
Bash
# Clone the repository
git clone https://github.com/your-username/cookie-forensics.git

# Install dependencies
pip install playwright
playwright install chromium

# Run your first audit
python auditor.py
Data Output
The tool generates a JSON report for every audited URL, allowing you to build a queryable database of site hostility:

JSON
{
    "name": "li_sugr",
    "entropy": 4.82,
    "lifespan_days": 730,
    "classification": "Excess/Tracking",
    "flags": {
        "aggressive_persistence": true,
        "high_entropy": true
    }
}
Roadmap
[ ] Cross-Regional Comparison: Audit sites from different IP addresses (VPN) to see how tracking changes under GDPR.

[ ] Shadow Storage Audit: Expanding detection to localStorage and IndexedDB.

[ ] Hostility Scoring: Implementing a 1-100 "Privacy Hostility Score."

⚖️ License
MIT - Go forth and audit.
