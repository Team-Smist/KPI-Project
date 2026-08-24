# Project Progress & Architecture Log
Project Title: AI-Enabled Unified Multi-Source KPI Dashboard Platform
Team: SMIST (University of Moratuwa)
Supervisors: Dr. Dilini & Lashan Silva (eZuite Cloud)

## 📅 Supervisor Sync & Progress Log

### Meeting #1 Overview
> **Mode:** Zoom  
> **Date:** July 21, 2026  
> **Time:** 3:30 PM – 4:10 PM  
> **Project Title:** AI-Enabled Unified Multi-Source KPI Dashboard Platform  

#### 🎯 Assigned Action Items
| Task Description | Assigned To | Status | Deadline |
| :--- | :--- | :---: | :--- |
| **Literature Review:** Study & analyze at least 5 similar systems/approaches | Team SMIST (All Members) | 🟡 In Progress | **July 27, 2026 (12:00 PM)** |

#### 📝 Key Discussion Points
* Finalized the official project title: **AI-Enabled Unified Multi-Source KPI Dashboard Platform**.
* Established initial requirements for background research before technical prototyping.
* Delegated the competitive/similar system study across all 5 group members (1 system/paper per member).

---

### Meeting #2 Overview
> **Mode:** In-person  
> **Date:** August 12, 2026  
> **Time:** Class Session  
> **Topic:** Software Project Proposal & Interim Guidelines  

#### 🎯 Assigned Action Items
| Task Description | Assigned To | Status | Deadline |
| :--- | :--- | :---: | :--- |
| **Draft Proposal Submission:** Submit proposal (Intro, Scope, Objectives, Features) | Team SMIST | 🟡 In Progress | **August 17, 2026 (12:00 AM)** |
| **Similar Systems Matrix:** Draft feature comparison table based on new guidelines | Team SMIST | 🟡 In Progress | **Pre-Interim** |
| **User Feedback Strategy:** Design 1-5 scale evaluation mechanism for 50+ users | Team SMIST | 🔴 Not Started | **Pre-Interim** |

#### 📝 Key Discussion Points
* **Aim & Objectives Documentation:** The Aim must be a single sentence that includes the frameworks and tools being used. Objectives must break down the aim into smaller, actionable parts showing *how* to achieve it.
* **Similar Systems Comparison Matrix Guidelines:** 
  * Features must be listed in descending order (from Most Common $\rightarrow$ Least Common/Uncommon).
  * Must identify **Mandatory** features among the common ones.
  * Must include at least **one new/novel feature** that is completely unique to our system and not present in others.
  * Feature availability should be marked with checkmarks (adding qualitative tags like *poor, limited, or complex* if needed) and crosses (X).
  * **Interim Goal:** By the Interim Level, our system's column must show all features as "Available".
* **Referencing Rules:** Research papers must be explicitly cited in the Background, Motivation, Intro, and Problem in Brief sections using standard bracketed numbering (e.g., [3], [6], [7]).
* **Project Scope Lock:** Course rules dictate that changes can still be made at the proposal level, but **no changes** will be allowed after the interim level.
* **User Feedback System:** It is a mandatory requirement to collect "real feedback" from at least **50 people** using a 1 to 5 rating scale (1 = very poor, 5 = very good).

---

### Meeting #3 Overview (Pre-Meeting Sprint & Homework)
> **Mode:** Group Sync / WhatsApp Collaboration  
> **Date:** August 21 - August 23, 2026  
> **Topic:** System Architecture Blueprint, Use Case Modeling & Role Specifications  

#### 🎯 Assigned Action Items
| Task Description | Assigned To | Status | Deadline |
| :--- | :--- | :---: | :--- |
| **System Blueprinting:** Finalize markdown documentation for all 4 tier roles | Team SMIST | 🟢 Completed | **August 23, 2026** |
| **Use Case & Workflow Diagrams:** Compare versions (V2 vs V3) and finalize | Shahanmi / Dev | 🟡 In Progress | **Next Weekend Meeting** |
| **Presentation Prep:** Prepare role-based explanations for the supervisor | All Members | 🟡 In Progress | **Next Weekend Meeting** |

#### 📝 Key Discussion Points
* **System Modules & Use Case Finalization:** Successfully mapped out 28 core Use Cases across 5 primary system modules: (1) Data Management & Integration, (2) Security, Access & RLS, (3) Dashboards & Visualization, (4) AI & ML Analytics, and (5) Alerting & Diagnostics.
* **Actor Hierarchy & RBAC:** Established a strict 4-tier Role-Based Access Control (RBAC) model. 
  * *Super Admin:* Complete tenant configuration & AI policies.
  * *Admin:* Division scope, team creation, data source management.
  * *Team Leader:* Member activity assignment, transfer requests, root-cause logging.
  * *User/Member:* RLS-governed personal KPI exploration.
* **Data Governance vs. Quality:** Reached a critical architectural decision regarding data validation. The system will utilize **OpenMetadata** for Data Governance, Lineage, and Cataloging, and **Great Expectations** for rule-based Data Quality Validation. (Commercial tools like Atlan were rejected to maintain open-source university project requirements).
* **AI Decision Support System (DSS):** Defined the AI's autonomy level. The DSS will operate in an "Advisory" capacity utilizing Multi-Criteria Decision Analysis (MCDA), meaning it will simulate what-if scenarios and recommend actions, but humans must explicitly approve them.
* **Presentation Delegation:** To effectively explain the complex architecture during the upcoming supervisor meeting, the team delegated presentation segments matching the system's actual roles: Ishara (Super Admin), Sithira (Admin), Thiseni (Team Leader), and Madhura (User).
