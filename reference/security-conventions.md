# Security Conventions

The security rules a team using AI in development should adopt. These are starting-point defaults — adjust to your regulatory context, but **not loosely.** Start strict, loosen only with explicit sign-off.

---

## The core principle

**Treat every AI tool as an untrusted external service** until you've verified otherwise. That means:

- Data you send it may be stored, logged, used for training, or exposed in a breach.
- The tool itself may be compromised (supply-chain attack, rogue employee, misconfigured server).
- Even "private" or "enterprise" tiers have failure modes — default-settings change, API endpoints are reachable, and conversation history is retained somewhere.

Build the boundary at the data, not at the tool's reputation.

---

## What never leaves the local environment

These are non-negotiable across virtually every team:

- Production `.env` values
- API keys, tokens, OAuth secrets
- Database credentials
- Customer personally identifiable information (PII) — names, emails, addresses, phone numbers, government IDs
- Financial data — card numbers, account numbers, transaction details
- Health data — medical records, prescription data, clinical notes
- Employee personal information
- Internal strategy documents, acquisition targets, legal proceedings
- Contents of secrets managers (Vault, AWS Secrets Manager, etc.)

"Leaves the local environment" means:

1. Sent to any cloud-based AI tool (ChatGPT, Claude.ai, Copilot chat, Cursor cloud features)
2. Passed through a third-party API that relays to an LLM provider
3. Stored in a memory system, conversation history, or log that isn't on your local machine

This is stricter than "don't paste into ChatGPT." Many tools send conversations to cloud providers even when they feel local.

---

## What is safe

- Code structure and architecture
- Publicly-documented library or API usage
- Error messages with sensitive details redacted (replace IDs, emails, secrets with placeholders)
- Fake fixture data (verify it's fake, not anonymized-real)
- Open-source dependency code
- Your own public GitHub projects

When in doubt, redact. "Was this safe to paste?" is a better question to ask before you paste than after.

---

## Privacy-regulation implications

Depending on jurisdiction and data type:

### GDPR (EU)

- Customer data cannot leave the EU without compliance mechanisms (Standard Contractual Clauses, adequacy decisions).
- AI tools that store conversation data may count as **processors** under GDPR.
- Data subject access requests (Article 15) may extend to AI conversation logs if personal data is in them.
- Right to erasure (Article 17) may require you to delete AI conversation history.

**Practical:** assume EU customer data touching any cloud AI tool is a compliance issue unless you've confirmed otherwise with legal.

### CCPA (California)

- Similar to GDPR for California residents. Less stringent on cross-border but similar on disclosure and deletion rights.

### HIPAA (US, healthcare)

- Protected Health Information (PHI) cannot go to an AI tool unless the tool vendor has signed a Business Associate Agreement (BAA).
- Most consumer AI tools do not have BAAs. Assume PHI is a red line.

### PCI DSS (payment cards)

- Cardholder data cannot be processed by unauthorized systems. Most AI tools are unauthorized.
- Even test data should not contain real card numbers — use validated test numbers from card network documentation.

### Industry-specific (legal, finance, defense, etc.)

- Your industry likely has stricter rules than any of the above. Check with compliance before AI tools touch anything.

---

## Local-only is often fine

**MCP servers that query your local database:** fine. The data doesn't leave your machine.

**MCP servers that call an internal API:** fine if the internal API is within your network boundary.

**MCP servers that call a third-party service:** evaluate. Is the service trusted with the data you're passing it?

The distinction that matters: **does the tool operate entirely within your machine's trust boundary, or does it send data across it?**

---

## New tool approval process

Before a new AI tool gets access to company code or data, require:

1. **Data flow diagram.** What data does the tool receive? Where does it go? Who sees it?
2. **Data retention policy.** How long does the tool store conversations, code, or outputs?
3. **Model training policy.** Does the tool train on customer submissions by default? Can this be disabled?
4. **Security certifications.** SOC 2, ISO 27001, or equivalent for anything touching production data.
5. **Data Processing Agreement** (DPA) if handling regulated data.

If a tool can't provide answers to these, it doesn't get company-data access. Individuals using it for non-work is a separate question.

### Common red flags

- "We use your data to improve our models" with no opt-out.
- No data retention policy, or "we retain conversations indefinitely."
- No regional data residency options if you're in the EU.
- Free tier with terms that are stricter than they appear.
- Browser-extension-based tools that read page content (could include internal dashboards).

---

## Specific workflow rules

### The CLI agent

- Can read and write your local filesystem. Treat its output like your own — if you wouldn't commit it, don't accept it.
- May send code snippets to the model provider. Assume anything it sees, the provider sees.
- May cache conversation history locally. Check your tool's docs for where.

### Chat AI

- Everything you paste is sent to the provider. Including the "just to get a quick opinion" paste.
- Conversation history is typically retained for some period. Audit your tool's settings for "train on my conversations" options and disable them.

### In-editor AI

- May send file contents (potentially whole files, not just selections) to the model provider. Check the tool's docs.
- May have "workspace features" that index your entire project. Know what's indexed and where it's stored.

### Browser-automation tools

- Can read the contents of any page you navigate to. If you navigate to an internal dashboard with customer data, the tool sees it.
- May take screenshots that include sensitive information. Know where screenshots are stored.

---

## Incident response

If you accidentally sent sensitive data to an AI tool:

1. **Don't delete it yet** — conversation history may be needed for compliance reporting.
2. **Report to security / compliance immediately.** Most data incidents have notification deadlines (72 hours for GDPR).
3. **Identify the scope.** What was sent? When? What models processed it?
4. **Invalidate compromised credentials.** If tokens or keys were in the paste, rotate them now.
5. **After reporting:** delete the conversation history if your compliance team approves.

"I accidentally pasted the `.env`" is a real, common incident. Treat it as one.

---

## Build boundaries into tooling, not just policy

Policies get forgotten. Boundaries in tooling don't.

- **Use pre-commit hooks** that scan staged files for secrets before they get committed.
- **Use a secrets scanner** in CI that catches leaks if they make it to a commit.
- **Configure your CLI agent** to refuse to read `.env` files (some tools support an allowlist / denylist).
- **Configure your IDE** to not index `.env` or secrets directories.
- **Set up gitignore properly** AND confirm files aren't already tracked (see `common-gotchas.md` for the trap).

The team member who forgets the policy is not the problem. The tooling that let them fail is the problem. Fix tooling first.
