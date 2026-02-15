# Requirements: ClaimFlow AI (Smart Health-Fintech Auditor)

## 1. Project Overview
ClaimFlow AI is an intelligent "pre-audit" system for hospitals. It acts as a safety net between the hospital's billing department and the insurance company. By using Generative AI and Medical NLP, it reads patient files in real-time to catch errors, missing documents, or mismatched codes *before* the claim is sent. This prevents rejections, speeds up patient discharge, and saves hospitals millions in lost revenue.

## 2. The Problem
* **The "Discharge Waiting Room" Nightmare:** Patients in India often wait 6–10 hours *after* being told they can go home, just waiting for insurance approval.
* **Revenue Leakage:** Hospitals lose 15-20% of potential revenue because of simple clerical errors or missed billing codes that insurance companies reject.
* **Manual Overload:** Doctors and nurses spend hours on paperwork instead of patient care.

## 3. User Personas
* **The Billing Executive:** Overworked and stressed. Needs a tool that says, "Hey, you missed the MRI report for this claim," before they hit send.
* **The Hospital Administrator:** Wants to reduce the "Claim Rejection Rate" and improve cash flow.
* **The Patient Family:** Anxious to take their loved one home; wants the insurance approval process to be instant.

## 4. Functional Requirements
### 4.1. Core Features
* **Smart Document Scanning:** Upload messy scanned PDFs, handwritten notes, or digital EMR records.
* **Instant "Pre-Auth" Check:** The AI compares the medical diagnosis (e.g., "Dengue") with the insurance policy rules (e.g., "Requires min. 24hr hospitalization") and flags issues instantly.
* **ICD-10 Code Validator:** Automatically checks if the treatment codes match the diagnosis codes (a common reason for rejection).
* **Missing Info Alerts:** explicitly tells the user: "Cannot submit. Missing 'Discharge Summary' document."

### 4.2. User Interface
* **Traffic Light System:**
    * 🟢 **Green:** Ready to submit (99% approval chance).
    * 🟡 **Yellow:** Warnings (e.g., "Check patient age").
    * 🔴 **Red:** Critical Error (e.g., "Missing signature").

## 5. Non-Functional Requirements
* **Privacy First:** strict adherence to DISHA (Digital Information Security in Healthcare Act) and HIPAA standards. Patient PII is masked.
* **Speed:** Must analyze a 50-page medical file in under 30 seconds.
* **Integration:** Must be able to export data to standard hospital ERP formats (JSON/CSV).

## 6. Constraints
* **Cloud Only:** The solution relies on AWS cloud processing power.
* **Language:** Currently optimized for English medical records (standard in Indian hospitals).