# WealthCaseIQ – Agentic Client Onboarding Case Management with UiPath Maestro

## Overview

WealthCaseIQ is an agentic client onboarding case management solution built with UiPath Maestro, UiPath Agents, API Workflows, Coded Apps, Autopilot, and Codex with UiPath Skills.

The solution automates a realistic wealth management onboarding workflow where client documents are received, validated, risk-scored, reviewed by human users when needed, provisioned into a mock Wealth CRM, and closed with an audit-ready summary.

The project demonstrates how AI agents, deterministic workflows, coded human-review apps, and UiPath Maestro can work together in an enterprise case management process.

## Hackathon Track

WealthCaseIQ was built for **Track 1: UiPath Maestro Case**.

The project demonstrates a dynamic, exception-heavy case management process where work moves through multiple stages, including AI agents, deterministic API workflows, coded human-review apps, decision gateways, wait/resume loops, provisioning, and audit closure.

Humans remain in charge at key decision points such as Operations Review, Compliance Review, and Supervisor Escalation.

## Business Problem

Client onboarding in wealth management is document-heavy, exception-heavy, and audit-sensitive.

Operations and Compliance teams often need to manually review:

* Client onboarding forms
* Identity documents
* Tax documents
* Address proof documents
* Duplicate client records
* AML, PEP, sanctions, and adverse media risk indicators
* Human approval and rejection decisions
* Account provisioning status
* Final audit records

Manual handling can cause onboarding delays, inconsistent routing, duplicate account creation, missed risk indicators, and incomplete audit trails.

## Solution

WealthCaseIQ uses UiPath Maestro Case Management to orchestrate an end-to-end onboarding case.

The solution receives onboarding documents, analyzes them with an AI Document Review Agent, performs deterministic duplicate checks and risk scoring, routes exceptions to Operations or Compliance, supports Supervisor Escalation, provisions the account, and creates a final audit closure record.

The AI agents are used only where they add value: document understanding and human-readable summarization. Deterministic API workflows are used for scoring, routing, provisioning, and final closure decisions.

This creates a practical enterprise pattern for regulated onboarding: AI assists, deterministic workflows decide, and humans handle exceptions.

## Key Features

* Email-based onboarding intake
* Attachment upload and file preparation
* AI-powered document classification and extraction
* Required document validation
* Cross-document validation
* Duplicate client detection
* Operations Review for duplicate and document issues
* Deterministic risk scoring
* Mock AML, PEP, sanctions, and adverse media screening logic
* AI-generated risk summary
* Compliance Review for risk and screening exceptions
* Supervisor Escalation for high-risk or policy exception cases
* Deterministic provisioning workflow
* Final audit closure workflow
* AI-generated audit summary
* Human-in-the-loop case routing
* Audit-ready output at case closure

## UiPath Components Used

This project uses the following UiPath components:

* UiPath Maestro / Case Management
* UiPath Studio Web
* UiPath Agents
* UiPath Agent Builder
* UiPath Autopilot
* Codex with UiPath Skills
* UiPath for Coding Agents
* UiPath API Workflows
* UiPath Apps / Coded Apps
* UiPath Integration Service
* Gmail Connector
* UiPath Storage Bucket
* UiPath Orchestrator
* Human-in-the-loop review pattern
* Action Center-style review tasks

## Use of UiPath Autopilot, Codex, and UiPath Skills

WealthCaseIQ was built using a combination of UiPath low-code, agentic, and coded development tools.

This project intentionally uses **UiPath Autopilot** and **Codex with UiPath Skills** as part of the development workflow, not just as documentation tools.

### UiPath Autopilot

UiPath Autopilot was used to help design and build the low-code and agentic parts of the solution, including:

* DocumentReviewAgent for document classification, extraction, and validation
* RiskSummarizationAgent for explaining deterministic risk scoring results
* AuditSummaryAgent for producing a final audit narrative
* API Workflow logic for duplicate checking, risk scoring, provisioning, and audit closure
* Structured JSON input/output contracts
* Decision gateway and routing patterns

Autopilot helped convert business requirements into working UiPath components faster while still preserving deterministic controls for regulated decision points.

### Codex with UiPath Skills

Codex with UiPath Skills was used for the coded and deployment-heavy parts of the solution.

It helped build, rebuild, troubleshoot, and improve:

* Operations Review Coded App
* Compliance Review Coded App
* Supervisor Escalation Coded App
* Human review app schemas
* Null-safe independent inputs
* Dropdown decision logic
* Explicit next-stage routing
* Structured output objects for Maestro
* Email preparation fields for customer/advisor communication
* Audit note and audit event structures
* Case plan binding issues
* Schema mapping errors
* Deployment and publishing troubleshooting

The Operations, Compliance, and Supervisor apps were rebuilt using Codex with UiPath Skills after earlier schema mapping issues. The rebuilt apps use cleaner independent inputs, a single full output object, explicit decision dropdowns, and a separate next-stage dropdown so Maestro can route cases reliably.

### Deployment Troubleshooting with Codex

Deployment was one of the harder parts of the project.

Codex with UiPath Skills was used to troubleshoot:

* UiPath app output mapping issues
* Case plan variable binding issues
* API workflow input/output contracts
* Publishing and deployment errors
* Solution structure and packaging notes

## Agent Type

This solution uses **both low-code AI agents and coded development patterns**.

### Low-code / UiPath AI Agents

* **DocumentReviewAgent**
  Reviews uploaded onboarding documents, classifies document types, extracts fields, validates required documents, and identifies document issues.

* **RiskSummarizationAgent**
  Summarizes deterministic risk scoring output into a business-readable explanation. It does not calculate or change the risk score.

* **AuditSummaryAgent**
  Creates a final human-readable audit narrative from the audit closure result. It does not change the final case status or routing.

### Coded Apps Built with Codex and UiPath Skills

* **Operations Review Coded App**
  Used by Operations Specialists for document issues, duplicate resolution, customer clarification, data corrections, and provisioning failures.

* **Compliance Review Coded App**
  Used by Compliance Analysts for risk review, screening review, additional verification, supervisor escalation, approval, and rejection decisions.

* **Supervisor Escalation Coded App**
  Used by Supervisors for exception approval, return-to-compliance guidance, and rejection confirmation.

### Deterministic API Workflows

* **DuplicateCheckApiWorkflow**
  Performs rule-based duplicate client matching and returns duplicate status, confidence, and recommended action.

* **RiskScoringApiWorkflow**
  Calculates deterministic onboarding risk using document results, duplicate results, and mock AML, PEP, sanctions, and adverse media screening logic.

* **ProvisionAccountApiWorkflow**
  Provisions the client/account into a mock Wealth CRM. It supports creating a new client, adding an account to an existing client, and merging allowed fields.

* **AuditClosureApiWorkflow**
  Determines final case closure status, preserves prior decisions, creates the audit trail, and returns the final audit closure result.

## Architecture Flow

```text
GMAIL_TRIGGER
↓
INTAKE
   ├── DocumentTakerWorkflow → FileArray
   └── DocumentAnalysisAgent → caseDocumentValidationResult
↓
DOCUMENT_DECISION_GATEWAY
   ├── Document Missing / Document Issue → CLIENT_DOCUMENT_RESUBMISSION
   ├── Compliance issue from intake → COMPLIANCE_REVIEW
   └── Doc Validation Pass → DUPLICATE_CHECK

CLIENT_DOCUMENT_RESUBMISSION
   ├── SentEmailNotification / Request Document Resubmission
   └── Wait for Client Response
        ↓
        Resume case when client responds / new documents arrive
        ↓
        INTAKE

DUPLICATE_CHECK
   └── DuplicateValidator API
↓
DUPLICATE_DECISION_GATEWAY
   ├── No Duplicate → RISK_REVIEW
   └── Possible / Strong Duplicate → OPERATIONS_REVIEW

OPERATIONS_REVIEW
   └── Action Center-style task: Operations Review Coded App
↓
OPERATIONS_DECISION_GATEWAY
   ├── REQUEST_CORRECT_DOCUMENT → CLIENT_DOCUMENT_RESUBMISSION
   ├── REQUEST_CLARIFICATION → REQUEST_CLARIFICATION
   ├── ADD_ACCOUNT / MERGE / CREATE_NEW_CLIENT → RISK_REVIEW
   ├── RETRY_PROVISIONING → APPROVE_ONBOARDING
   ├── SEND_BACK_TO_COMPLIANCE / ESCALATE → COMPLIANCE_REVIEW
   └── REJECT_CASE → REJECT_CASE

REQUEST_CLARIFICATION
   ├── SentEmailNotification / Request Clarification
   └── Wait for Clarification
        ↓
        Resume case when client responds
        ↓
        INTAKE or configured next stage

RISK_REVIEW
   ├── RiskScoringApiWorkflow
   └── RiskSummarizationAgent
↓
RISK_DECISION_GATEWAY
   ├── Low / Clear / Acceptable Risk → APPROVE_ONBOARDING
   └── Medium / High / Screening Issue → COMPLIANCE_REVIEW

COMPLIANCE_REVIEW
   └── Action Center-style task: Compliance Review Coded App
↓
COMPLIANCE_DECISION_GATEWAY
   ├── APPROVED_FOR_PROVISIONING → APPROVE_ONBOARDING
   ├── REQUEST_ADDITIONAL_VERIFICATION → REQUEST_ADDITIONAL_VERIFICATION
   ├── SEND_BACK_TO_OPERATIONS → OPERATIONS_REVIEW
   ├── SUPERVISOR_ESCALATION → SUPERVISOR_ESCALATION
   └── REJECT_CASE / CONFIRM_REJECTION → REJECT_CASE

REQUEST_ADDITIONAL_VERIFICATION
   ├── SentEmailNotification / Request Additional Verification
   └── Wait for Additional Verification
        ↓
        Resume case when additional documents arrive
        ↓
        INTAKE

SUPERVISOR_ESCALATION
   └── Action Center-style task: Supervisor Escalation Coded App
↓
SUPERVISOR_DECISION_GATEWAY
   ├── APPROVE_EXCEPTION → COMPLIANCE_REVIEW
   ├── RETURN_TO_COMPLIANCE → COMPLIANCE_REVIEW
   └── CONFIRM_REJECTION → REJECT_CASE

APPROVE_ONBOARDING
   └── ProvisionAccountApiWorkflow / Account Provisioning
↓
AUDIT_CLOSURE_APPROVED
   ├── AuditClosureApiWorkflow
   └── AuditSummaryAgent
↓
COMPLETION

REJECT_CASE
   ├── AuditClosureApiWorkflow
   └── AuditSummaryAgent
↓
CLOSED_REJECTED / COMPLETION
```

## What Happens in Each Major Stage

| Stage                        | What Happens                                                                                                                                                                                      | Output / Routing                                                                                             |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| GMAIL_TRIGGER                | Receives onboarding email and attachments.                                                                                                                                                        | Starts the case.                                                                                             |
| INTAKE                       | Prepares attachments and sends them to document analysis.                                                                                                                                         | Produces `FileArray` and `caseDocumentValidationResult`.                                                     |
| DOCUMENT_REVIEW              | AI Document Review Agent classifies onboarding, identity, tax, and address proof documents. It extracts client fields and validates cross-document consistency.                                   | Passes to Duplicate Check or routes document issues to resubmission / Operations / Compliance.               |
| CLIENT_DOCUMENT_RESUBMISSION | Sends a document request and waits for the client/advisor to provide corrected or missing documents.                                                                                              | Resumes back to Intake.                                                                                      |
| DUPLICATE_CHECK              | Deterministic duplicate scoring using name, DOB, tax ID last4, address, email, and phone.                                                                                                         | No duplicate goes to Risk Review; possible/strong duplicate goes to Operations Review.                       |
| OPERATIONS_REVIEW            | Human review for operational issues such as document correction, duplicate resolution, add-account-to-existing-client, merge-with-existing-client, customer clarification, or provisioning retry. | Routes to document request, clarification, risk review, compliance review, provisioning retry, or rejection. |
| RISK_REVIEW                  | RiskScoringApiWorkflow performs deterministic mock screening and risk scoring. RiskSummarizationAgent explains the result.                                                                        | Low risk goes to approval/provisioning. Medium/high/screening risk goes to Compliance Review.                |
| COMPLIANCE_REVIEW            | Human compliance review for AML, PEP, sanctions, adverse media, fraud, identity mismatch, tax mismatch, high risk, or policy-sensitive cases.                                                     | Routes to approval, additional verification, Operations, Supervisor Escalation, or rejection.                |
| SUPERVISOR_ESCALATION        | Supervisor reviews exception approval, return-to-compliance guidance, or rejection confirmation.                                                                                                  | Routes to Compliance Review or Rejected.                                                                     |
| APPROVE_ONBOARDING           | Approved case moves to provisioning.                                                                                                                                                              | Starts ProvisionAccountApiWorkflow.                                                                          |
| PROVISIONING                 | Creates a new client/account, adds an account to an existing client, or merges allowed non-sensitive fields in mock Wealth CRM logic.                                                             | Success goes to Audit Closure. Failure routes back to Operations.                                            |
| AUDIT_CLOSURE                | Deterministic workflow creates final closure status, captures key automated and human decisions, and generates audit trail.                                                                       | Routes to Completed, Closed Rejected, or Operations if closure is blocked.                                   |
| AUDIT_SUMMARY                | AI agent creates a human-readable audit narrative from the deterministic audit closure result.                                                                                                    | Does not change routing.                                                                                     |
| COMPLETED                    | Terminal successful state.                                                                                                                                                                        | No further action required.                                                                                  |
| CLOSED_REJECTED              | Terminal rejected state.                                                                                                                                                                          | No account provisioned; rejection audit retained.                                                            |

## Risk Scoring Details

The risk stage is deterministic and intentionally separated from AI-generated summaries.

`RiskScoringApiWorkflow` evaluates:

* Document validation result
* Duplicate status
* Operations Review outcome
* Mock sanctions screening
* Mock AML screening
* Mock PEP screening
* Mock adverse media screening
* Fraud indicators
* Identity mismatch
* Tax ID mismatch
* Address mismatch
* Duplicate resolution risk
* Customer/advisor email overlap
* High-risk onboarding attributes

The workflow assigns points and produces a structured risk result, including:

* `riskScore`
* `riskLevel`
* `recommendation`
* `reasonCode`
* `screeningResults`
* `riskFactors`
* `nextSuggestedStage`

Example routing:

```text
LOW / CLEAR → APPROVED_FOR_PROVISIONING
MEDIUM / HIGH / SCREENING ISSUE → COMPLIANCE_REVIEW
FAILED / MALFORMED INPUT → OPERATIONS_REVIEW
```

Important: The AML, PEP, sanctions, and adverse media checks in this demo use mock logic and synthetic test profiles. In a production version, these would be replaced by approved enterprise KYC/AML screening APIs.

## Why This Design Is Enterprise-Ready

WealthCaseIQ separates AI reasoning from deterministic business decisions.

AI is used for:

* Document understanding
* Risk explanation
* Audit narrative generation

Deterministic workflows are used for:

* Duplicate scoring
* Risk scoring
* Provisioning
* Case closure
* Routing decisions

Human review apps are used for:

* Operational exceptions
* Duplicate resolution
* Compliance decisions
* Supervisor approval or rejection confirmation

This is important for financial services because onboarding decisions must be explainable, auditable, and repeatable.

## Enterprise Value

WealthCaseIQ demonstrates a reusable enterprise pattern for regulated, exception-heavy onboarding processes.

| Area | Value |
|---|---|
| Business impact | Reduces manual handoffs across Operations, Compliance, Supervisors, and provisioning teams. |
| Auditability | Preserves document results, duplicate decisions, risk scoring, human review decisions, provisioning output, and final closure history. |
| Explainability | Uses deterministic workflows for duplicate scoring, risk scoring, routing, provisioning, and closure decisions. |
| Human control | Keeps Operations, Compliance, and Supervisors in control of exception decisions. |
| AI where useful | Uses AI for document understanding, risk explanation, and audit narratives, not final regulated decisions. |
| Reusability | The same pattern can apply to account opening, KYC refresh, loan onboarding, insurance intake, and other document-heavy case processes. |
| Developer productivity | Uses UiPath Autopilot and Codex with UiPath Skills to accelerate agents, workflows, coded apps, troubleshooting, and deployment. |


## Main Case Stages

| Stage                    | Purpose                                               |
| ------------------------ | ----------------------------------------------------- |
| EMAIL_INTAKE             | Starts the onboarding case from an email request      |
| ATTACHMENT_UPLOAD        | Resolves and prepares attached files                  |
| DOCUMENT_REVIEW          | Uses AI to classify and validate onboarding documents |
| DUPLICATE_CHECK          | Checks for possible existing client profiles          |
| OPERATIONS_REVIEW        | Human review for operational exceptions               |
| RISK_SCORING             | Deterministic risk calculation and mock screening     |
| RISK_SUMMARY             | AI explanation of deterministic risk results          |
| COMPLIANCE_REVIEW        | Human compliance decision stage                       |
| SUPERVISOR_ESCALATION    | Higher-level review for exceptions                    |
| PROVISIONING_IN_PROGRESS | Creates or updates client/account records             |
| AUDIT_CLOSURE            | Creates final audit closure result                    |
| AUDIT_SUMMARY            | Creates final human-readable audit summary            |
| COMPLETED                | Terminal successful closure                           |
| CLOSED_REJECTED          | Terminal rejected closure                             |

## Repository Structure

```text
wealthcaseiq-uipath-agenthack/
├── README.md
├── LICENSE
├── docs/
│   ├── architecture-overview.md
│   ├── setup-guide.md
│   ├── demo-script.md
│   └── screenshots/
├── sample-inputs/
│   ├── document-review-sample.json
│   ├── duplicate-check-sample.json
│   ├── risk-scoring-sample.json
│   ├── provisioning-sample.json
│   ├── audit-closure-sample.json
│   └── audit-summary-sample.json
├── sample-documents/
│   └── README.md
├── case-plan/
│   └── README.md
├── workflows/
│   ├── DuplicateCheckApiWorkflow/
│   ├── RiskScoringApiWorkflow/
│   ├── ProvisionAccountApiWorkflow/
│   └── AuditClosureApiWorkflow/
├── agents/
│   ├── DocumentReviewAgent/
│   ├── RiskSummarizationAgent/
│   └── AuditSummaryAgent/
├── apps/
│   ├── OperationsReviewApp/
│   ├── ComplianceReviewApp/
│   └── SupervisorEscalationApp/
└── deployment/
    └── uipath-deployment-notes.md
```

## Submission Assets

* Public GitHub repository
* MIT License
* README with setup instructions
* Demo video under 5 minutes
* Presentation deck
* Sample documents
* Sample input/output JSON
* UiPath component list
* Agent type explanation
* Deployment notes

## Setup Instructions

### Prerequisites

To run this project, you need:

* UiPath Automation Cloud access
* UiPath Studio Web access
* UiPath Maestro / Case Management enabled
* UiPath Agents enabled
* UiPath Apps / Coded Apps enabled
* UiPath Integration Service enabled
* Gmail Connector configured
* UiPath Orchestrator folder access
* Permissions to publish and run API Workflows

### 1. Clone the Repository

```bash
git clone https://github.com/<your-github-username>/wealthcaseiq-uipath-agenthack.git
cd wealthcaseiq-uipath-agenthack
```

### 2. Create UiPath Components

Create or import the following components in UiPath Studio Web / Automation Cloud.

#### Agents

* DocumentReviewAgent
* RiskSummarizationAgent
* AuditSummaryAgent

#### API Workflows

* DuplicateCheckApiWorkflow
* RiskScoringApiWorkflow
* ProvisionAccountApiWorkflow
* AuditClosureApiWorkflow

#### Apps / Coded Apps

* OperationsReviewApp
* ComplianceReviewApp
* SupervisorEscalationApp

#### Maestro Case Plan

Create the case plan using the architecture described in this README.

### 3. Configure Storage Bucket

Create a UiPath Storage Bucket for onboarding documents.

Recommended folder pattern:

```text
/onboarding/{caseId}/documents/
```

The uploaded email attachments should be stored or resolved into a file array that can be passed into DocumentReviewAgent.

### 4. Configure Gmail Trigger

Configure a Gmail trigger for onboarding emails.

Recommended trigger condition:

```text
Subject contains "Onboarding Request"
Has attachments = true
```

Sample subject:

```text
Onboarding Request - Michael Thompson
```

### 5. Configure Case Variables

Recommended case variables:

```text
caseDocumentValidationResult
duplicateCheckResult
operationsReviewResult
riskScoringResult
riskSummaryResult
complianceReviewResult
supervisorEscalationResult
provisioningResult
auditClosureResult
auditSummaryResult
```

### 6. Configure Human Review App Outputs

Each review app should return one complete output object.

Recommended output objects:

```text
operationsReviewResult
complianceReviewResult
supervisorEscalationResult
```

Important mapping rule:

```text
Maestro routing should use each stage's deterministic nextStage value.
```

Examples:

```text
operationsReviewResult.nextStage
complianceReviewResult.nextStage
supervisorEscalationResult.nextStage
auditClosureResult.nextStage
```

### 7. Run the Happy Path

Send an onboarding email with the required documents.

Expected happy path:

```text
EMAIL_INTAKE
→ ATTACHMENT_UPLOAD
→ DOCUMENT_REVIEW
→ DUPLICATE_CHECK
→ RISK_SCORING
→ RISK_SUMMARY
→ APPROVED_FOR_PROVISIONING
→ PROVISIONING_IN_PROGRESS
→ AUDIT_CLOSURE
→ AUDIT_SUMMARY
→ COMPLETED
```

### 8. Run Exception Scenarios

Recommended test scenarios:

1. Missing tax document
2. Expired identity document
3. Address mismatch
4. Strong duplicate client match
5. High-risk case requiring Compliance Review
6. Mock AML / PEP / sanctions / adverse media case requiring Compliance Review
7. Supervisor escalation
8. Provisioning failure routed back to Operations Review
9. Rejected onboarding case
10. Successful audit closure

### 9. Deployment Notes

Deployment and schema binding are important for this solution.

Recommended validation before running:

1. Confirm all API Workflow inputs and outputs are published.
2. Confirm all Coded Apps are published.
3. Confirm each human review app returns one complete output object.
4. Confirm Maestro variables are mapped to the correct app outputs:

   * `operationsReviewResult`
   * `complianceReviewResult`
   * `supervisorEscalationResult`
5. Confirm `AuditClosureApiWorkflow` receives the latest upstream results.
6. Confirm `AuditSummaryAgent` runs after `AuditClosureApiWorkflow`.
7. Confirm final routing uses `auditClosureResult.nextStage`.

Important:

Do not route based on AI summary output. Final routing should use deterministic outputs such as:

* `duplicateCheckResult.nextSuggestedStage`
* `riskScoringResult.nextSuggestedStage`
* human review `nextStage`
* `auditClosureResult.nextStage`

## Demo Video

Demo video link:

```text
<PASTE_PUBLIC_YOUTUBE_OR_VIMEO_LINK_HERE>
```

The video is under five minutes and demonstrates the solution functioning in UiPath.

## Demo Video Suggested Flow

Recommended video structure:

1. Show the onboarding email and attachments
2. Show the Maestro case created
3. Show Document Review result
4. Show Duplicate Check result
5. Show Operations Review or Compliance Review
6. Show Risk Scoring and mock screening result
7. Show Provisioning result
8. Show Audit Closure result
9. Show final Completed or Closed Rejected stage

## Built With

* UiPath Maestro
* UiPath Studio Web
* UiPath Agents
* UiPath Agent Builder
* UiPath Autopilot
* Codex with UiPath Skills
* UiPath for Coding Agents
* UiPath API Workflows
* UiPath Apps / Coded Apps
* UiPath Integration Service
* Gmail Connector
* UiPath Storage Bucket
* UiPath Orchestrator
* Human-in-the-loop review pattern
* Action Center-style review tasks
* JSON
* JavaScript expressions
* PDF sample documents
* Mock Wealth CRM logic
* Mock AML / PEP / Sanctions / Adverse Media screening logic
* GitHub
* YouTube

## License

This repository is licensed under the MIT License.

The open-source license applies only to the original solution code and supporting artifacts in this repository. UiPath proprietary tools, activities, SDK packages, platform components, and services remain subject to their own UiPath license terms.

## Author

Ravindra Reddy

## Hackathon Submission

* Project Name: WealthCaseIQ
* Repository: [https://github.com/<your-github-username>/wealthcaseiq-uipath-agenthack](https://github.com/rmadireddy33/wealthcaseiq-uipath-agenthack)
* Demo Video: [<PASTE_PUBLIC_YOUTUBE_OR_VIMEO_LINK_HERE>](https://www.youtube.com/watch?v=-0nFi7vSudk)
