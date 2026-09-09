# MailTrace — Demo Output Walkthrough

This document explains what each MailTrace screen demonstrates for the SIH26106 presentation.

## 1. Dashboard

The dashboard is the analyst entry point. It shows investigation count, high-risk cases, observed indicators and local engine status.

**Demo interpretation:** a suspicious `.eml` is treated as evidence. The analyst can upload a file or load the safe phishing/benign demo.

![Dashboard](../public/demo/dashboard.svg)

## 2. Email Threat Analysis

The analysis screen converts the message into an explainable risk decision.

For the phishing demo, the screen shows:

- Threat score: **87/100** in the illustrative demo output.
- Classification: **CRITICAL / Phishing-Suspicious Email**.
- Sender and Reply-To identities.
- SPF, DKIM and DMARC results.
- Explainable reasons such as sender/reply-to mismatch, failed authentication and urgency language.
- Recommended analyst action: quarantine, preserve evidence and verify independently.

![Analysis](../public/demo/analysis.svg)

## 3. Authentication & Indicators

MailTrace parses authentication evidence already present in the message. It does not invent missing results.

The demo illustrates:

- SPF: FAIL
- DKIM: FAIL
- DMARC: FAIL
- Extracted IP: `203.0.113.42` (TEST-NET/example data)
- Extracted domains and URL indicators

The important SIH point is that authentication signals become evidence contributing to an explainable investigation rather than being shown as an unexplained AI verdict.

![Authentication](../public/demo/authentication.svg)

## 4. Threat Correlation Graph

The graph represents relationships among evidence types:

`Email → IP → Domain → URL → Attachment`

This is useful during forensic analysis because an analyst can move from one indicator to related indicators instead of reviewing every field independently.

![Threat Graph](../public/demo/threat-graph.svg)

## 5. Geolocation

The geolocation screen is deliberately labelled **approximate infrastructure intelligence**.

It should be explained during the SIH pitch as:

> IP geolocation can provide network/infrastructure context, but it is not an exact physical location and does not identify a person's home or identity.

The demo coordinates are illustrative and the IP is a reserved TEST-NET example address.

![Geolocation](../public/demo/geolocation.svg)

## 6. Forensic Report

The report view turns the investigation into a handoff document containing:

- Case identifier
- Threat classification and score
- Sender/recipient
- Subject
- Authentication status
- Detection reasons
- Extracted indicators
- Approximate geolocation context
- Recommended action
- Analyst notes

The prototype supports browser print / Save as PDF for demonstration.

![Forensic Report](../public/demo/report.svg)

## End-to-End SIH Flow

```text
Suspicious .eml
      ↓
Safe ingestion
      ↓
Header + content parsing
      ↓
SPF / DKIM / DMARC evidence
      ↓
Explainable threat scoring
      ↓
IP / domain / URL extraction
      ↓
Threat-intelligence boundary
      ↓
Approximate geolocation
      ↓
Indicator correlation
      ↓
Investigation notes
      ↓
Forensic report
```

## Security Boundaries

- Uploaded email is untrusted input.
- The prototype does not execute attachments.
- Extracted URLs are not opened automatically.
- Scripts from an email are never executed.
- Missing authentication evidence is represented as `UNKNOWN`.
- Demo intelligence is explicitly labelled as demo/fallback data.
- Geolocation is approximate infrastructure context, not attribution.
- A high score is an investigation signal, not proof of criminal intent.

## Running the Prototype

From the repository root:

```bash
python -m http.server 8080
```

Open `http://localhost:8080` in a browser and select **Try Demo Investigation**.

No npm install, database, API key or external service is required for the SIH demo path.
