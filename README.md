# Phishing
TryHackMe Phishing Lab - Full phishing campaign simulation using GoPhish, OSINT, spoofed domains, and HTML harvesters for red team ops.

By Ramyar Daneshgar 

---

**Objective:** Simulate and execute a full phishing engagement, from reconnaissance to payload delivery, using GoPhish and realistic attack vectors.

---

## **Task 01: Brief**

This lab provided a controlled environment to replicate a real-world phishing attack lifecycle. As part of the exercise, I engaged in reconnaissance, created persuasive phishing emails, deployed a spoofed login page, and leveraged tools such as GoPhish for analytics and automation.

---

## **Task 02: Introduction to Phishing Attacks**

Phishing is a **subdomain of social engineering**, focused on exploiting **cognitive biases and emotional triggers**—such as fear, urgency, and curiosity—delivered via deceptive emails. The most effective vector in red teaming is **spear-phishing**, where messages are highly targeted using contextual data sourced through OSINT (Open Source Intelligence).

### Key Takeaways:
- **Phishing = Social Engineering via Email**
- **Spear-Phishing = Highly targeted attacks**
- **Variants**: Smishing (SMS), Vishing (Voice), Whaling (C-level execs)

---

## **Task 03: Writing Convincing Phishing Emails**

Effective phishing emails must manipulate three elements:
1. **Sender Address** – ideally spoofed or typosquatted domains.
2. **Subject Line** – emotionally charged or urgent.
3. **Content** – visually authentic with disguised anchor text using `<a href="malicious.link">legit-looking text</a>`

To maximize success rates, I employed **OSINT** to identify brands, colleagues, or suppliers relevant to the target. For instance, mimicking internal emails referencing "payroll" or "IT updates" increases the likelihood of interaction.

---

## **Task 04: Phishing Infrastructure**

I built a modular phishing infrastructure consisting of:
- **Domain Name** – spoofed or typosquatted (e.g., `admin-acmeit[.]support`)
- **SSL/TLS Certificate** – using Let's Encrypt for legitimacy
- **SMTP Server** – local `127.0.0.1:25` via GoPhish for controlled delivery
- **DNS Records** – SPF/DKIM/DMARC to improve deliverability
- **Web Server** – hosted spoofed HTML login page
- **Analytics** – GoPhish campaign monitoring

I leveraged **GoPhish** for its campaign automation, email template WYSIWYG editor, and data collection. The **Social Engineering Toolkit (SET)** was noted as an alternative with advanced spoofing and payload generation features.

---

## **Task 05: Using GoPhish**

I set up and launched a live phishing campaign using the following components:
- **Sending Profile**: Localhost SMTP from `noreply@redteam.thm`
- **Landing Page**: A spoofed ACME IT Support login page with credential capture enabled
- **Email Template**: Custom HTML with embedded `{{.URL}}` replaced by campaign-specific landing URLs
- **Target Group**: Three test users at `@acmeitsupport.thm`
- **Campaign Settings**: Configured with backdated timestamps and tracked metrics (opens, clicks, credentials submitted)

Upon launch:
- Two emails were successfully delivered.
- One was rejected due to an invalid recipient.
- Credential submission from a target was captured, validating success.

---

## **Task 06: Droppers**

I studied **droppers**, which are initial-stage malware carriers. While they appear benign and bypass AV checks, once executed, they:
- Connect to C2 infrastructure
- Retrieve and deploy second-stage payloads (RATs, keyloggers, etc.)
- Establish persistence and remote control

Dropper deployment can be integrated via phishing by disguising it as a software update or required codec.

---

## **Task 07: Choosing A Phishing Domain**

Domain reputation significantly affects deliverability and trust. I explored:
- **Expired Domains**: Better spam filter scores due to history
- **Typosquatting**: Subtle variations (e.g., `go0gle.com`)
- **TLD Substitution**: `example[.]co` vs `example[.]com`
- **IDN Homograph Attacks**: Exploiting visually indistinguishable Unicode characters to mimic trusted domains

This understanding is vital for red team operations seeking stealth and credibility.

---

## **Task 08: Using MS Office in Phishing**

Microsoft Office documents are common phishing vectors due to their support for **macros** (VBA scripting). I examined how a spreadsheet attachment can:
- Prompt the user to "Enable Content"
- Trigger malicious code execution (e.g., PowerShell scripts, DLL injection)
- Establish a backdoor

Red teams often spoof internal HR communications to lure users into opening infected docs (e.g., salary spreadsheets or policy updates).

---

## **Task 09: Using Browser Exploits**

While rare in modern environments, **browser exploits** remain viable against legacy systems. Exploits such as **CVE-2021-40444** allowed RCE through crafted Office files leveraging malicious ActiveX controls.

These attacks succeed when:
- Victims are coerced into visiting attacker-controlled sites
- Vulnerable browsers (or embedded Office components) execute arbitrary code

Key lesson: recon the target environment before deploying this vector, as it's often patch-dependent.

---

## **Task 10: Phishing Practical**

In the final task, I evaluated phishing emails via a simulated inbox. Key identifiers included:
- Inconsistent sender domains
- Suspicious or mismatched URLs
- Unexpected attachments (Office files, executables)
- Spelling errors or urgent language patterns

This exercise emphasized attention to detail and validated earlier technical concepts.

---

# **Lessons Learned**

###  **OSINT is Foundational**
Understanding a target’s digital footprint is critical for crafting believable spear-phishing content.

###  **Domain & Infrastructure Matters**
Using expired domains with proper TLS and DNS configuration significantly improves deliverability and believability.

###  **Tool Proficiency**
GoPhish proved invaluable for orchestrating full-fledged phishing simulations with real-time analytics and payload delivery.

###  **Psychological Engineering is the Real Exploit**
Humans remain the weakest link. Leveraging urgency, familiarity, and curiosity enables high conversion rates in phishing campaigns.

###  **Defense Perspective**
From a Blue Team standpoint, this room reinforced the importance of:
- DNS filtering
- Employee awareness training
- Macro execution restrictions
- Secure email gateways with DMARC enforcement
