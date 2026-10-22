# BioShield Web: Automated HTTP Header & CORS Security Auditor

**Author:** Sampriti Roy  
**Program:** IBM SkillsBuild Cyber Security BootCamp 2026 | BharatCares  
**Project type:** Beginner-friendly, Python-based web security auditing tool  
**Primary file:** `SampritiRoy_AutomatedWebScanner.ipynb`

---

## 1. Project Overview

**BioShield Web** is an educational Python project that checks a website's HTTP response for six commonly used security headers and performs a basic, passive check of its Cross-Origin Resource Sharing (CORS) response.

The tool sends an HTTP request to each configured website, reads the response headers, applies a simple rule-based scoring system, and creates a dark-themed HTML report. The report shows which security headers were found, which were missing, their stated purpose, and the result of the basic CORS check.

The project uses a **biology-inspired analogy** to make security concepts easier to understand. For example, a security header is compared to part of a cell's protective system. These analogies are learning aids; they do not mean that websites and biological cells work in the same way.

One configured target is the National Center for Biotechnology Information (NCBI), which makes the project relevant to a biotechnology student's interest in scientific and biological information resources. The other configured targets are GitHub and the standard example.com test domain.

> **Important:** This is a learning and preliminary review tool, not a complete penetration-testing system, a vulnerability scanner, or a guarantee that a website is secure.

## 2. Problem Statement

Websites use HTTP response headers and CORS policies to help browsers handle content, permissions, and cross-origin requests. A beginner may find these settings difficult to identify and interpret by reading raw response headers.

BioShield Web aims to make a small part of this process easier by automatically checking selected headers and presenting the findings in a readable report.

## 3. Objectives

The objectives of this project are to:

1. Learn how Python can send HTTP requests and read server responses.
2. Check for six selected HTTP security headers.
3. Identify some potentially concerning CORS response patterns using a test `Origin` value.
4. Calculate a simple, rule-based score from the checks included in the code.
5. Present results in a dark-themed HTML dashboard.
6. Make basic web-security concepts more approachable through plain-language explanations and biology-inspired analogies.
7. Demonstrate a beginner-level project that connects Python programming with the safe review of publicly accessible web responses.

## 4. Technology Stack

| Technology or library | Role in this project | Beginner-friendly explanation |
|---|---|---|
| Python 3 | Main programming language | Runs the scanner and processes the results. |
| `requests` | Sends HTTP requests | Requests a website and reads its response. |
| `urllib3` | Handles HTTP-related functionality used by the requests stack | The code uses it to suppress certain SSL warning messages. |
| `datetime` | Creates a report timestamp | Adds the date and time to the dashboard. |
| `IPython.display` | Optional notebook display | Displays the generated HTML inside a compatible Jupyter Notebook or Google Colab environment. |
| HTML | Report structure | Defines the dashboard's content. |
| CSS | Report appearance | Gives the dashboard its dark theme, tables, badges, and layout. |
| Jupyter Notebook / Google Colab | Suggested learning environment | Lets a beginner run and inspect Python code in notebook cells. |

### External services and datasets

- The code does not load a machine-learning model.
- The code does not use a training dataset.
- The scanner checks the live HTTP responses of the target websites configured in the notebook.
- The configured targets are `https://www.ncbi.nlm.nih.gov`, `https://github.com`, and `http://example.com`.

Website responses may change over time, so a later run may produce different results.

## 5. Security Checks Performed

### 5.1 HTTP security headers

The scanner checks whether each of these six headers is present in the response:

| Header | Purpose in simple terms | Missing-header penalty in the project's score |
|---|---|---:|
| `Strict-Transport-Security` | Tells browsers to use HTTPS for future connections under the policy's conditions. | 15 points |
| `Content-Security-Policy` | Helps restrict the sources from which a page can load certain resources, reducing some risks such as cross-site scripting. | 15 points |
| `X-Frame-Options` | Helps control whether a page may be displayed in a frame, reducing some clickjacking risks. | 10 points |
| `X-Content-Type-Options` | Helps prevent browsers from interpreting a response as a different content type. | 10 points |
| `Referrer-Policy` | Controls how much referrer information a browser sends. | 5 points |
| `Permissions-Policy` | Controls access to selected browser features and capabilities. | 5 points |

**Important interpretation note:** The current code checks whether a header exists; it does not fully validate whether the header's value is correctly configured. A header marked `PRESENT (SAFE)` therefore means only that the header was found by this check, not that its policy is necessarily secure.

The penalties above are the project's own simplified scoring choices. They are not an official industry rating.

### 5.2 CORS response check

CORS stands for **Cross-Origin Resource Sharing**. It is a browser mechanism that uses response headers to determine whether web pages from other origins may access certain responses.

The scanner sends a test `Origin` value:

`https://unauthorized-external-site.example`

It then examines `Access-Control-Allow-Origin` and `Access-Control-Allow-Credentials` in the response.

The code assigns these outcomes:

- **Critical-flaw warning:** The response reflects the test origin and allows credentials.
- **Warning:** The response reflects the test origin, but the credentials condition above is not met.
- **Wildcard warning:** The response uses `Access-Control-Allow-Origin: *`. This needs context; a wildcard is not automatically a vulnerability for every resource.
- **Secure/restrictive or no-wildcard label:** The response does not match the three patterns above.

These labels are preliminary signals, not proof that a CORS policy is safe or vulnerable. The check does not test every possible origin, request method, header, or browser behavior.

## 6. Workflow

The project follows this sequence:

1. **Load the checklist:** The program defines the six security headers, their stated purposes, risk labels, and biology-inspired analogies.
2. **Choose target websites:** The notebook contains three default reference targets.
3. **Prepare the request:** If a target has no URL scheme, the code adds `https://`. It then sends a request with a test `Origin` header.
4. **Read the response:** The program records the HTTP status code and response headers.
5. **Check the six headers:** Each header is marked present or missing. The program subtracts the configured penalty for each missing header.
6. **Review the CORS response:** The program checks the two CORS-related response headers against the rules described above.
7. **Calculate the score:** The result starts at 100, penalties are subtracted, and the displayed score is prevented from falling below 0.
8. **Build the dashboard:** HTML and CSS are generated to show the target, status code, score, header findings, and CORS result.
9. **Display and save the report:** In a compatible notebook, the dashboard is displayed inline. The code also writes `Security_Audit_Dashboard.html` to the current working directory.
10. **Review the result carefully:** Findings should be treated as preliminary observations and interpreted with knowledge of the website and its intended configuration.

## 7. Applications

Potential educational uses include:

- Learning how websites return HTTP response headers.
- Practising basic Python automation and response handling.
- Understanding the difference between a missing header and a header whose value needs further review.
- Learning the basic purpose of CORS.
- Producing a readable HTML summary for a small, authorized review.
- Introducing web-security concepts to students from non-computer-science backgrounds.
- Using a public scientific information website such as NCBI as a reference example, without accessing private accounts or data.

This tool is not designed to access private information, bypass security controls, exploit vulnerabilities, or make changes to a target website.

## 8. Project Structure

The following is a suggested GitHub repository layout. Not every item is generated automatically by the notebook.

```text
BioShield-Web/
├── SampritiRoy_AutomatedWebScanner.ipynb
├── README.md
├── requirements.txt
├── SampritiRoy_ProjectReport.docx
└── Security_Audit_Dashboard.html   # generated after running the notebook
```

### File descriptions

- `SampritiRoy_AutomatedWebScanner.ipynb` — the notebook containing the Python source code.
- `README.md` — this guide to the project.
- `requirements.txt` — the Python libraries needed to run the scanner.
- `SampritiRoy_ProjectReport.docx` — the written project report, if included in the final submission.
- `Security_Audit_Dashboard.html` — the report generated by a successful run. It may not exist until the notebook has been executed.

The project code does not automatically create the README, requirements file, or Word report. Those files must be saved separately.

## 9. Setup and Installation

### Option A: Run in Google Colab (beginner-friendly)

Google Colab is a browser-based notebook environment and can be convenient on a mobile phone.

1. Open Google Colab: https://colab.research.google.com/
2. Sign in with your Google account.
3. Upload `SampritiRoy_AutomatedWebScanner.ipynb`, or open it from your Google Drive if it is stored there.
4. Read the notebook before running it.
5. If the `requests` library is missing, run this in a separate code cell:

   ```python
   %pip install requests urllib3
   ```

6. Run the notebook cell.
7. Wait for the scan to finish. The code requests the configured websites, so an internet connection is required.
8. Look for the dashboard displayed in the notebook.
9. The HTML file is saved in the notebook's current working directory as `Security_Audit_Dashboard.html`.

**To download the generated HTML file in Colab**, run this in a new code cell after the scan has completed:

```python
from google.colab import files
files.download("Security_Audit_Dashboard.html")
```

If you are using an environment other than Google Colab, this download command will not work there.

### Option B: Run on a computer with Python

1. Install a supported Python 3 version.
2. Download or clone the project repository.
3. Open a terminal in the project folder.
4. Install the dependencies:

   ```bash
   python -m pip install -r requirements.txt
   ```

5. Open the notebook in Jupyter, or convert the scanner code into a Python file if you prefer to run it as a script.
6. Run the notebook or script while connected to the internet.
7. Find `Security_Audit_Dashboard.html` in the working directory.
8. Open the HTML file in a browser to view the saved report.

A compatible Jupyter environment may be installed with:

```bash
python -m pip install notebook
```

Then launch it from the project folder with:

```bash
jupyter notebook
```

### 9.1 `requirements.txt`

For the current scanner code, the main third-party dependencies are:

```text
requests
urllib3
```

`IPython` is used for displaying HTML inside a notebook, but the code treats it as optional. A typical Jupyter or Colab environment already includes it. The standard-library modules `datetime` and other built-in Python features do not need separate installation.

For a more reproducible submission, test the project in a clean environment and record the versions that work there.

### 9.2 Network and connection notes

- An internet connection is needed to request the target websites.
- A website may block automated requests, redirect the request, or be temporarily unavailable.
- A connection error does not mean the website is insecure.
- The request timeout in the code is 10 seconds.
- The code uses normal certificate verification with `verify=True`. It also suppresses some `urllib3` SSL warning messages; suppressing a warning does not repair a certificate problem.

## 10. How to Read the Output

The dashboard contains:

- **Target:** The website being checked.
- **HTTP status:** The response status returned by the server.
- **Security score:** A score produced by the project's simple rule-based formula.
- **Security header:** The name of a header being checked.
- **Audit status:** Whether the header was found or was missing.
- **Configured value:** A shortened version of the response header value, or `Not Configured`.
- **Role and bio-defense analogy:** A brief explanation of the header's purpose.
- **CORS audit:** The observed `Access-Control-Allow-Origin` value and the label assigned by the project's CORS rules.
- **Connection error:** A message shown when the request raises an exception.

### Understanding the score

The score starts at **100**. The program subtracts points for missing headers and selected CORS patterns, then limits the minimum score to 0.

The score is a **custom educational indicator**, not a validated security standard. It does not account for every possible configuration issue, vulnerability, asset, or threat. Two websites with the same score may have very different security situations.

A high score does not prove a site is safe, and a low score does not prove it has been compromised.

## 11. Model Evaluation

**This project does not use a machine-learning model.** It uses explicit, rule-based checks. Therefore, machine-learning metrics such as accuracy, precision, recall, F1-score, training loss, and test-set performance are not applicable to the current implementation.

The current code's behavior can instead be evaluated with functional tests:

1. **Header-present test:** Use a controlled test server that returns a selected header and confirm that the scanner reports it as present.
2. **Header-missing test:** Use a controlled test server that omits a selected header and confirm that the scanner reports it as missing and applies the intended penalty.
3. **CORS reflection test:** In a controlled environment, return the test origin in `Access-Control-Allow-Origin` and check that the appropriate warning is produced.
4. **Credential condition test:** In a controlled environment, vary `Access-Control-Allow-Credentials` and check the branch selected by the code.
5. **Wildcard test:** Return `Access-Control-Allow-Origin: *` and confirm the wildcard outcome.
6. **Connection-failure test:** Use an intentionally unavailable local test endpoint and confirm that the program reports an error rather than presenting a normal completed audit.
7. **Score-boundary test:** Check that the displayed score never drops below 0.
8. **Report-generation test:** Confirm that the HTML report is created and includes the expected target names and findings.

These tests should be carried out against a local test server or a system for which permission has been granted. Record the test setup, expected outcome, actual outcome, and any differences in the project report.

No measured test results are claimed in this README. Results should be added only after the tests have actually been run and documented.

## 12. Limitations

1. **Limited checklist:** The scanner checks only six HTTP security headers and two CORS response headers.
2. **Presence is not correctness:** It checks whether a security header exists, but does not comprehensively validate the policy value.
3. **Simplified CORS logic:** CORS behavior is more complex than the patterns included in this project. The current labels can produce false alarms or miss issues.
4. **No full vulnerability testing:** The tool does not test for SQL injection, cross-site scripting, authentication problems, access-control flaws, or other vulnerability classes.
5. **No JavaScript/browser analysis:** It does not run a browser or inspect how a page behaves after loading.
6. **No machine learning:** There is no trained model, training dataset, or predictive capability.
7. **Single-request approach:** Each target is checked with a basic GET request. Some sites may respond differently to other paths, methods, sessions, or browser contexts.
8. **Network-dependent results:** Results depend on the website's current response, connectivity, redirects, and server behavior.
9. **Custom scoring:** The score is a simplified project-specific calculation and should not be interpreted as an official security rating.
10. **HTML output handling:** Values from HTTP responses are inserted into the generated HTML without comprehensive HTML escaping. Before using this tool with untrusted targets or sharing generated reports, output handling should be improved.
11. **SSL warning suppression:** Suppressing warning messages can make diagnostic issues less visible. It does not disable certificate verification in the current request, which uses `verify=True`.
12. **Not a substitute for expert review:** A qualified security professional should evaluate important systems and interpret findings in context.

## 13. Future Improvements

Possible improvements for a future version include:

- Validate the actual values of security headers, not just their presence.
- Expand the CORS checks and explain the context required to interpret each result.
- Add unit tests with controlled, local mock responses.
- Add clearer handling for redirects, timeouts, invalid URLs, and different HTTP errors.
- Use HTML escaping when inserting server-provided values into the report.
- Add a downloadable JSON or CSV summary for further analysis.
- Include a clear explanation of how each score is calculated.
- Improve accessibility and mobile responsiveness of the dashboard.
- Record dependency versions for reproducibility.
- Allow users to enter an authorized target instead of editing the source code.
- Add a timestamp and project version to saved reports.
- Compare results across repeated authorized scans while making clear that website configurations can change.
- Add a disclaimer to the dashboard explaining that results are preliminary and do not certify a website's security.

These are proposed improvements, not features already implemented in the current code.

## 14. Responsible Use and Ethics

BioShield Web is intended for education and authorized, low-impact review.

- Scan only websites you own or have clear permission to assess, and follow their terms and applicable policies.
- For practice, prefer a local test server or an environment specifically provided for security training.
- Do not use the project to bypass access controls, exploit vulnerabilities, disrupt services, or access private information.
- Do not treat the score or any status label as proof that a website is safe or unsafe.
- Avoid repeated or high-volume requests. This code makes one basic request per configured target during a normal run, but users should still respect the target's policies.
- Review the report before sharing it. Do not include credentials, private URLs, tokens, or other sensitive information.
- Do not present findings as confirmed vulnerabilities unless they have been properly verified.
- Use the project as a learning aid and seek qualified review before making security decisions.

## 15. Author

**Sampriti Roy**  
B.Sc. Biotechnology student  
IBM SkillsBuild Cyber Security BootCamp 2026 | BharatCares

This project explores Python-based web-security concepts through a beginner-friendly approach that connects technology learning with a biotechnology background.

## 16. Conclusion

BioShield Web demonstrates how a small Python program can request public website responses, inspect selected HTTP security headers and CORS-related values, apply transparent rule-based checks, and produce a readable HTML report.

For a biotechnology student who is beginning to learn coding, the project offers practice with Python libraries, web requests, conditional logic, scoring, and report generation. Its results should be understood as an introductory educational audit—not as a complete or definitive assessment of website security.

## 17. References and Learning Resources

- Python documentation: https://docs.python.org/3/
- Requests documentation: https://requests.readthedocs.io/
- MDN Web Docs — HTTP headers: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers
- MDN Web Docs — Cross-Origin Resource Sharing (CORS): https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
- Jupyter documentation: https://docs.jupyter.org/
- Google Colab: https://colab.research.google.com/
- National Center for Biotechnology Information (NCBI): https://www.ncbi.nlm.nih.gov/
- GitHub documentation: https://docs.github.com/

---

**Project status:** Educational project / preliminary rule-based web response audit.  
**Reproducibility note:** Run the notebook in a connected environment to generate a fresh dashboard. Website responses and results can change over time.
