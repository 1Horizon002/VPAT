

---

# Practical No. 03: Google Dorking for Passive Reconnaissance

## 🎯 Aim
To study and perform Google Dorking techniques for passive information gathering and to understand how misconfigured web resources can unintentionally expose sensitive information.

---

## 📖 Theory for Oral Exams
**What is Google Dorking?**
* Also known as **Google Hacking**.
* A **passive reconnaissance** technique that uses advanced search operators to find sensitive information indexed by search engines.
* It **only** accesses publicly available data (does not interact directly with the target server).

**What can it reveal?**
* Exposed admin login pages.
* Configuration files (`.env`, `.conf`).
* Backup databases (`.sql`, `.bak`).
* Sensitive documents (`.pdf`, `.xls`, `.doc`).
* Detailed error messages providing server paths.

---

## 🛠️ Advanced Search Operators
| Operator | Purpose |
| :--- | :--- |
| `site:` | Restricts results to a specific website or domain. |
| `intitle:` | Searches for specific text within the page title. |
| `inurl:` | Searches for specific text within the URL. |
| `filetype:` / `ext:` | Searches for specific file extensions (PDF, SQL, LOG). |
| `cache:` | Displays the version of the page stored in Google's cache. |
| `"` | Forces an exact match search for a phrase. |

---

## 💻 Procedure & Command List
Perform these queries in a standard Google search bar, replacing `example.com` with your legal target (e.g., `testphp.vulnweb.com`).

1. **Find Sensitive Documents:**
   * `site:example.com filetype:pdf`
   * `site:example.com filetype:xls`
2. **Find Configuration & Environment Files:**
   * `site:example.com ext:conf`
   * `site:example.com ext:env` (High Risk!)
3. **Detect Admin Panels:**
   * `site:example.com inurl:admin`
   * `site:example.com inurl:dashboard`
4. **Discover Backups & SQL Leaks:**
   * `site:example.com ext:bak`
   * `site:example.com ext:sql`
5. **Find Server Error Messages:**
   * `site:example.com "SQL syntax error"`
   * `site:example.com "Warning: mysql"`

---

## 📊 Observation Table (Advanced Dorks)
| Search Query | Potential Observation | Security Risk |
| :--- | :--- | :--- |
| `intitle:"Index of /password"` | Directory listing of passwords | **Critical** (Credential Leak) |
| `ext:java intext:"executeUpdate"` | Source code showing DB queries | **High** (SQL Injection Risk) |
| `site:s3.amazonaws.com` | Publicly accessible cloud buckets | **High** (Data Breach) |
| `filetype:pem "private key"` | Exposed SSH private keys | **Critical** (Server Takeover) |
| `inurl:test/login` | Exposed test environment logins | **Medium** (Unauthorized Access) |

---

## ❓ Oral Exam (Viva) Preparation

**Q1: Is Google Dorking illegal?**
* **Ans:** No. It uses publicly available information already indexed by Google. However, using this information to attempt an unauthorized login or attack is illegal.

**Q2: Why are `.env` files extremely dangerous?**
* **Ans:** They often contain plain-text passwords, API keys, and database connection strings. If indexed, anyone can gain full access to the backend.

**Q3: How can an organization prevent Google Dorking?**
* **Ans:** 1. Use a `robots.txt` file to tell search engines what *not* to index.
  2. Implement proper access control (Authentication) for sensitive directories.
  3. Use the `noindex` meta tag on sensitive pages.

**Q4: What is `robots.txt`?**
* **Ans:** It is a text file placed in the root directory of a website that provides instructions to web robots (crawlers) about which pages should not be crawled or indexed.

---

## ✅ Conclusion
The experiment demonstrates that Google Dorking is a high-impact reconnaissance method. It highlights how organizational negligence regarding file permissions and search engine indexing can lead to the exposure of high-risk assets like SSH keys, financial invoices, and internal audit logs.

---

**Exam Tip:** If the examiner asks you to demonstrate a "dork" live, use something safe like `site:edu.in filetype:pdf "scholarship"`—it shows you understand the syntax without looking like you're trying to "hack" the lab network! 😈

