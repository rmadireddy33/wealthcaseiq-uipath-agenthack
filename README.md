# wealthcaseiq-uipath-agenthack
Agentic wealth management client onboarding case management solution built with UiPath Maestro, Agents, API Workflows, and Coded Apps.

# WealthCaseIQ – Agentic Client Onboarding Case Management with UiPath Maestro

## Overview

WealthCaseIQ is an agentic client onboarding case management solution built with UiPath Maestro, UiPath Agents, API Workflows, and UiPath Apps / Coded Apps.

The solution automates a realistic wealth management onboarding workflow where client documents are received, validated, risk-scored, reviewed by human users when needed, provisioned into a mock Wealth CRM, and closed with an audit-ready summary.

The project demonstrates how AI agents and deterministic automation can work together in an enterprise case management process.

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

## Key Features

* Email-based onboarding intake
* Attachment upload and file preparation
* AI-powered document classification and extraction
* Required document validation
* Cross-document validation
* Duplicate client detection
* Operations Review for duplicate and document issues
* Deterministic risk scoring
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
* UiPath API Workflows
* UiPath Apps / Coded Apps
* UiPath Integration Service
* Gmail Connector
* UiPath Storage Bucket
* UiPath Orchestrator
* UiPath Action Center / Human-in-the-loop pattern
* UiPath Autopilot / UiPath Skills / Codex-assisted development

## Agent Type

This solution uses both AI agents and deterministic workflows.

### AI Agents

* **DocumentReviewAgent**
  Reviews uploaded onboarding documents, classifies document types, extracts fields, validates required documents, and identifies document issues.

* **RiskSummarizationAgent**
  Summarizes deterministic risk scoring output into a business-readable explanation. It does not calculate or change the risk score.

* **AuditSummaryAgent**
  Creates a final human-readable audit narrative from the audit closure result. It does not change the final case status or routing.

### Deterministic API Workflows

* **DuplicateCheckApiWorkflow**
  Performs rule-based duplicate client matching and returns duplicate status, confidence, and recommended action.

* **RiskScoringApiWorkflow**
  Calculates deterministic onboarding risk using document, duplicate, and mock screening results.

* **ProvisionAccountApiWorkflow**
  Provisions the client/account into a mock Wealth CRM. It supports creating a new client, adding an account to an existing client, and merging allowed fields.

* **AuditClosureApiWorkflow**
  Determines final case closure status, preserves prior decisions, creates the audit trail, and returns the final audit closure result.

### Human Review Apps

* **OperationsReviewApp**
  Used by Operations Specialists for document issues, duplicate resolution, data corrections, and provisioning failures.

* **ComplianceReviewApp**
  Used by Compliance Analysts for risk review, screening review, fraud indicators, and final compliance decisions.

* **SupervisorEscalationApp**
  Used by Supervisors for high-risk exceptions, rejection confirmation, policy exception review, and additional guidance.

## Architecture Flow

```text
EMAIL_INTAKE
↓
ATTACHMENT_UPLOAD
↓
DOCUMENT_REVIEW
↓
DOCUMENT_DECISION_GATEWAY
   ├── Document issue → OPERATIONS_REVIEW or REQUEST_DOCUMENTS
   └── Passed → DUPLICATE_CHECK
↓
DUPLICATE_CHECK
↓
DUPLICATE_DECISION_GATEWAY
   ├── NO_DUPLICATE → RISK_SCORING
   └── POSSIBLE/STRONG_DUPLICATE → OPERATIONS_REVIEW
↓
OPERATIONS_REVIEW if needed
↓
RISK_SCORING
↓
RISK_SUMMARY
↓
RISK_DECISION_GATEWAY
   ├── LOW/CLEAR → APPROVED_FOR_PROVISIONING
   └── MEDIUM/HIGH/SCREENING ISSUE → COMPLIANCE_REVIEW
↓
COMPLIANCE_REVIEW if needed
   ├── APPROVE_ONBOARDING → APPROVED_FOR_PROVISIONING
   ├── REQUEST_ADDITIONAL_VERIFICATION → WAITING_FOR_ADDITIONAL_VERIFICATION
   ├── SEND_BACK_TO_OPERATIONS → OPERATIONS_REVIEW
   ├── ESCALATE_TO_SUPERVISOR → SUPERVISOR_ESCALATION
   ├── REJECT_ONBOARDING → REJECTED
   └── PLACE_ON_HOLD → ON_HOLD
↓
APPROVED_FOR_PROVISIONING
↓
PROVISIONING_IN_PROGRESS
↓
AUDIT_CLOSURE
↓
AUDIT_SUMMARY
↓
COMPLETED or CLOSED_REJECTED
```

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

This is important for financial services because onboarding decisions must be explainable, auditable, and repeatable.

## Main Case Stages

| Stage                    | Purpose                                               |
| ------------------------ | ----------------------------------------------------- |
| EMAIL_INTAKE             | Starts the onboarding case from an email request      |
| ATTACHMENT_UPLOAD        | Resolves and prepares attached files                  |
| DOCUMENT_REVIEW          | Uses AI to classify and validate onboarding documents |
| DUPLICATE_CHECK          | Checks for possible existing client profiles          |
| OPERATIONS_REVIEW        | Human review for operational exceptions               |
| RISK_SCORING             | Deterministic risk calculation                        |
| RISK_SUMMARY             | AI explanation of deterministic risk results          |
| COMPLIANCE_REVIEW        | Human compliance decision stage                       |
| SUPERVISOR_ESCALATION    | Higher-level review for exceptions                    |
| PROVISIONING_IN_PROGRESS | Creates or updates client/account records             |
| AUDIT_CLOSURE            | Creates final audit closure result                    |
| AUDIT_SUMMARY            | Creates final human-readable audit summary            |
| COMPLETED                | Terminal successful closure                           |
| CLOSED_REJECTED          | Terminal rejected closure                             |

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
* UiPath Storage Bucket access
* Permissions to publish and run API Workflows

### 1. Clone the Repository

```bash
git clone https://github.com/<your-github-username>/wealthcaseiq-uipath-agenthack.git
cd wealthcaseiq-uipath-agenthack
```

### 2. Create UiPath Components

Create or import the following components in UiPath Studio Web / Automation Cloud:

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

### 6. Run the Happy Path

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

### 7. Run Exception Scenarios

Recommended test scenarios:

1. Missing tax document
2. Expired identity document
3. Address mismatch
4. Strong duplicate client match
5. Medium-risk case requiring Compliance Review
6. Supervisor escalation
7. Provisioning failure routed back to Operations Review
8. Rejected onboarding case
9. Successful audit closure

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
6. Show Provisioning result
7. Show Audit Closure result
8. Show final Completed or Closed Rejected stage

## License

This repository is licensed under the MIT License.

The open-source license applies only to the original solution code and supporting artifacts in this repository. UiPath proprietary tools, activities, SDK packages, platform components, and services remain subject to their own UiPath license terms.

## Author

Ravindra Reddy

## Hackathon Submission

* Project Name: WealthCaseIQ
* Repository: https://github.com/<your-github-username>/wealthcaseiq-uipath-agenthack
* Demo Video: <PASTE_PUBLIC_YOUTUBE_OR_VIMEO_LINK_HERE>
