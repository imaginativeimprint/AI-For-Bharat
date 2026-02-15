# System Design: ClaimFlow AI

## 1. High-Level Architecture
We are building a "Serverless Pipeline." Think of it as a factory belt. The document enters at one end, passes through several AI robots (OCR, NLP, Logic), and comes out as a "Clean Claim" at the other end. We use AWS to ensure we don't need to manage heavy servers.

## 2. Component Design

### 2.1. Frontend (The Dashboard)
* **Tech:** React.js hosted on AWS Amplify.
* **Role:** A clean drag-and-drop interface for billing clerks. It shows the PDF on the left and the AI's "checklist" on the right.

### 2.2. The Brain (Backend & AI)
* **Orchestrator:** AWS Step Functions. It manages the flow of data so steps happen in order.
* **The "Eyes" (OCR):** **Amazon Textract**. It pulls raw text from scanned image PDFs (bills, reports).
* **The "Brain" (Medical NLP):** **Amazon Comprehend Medical**. This is our secret weapon. It understands medical context (e.g., it knows "500mg Paracetamol" is a *medication* and "Dengue" is a *diagnosis*).
* **The "Auditor" (GenAI):** **Amazon Bedrock (Claude 3)**. We use this to reason against policy rules.
    * *Prompt logic:* "Given this patient has Policy Type B and Diagnosis X, is a 5-day ICU stay covered? If not, explain why."

### 2.3. Data Storage
* **Secure Vault:** **Amazon S3** (Encrypted). Stores the raw medical documents.
* **Database:** **Amazon DynamoDB**. Stores the audit results, claim status, and hospital analytics.

## 3. Data Flow
1.  **Upload:** User drags patient file to Dashboard -> Uploads to S3.
2.  **Trigger:** S3 upload wakes up the AWS Lambda function.
3.  **Extraction:** Lambda sends file to **Textract** to get text.
4.  **Understanding:** Text is sent to **Comprehend Medical** to extract ICD-10 codes, Medications, and Dosages.
5.  **Validation:** **Bedrock** compares extracted data vs. Insurance Policy Rules.
6.  **Result:** A JSON report (Approved/Rejected with reasons) is sent to DynamoDB and pushed to the Frontend via WebSocket.

## 4. Database Schema (DynamoDB)

**Table: Claims**
* `PK`: HospitalID
* `SK`: ClaimID
* `Attributes`:
    * PatientName (Masked)
    * TotalAmount
    * RiskScore (0-100)
    * MissingDocuments (List)
    * Status (Processing/Ready/Flagged)

## 5. API Endpoints
* `POST /upload-claim`: Secure upload endpoint.
* `GET /audit-status/{claimId}`: Poll for the AI results.
* `GET /analytics`: Dashboard stats (e.g., "You saved ₹5 Lakhs this week").