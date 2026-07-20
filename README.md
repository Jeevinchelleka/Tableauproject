# Tableau Project

This repository was initialized for the Tableau project.


For a POC (Proof of Concept), don't build the full Zerodha-grade onboarding model. Focus on the minimum set of tables that can demonstrate:
1.	Customer registration
2.	KYC journey tracking
3.	Approval workflow
4.	Onboarding analytics/dashboard
Recommended POC Architecture
1. Customer Master
Stores the basic customer record.
1     Customer
2     ---------
3     customer_id (PK)
4     mobile_number
5     email
6     full_name
7     created_date
8     status
Status Values
1     Lead
2     In Progress
3     Pending Review
4     Approved
5     Rejected
6     Activated

2. Onboarding Application
A customer may start onboarding multiple times.
1     Onboarding_Application
2     ---------------------
3     application_id (PK)
4     customer_id (FK)
5     application_start_time
6     application_end_time
7     application_status
8     source_channel
Example channels:
1     Web
2     Android
3     iOS
4     Referral

3. Onboarding Journey Steps
Master list of onboarding stages.
1     Onboarding_Step_Master
2     ----------------------
3     step_id
4     step_name
5     step_sequence
Sample Data:
1     1 Mobile Verification
2     2 Email Verification
3     3 PAN Verification
4     4 Aadhaar Verification
5     5 Bank Verification
6     6 Document Upload
7     7 E-Sign
8     8 Compliance Review
9     9 Account Creation

4. Customer Step Tracker
Most important table for analytics.
1     Onboarding_Step_Status
2     ----------------------
3     id
4     application_id
5     step_id
6     status
7     start_time
8     end_time
9     failure_reason
Status:
1     Started
2     Completed
3     Failed
4     Skipped
This table will power almost all onboarding dashboards.

5. KYC Details
Keep it simple for POC.
1     Customer_KYC
2     ------------
3     customer_id
4     pan_number
5     aadhaar_reference
6     kyc_status
7     verification_date
Status:
1     Pending
2     Verified
3     Rejected

6. Bank Details
1     Customer_Bank
2     -------------
3     bank_id
4     customer_id
5     account_number_masked
6     ifsc
7     bank_name
8     verification_status

7. Documents
1     Customer_Document
2     -----------------
3     document_id
4     customer_id
5     document_type
6     upload_status
7     verification_status
8     uploaded_time
Examples:
1     PAN
2     Aadhaar
3     Cancelled Cheque
4     Photo

8. Approval Review
Operations team review.
1     Compliance_Review
2     -----------------
3     review_id
4     customer_id
5     decision
6     review_comment
7     review_date
8     reviewed_by
Decision:
1     Approved
2     Rejected
3     Need More Information

9. Event Tracking Table
Very useful for business dashboards.
1     Customer_Event
2     --------------
3     event_id
4     customer_id
5     event_name
6     event_time
Example events:
1     OTP Verified
2     PAN Verified
3     KYC Completed
4     E-Sign Completed
5     Account Approved
6     Account Activated

Minimum Dashboard KPIs for POC
1. Onboarding Funnel
The most important visualization.
1     1000 Leads
2      ↓
3     900 Mobile Verified
4      ↓
5     800 PAN Verified
6      ↓
7     700 Document Uploaded
8      ↓
9     650 E-Sign Completed
10      ↓
11     600 Approved
12      ↓
13     550 Activated
Visualization: Funnel Chart

2. Step Wise Drop-off %
Shows where users abandon onboarding.
Example:
1     Mobile → PAN     11% Drop
2     PAN → Documents  12% Drop
3     Documents → ESign 7% Drop
Visualization: Funnel + Waterfall

3. Onboarding Completion Rate
Formula:
1     Activated Customers
2     /
3     Customers Started Onboarding
Example:
1     550 / 1000 = 55%

4. KYC Success Rate
Formula:
1     Verified KYC
2     /
3     Total KYC Submitted
Example:
1     900 / 1000 = 90%

5. Approval vs Rejection
1     Approved = 600
2     Rejected = 70
3     Pending = 30
Visualization: Donut Chart

6. Average Onboarding Time
Formula:
1     Activation Time
2     -
3     Journey Start Time
Example:
1     Average = 18 mins
2     Median = 12 mins
This is a key business KPI.

7. Pending Cases Dashboard
Operations view.
1     Pending KYC = 45
2     Pending Documents = 20
3     Pending Review = 15
Visualization: Cards

8. Source Channel Performance
1     Web
2     Android
3     iOS
4     Referral
Metrics:
•	Applications Started
•	Completion Rate
•	Approval Rate
Example:
1     Web      65%
2     Android  56%
3     iOS      70%

POC Dashboard Layout
Executive Summary
•	Total Leads
•	Total Applications
•	Approved Accounts
•	Activated Accounts
•	Completion Rate %
Funnel Section
•	Lead → Activation Funnel
•	Step-wise Conversion %
Operations Section
•	Pending Applications
•	Rejected Applications
•	KYC Success Rate
•	Average TAT
Channel Analytics
•	Web vs Android vs iOS
•	Conversion by Channel

If I were building a Zerodha-like onboarding POC
I would create only 9 tables:
1     1. Customer
2     2. Onboarding_Application
3     3. Onboarding_Step_Master
4     4. Onboarding_Step_Status
5     5. Customer_KYC
6     6. Customer_Bank
7     7. Customer_Document
8     8. Compliance_Review
9     9. Customer_Event
and show only 5 business KPIs:
1.	Onboarding Funnel
2.	Completion Rate
3.	Drop-off Analysis
4.	KYC Success Rate
5.	Average Onboarding TAT
These are enough to convincingly demonstrate the customer onboarding journey in a brokerage platform POC.


For a customer onboarding POC for a Zerodha-like platform, keep the data model lean and focused on tracking the onboarding journey end-to-end.
Tables
Core Customer Tables
Customer
Customer master record
Mobile, email, name, status
Onboarding_Application
One onboarding application per onboarding attempt
Application status, start date, completion date, source channel
Onboarding_Step_Master
Master list of onboarding steps
Mobile Verification, PAN Verification, Bank Verification, eSign, etc.
Onboarding_Step_Status
Tracks progress of each customer through each step
Status, timestamps, failure reasons
Verification & Compliance Tables
Customer_KYC
PAN/Aadhaar details
KYC verification status
Customer_Bank
Bank account details
Verification status
Customer_Document
Uploaded documents
Verification status
Compliance_Review
Manual review and approval/rejection details
Analytics & Tracking Tables
Customer_Event
Event-level tracking
OTP Verified, KYC Completed, eSign Completed, Account Activated, etc.
KPIs
Funnel KPIs
Total Leads
Onboarding Started
Mobile Verified
KYC Completed
Documents Uploaded
eSign Completed
Approved Accounts
Activated Accounts
Conversion KPIs
Onboarding Completion Rate
Activated Accounts ÷ Applications Started
KYC Success Rate
KYC Verified ÷ KYC Submitted
Approval Rate
Approved Accounts ÷ Applications Submitted
Activation Rate
Activated Accounts ÷ Approved Accounts
Drop-off KPIs
Step-wise Drop-off %
Mobile → KYC
KYC → Documents
Documents → eSign
eSign → Approval
Highest Drop-off Step
Operational KPIs
Average Onboarding TAT
Start to Activation
Median Onboarding TAT
Pending Applications
Pending KYC Cases
Pending Compliance Reviews
Rejected Applications
Channel KPIs
Applications by Channel
Web / Android / iOS / Referral
Completion Rate by Channel
Approval Rate by Channel
Activation Rate by Channel
For a Business Dashboard (POC)
The top 8 KPIs I'd prioritize are:
Total Leads
Applications Started
Onboarding Completion Rate
KYC Success Rate
Approval Rate
Activation Rate
Average Onboarding TAT
Step-wise Drop-off Analysis
These are sufficient to demonstrate the entire onboarding journey and identify conversion bottlenecks.
 
for KPIs we can pick 4-5 to visualize based on synthetic generated
 

 
but broadly we would need below tables in onboarding journey
Customer table - To keep customer information based on registration
Document Table - to keep customer document information
Address table - to keep address details - to identify region , state etc
Customer bank details
Customer verification status - registered, KYC pending, bank details pending etc
Compliance(from broker side) - this for backend verification
Activation table
 
Problem Statement:
Design and implement a robust customer onboarding module for a stockbroking platform similar to Zerodha, covering the complete journey from user registration through KYC verification, bank account linking, e-signature, and trading account activation. The solution should define a normalized backend database schema capturing user profiles, KYC documents, verification statuses, and stage-wise progress. It must include scheduled jobs and database processes to automate status transitions, handle third-party verification callbacks (PAN, Aadhaar, bank), trigger reminders for drop-offs, and ensure data consistency across onboarding stages. Finally, build a Tableau dashboard visualizing funnel metrics such as stage-wise conversion rates, drop-off points, and average time-to-activation for stakeholder monitoring.
 


 Component	Keep?	Reason
Amazon S3	✅	Landing zone for synthetic data
AWS Lambda	✅	Trigger pipeline on file upload
AWS Step Functions	✅	Orchestrates ETL workflow
AWS Glue (PySpark)	✅	Cleaning, validation, transformation
Amazon RDS (PostgreSQL)	✅	Stores Bronze, Silver, and analytics-ready data
dbt	✅	Builds Gold models, star schema, and KPIs
Tableau	✅	Business dashboards
FastAPI	✅	Backend for chatbot APIs
LangChain	✅	Routes between SQL and RAG workflows
FAISS	✅	Stores embeddings for SOPs, FAQs, and policy documents
OpenAI / Bedrock	✅	Generates natural-language responses