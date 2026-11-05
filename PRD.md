# **Product Requirements Document (PRD)**
## **MOVE-Rx PHG: Sistem Pengurusan & Penilaian Objektif Pertukaran Anggota Farmasi Negeri Pahang**

---

## **1. Project Overview**
- **Project Name:** MOVE-Rx PHG (Mobility & Objective Valuation Evaluation for Pharmacy in Pahang) / Sistem Pertukaran Pegawai Farmasi Pahang (SPPF-PHG)
- **Project ID / NMRR ID:** NMRR ID-26-03968-MPU
- **Date:** 5 November 2026
- **Version:** 2.0 (Post-MREC Protocol & Ownership Migration)
- **Prepared By:** Muhammad Hafizul Aniq bin Khairul Hafidz, RPh (Principal Investigator, Pharmacy Services Department, JKNP)
- **Approved By:** 
  - Dr. Mastura binti Ahmad (Co-Investigator, JKNP)
  - Zaidah binti Abd Karim (Co-Investigator, JKNP)
  - Juliana binti Shamsudin (Co-Investigator, JKNP)
  - Medical Research & Ethics Committee (MREC), Ministry of Health Malaysia

### **1.1 Purpose of the Project**
The MOVE-Rx PHG system is a research-backed, web-based digital decision-support platform designed to standardize, objectively evaluate, and automate human resource transfer applications for pharmacy personnel under Jabatan Kesihatan Negeri Pahang (JKNP). Initiated to fulfill National Pharmacy Research Priority Domain 8.1 (Pharmacy Workforce Planning), the system converts subjective qualitative transfer appeals into a validated, defensible 45-point quantitative scoring framework.

### **1.2 Project Background**
Pharmacy personnel transfer and placement recommendations within JKNP historically relied on qualitative narratives and unstandardized administrative discretion. This often resulted in perceived favoritism, staff morale depression, and chronic maldistribution between high-density urban hospitals and remote/interior health clinics. To solve this, a 45-point rubric was drafted across 9 parameters (P1 to P9) covering personal urgency, family welfare, logistics, and service continuity. Under NMRR ID-26-03968-MPU, the tool is undergoing validation via a Modified Delphi study among Heads of Pharmacy Departments (KJF) and District Health Pharmacy Officers (PFK), coupled with an automated digital workflow that eliminates administrative friction.

### **1.3 Scope of the Project**
- **In-Scope:**
  - Single-page responsive web client (`index.html`) hosted on Cloudflare Pages / GitHub Pages.
  - Interactive e-Delphi scoring interface covering Parameters P1 through P9 with Likert scales (1–5) and discrete operational rubrics (P7/P8: 1, 3, 5; P9: 1, 3, 4, 5).
  - Dual-mode transfer handling: **Pertukaran Dalam Negeri (Pahang)** and **Pertukaran Luar Negeri (Antara Negeri)**.
  - Geospatial logistics engine (P3) supporting automated round-trip driving distance calculation ($2 \times \text{sehala}$) using Leaflet, OpenStreetMap/Nominatim, and OSRM, alongside manual distance overrides.
  - Identity verification and authentication via Google Identity Services SSO restricted exclusively to `@moh.gov.my` organizational accounts.
  - Real-time scoring tracker and categorical interpretation indicator (9–11: Tidak Disokong; 12–22: Ditangguhkan; 23–34: Disokong $\ge 3$ bulan; 35–45: Disokong Serta-Merta).
  - Serverless automation backend via Google Apps Script (`Code.gs`) interacting with a dedicated 33-column database tab named `Responses`.
  - Automated PDF certificate slip generation from a standardized Google Docs template and dispatch to the assessor's verified email.
  - Password-protected administrative dashboard for filtering and managing application statuses (`Dalam Proses`, `Diterima`, `Ditolak`).
- **Out-of-Scope:**
  - Automated human resource deployment execution in the HRMIS government system.
  - Storing transient home addresses on the final printed PDF slip (excluded by design to preserve officer privacy).
  - Direct integration with Public Service Commission (SPA) databases.

---

## **2. Stakeholders**

| Role / Entity | Key Contact / Group | Responsibility |
| :--- | :--- | :--- |
| **Principal Investigator & Lead Developer** | Muhammad Hafizul Aniq bin Khairul Hafidz | System architecture, frontend/backend engineering, research protocol adherence, NMRR liaison. |
| **Co-Investigators & Subject Matter Experts** | Dr. Mastura binti Ahmad, Zaidah binti Abd Karim, Juliana binti Shamsudin | Protocol review, Delphi synthesis, governance alignment with JKNP HR committee. |
| **Primary End Users (Assessors)** | Heads of Pharmacy Departments (KJF - Hospitals) & District Health Pharmacy Officers (PFK - PKDs) | Completing official objective appraisals for officers applying for transfer. |
| **Governing Body / Ethics** | Medical Research & Ethics Committee (MREC), NIH MOH | Ethical clearance, research compliance, participant rights monitoring. |
| **Secondary Beneficiaries** | Serving Pharmacy Personnel (UF9–UF14) | Applicants receiving equitable, transparent, and objective transfer consideration. |

### **Stakeholder Communication Plan**
- Bi-weekly research sprint updates between investigators during the Delphi consensus phase.
- Formal notifications via official MOH email and administrative circulars prior to Delphi Round 1 and Round 2 survey distribution.
- Annual post-trial performance report presented to the JKNP State Pharmacy Human Resource Committee.

---

## **3. Objectives and Goals**

### **3.1 Primary Objectives**
1. Digitize and validate a standardized 45-point transfer scoring instrument across 9 parameters for pharmacy personnel in Pahang.
2. Achieve expert consensus ($\ge 80\%$ stability, Interquartile Range $\text{IQR} \le 1.0$) on rubric weights using a 2-round Modified e-Delphi method.
3. Streamline assessment turnaround time from weeks to under 5 minutes per applicant, with instantaneous PDF generation and cloud archiving.

### **3.2 Success Criteria**
- **Consensus Threshold:** Panel consensus reached on all 9 parameters with an $\text{IQR} \le 1.0$ and median $\ge 4.0$.
- **Response Stability:** Wilcoxon signed-rank test across Delphi rounds yielding $p > 0.05$.
- **System Reliability:** 100% automated capture rate of submitted appraisals into the 33-column Google Sheet without column mismatch or sheet-pointer errors.
- **Delivery Rate:** Instantaneous PDF generation and delivery to the assessor's `@moh.gov.my` inbox within 30 seconds of submission.

---

## **4. Functional Requirements**

### **4.1 Section 1: Officer Information & Current Placement**
- **FR-1.1:** System shall capture Officer Full Name (`nama`), National Registration Identity Card (`kp`), and Grade (`gred`: UF9, UF10, UF12, UF13, UF14).
- **FR-1.2:** System shall capture Current Responsibility Center (`ptjSemasa`), covering BPF JKNP, 12 State Hospitals, and 11 District Health Offices (PKDs).
- **FR-1.3:** Dependent specification field (`val_fasilitiSemasa`) must dynamically render based on `ptjSemasa`:
  - If BPF JKNP $\rightarrow$ Dropdown of 3 Branches (Pengurusan, Amalan & Perkembangan, Penguatkuasaan).
  - If PKD $\rightarrow$ Autocomplete text input linked to a preloaded dataset of Pahang Health Clinics (`KK`).
  - If Hospital $\rightarrow$ Hidden mirror input passing the Hospital designation.

### **4.2 Section 2: Transfer Scope & Destination Handling**
- **FR-2.1:** System shall provide a binary selector for Transfer Type (`jenisPertukaran`):
  1. `Pertukaran Dalam Negeri (Pahang)`
  2. `Pertukaran Luar Negeri (Antara Negeri)`
- **FR-2.2:** If `Pertukaran Dalam Negeri` is selected:
  - Display `ptjDiminta` dropdown and dependent clinic/branch input (`val_fasilitiDiminta`).
  - P3 module computes daily round-trip commute from home to current facility.
- **FR-2.3:** If `Pertukaran Luar Negeri` is selected:
  - Display `negeriDiminta` dropdown with all Malaysian states, Federal Territories, and MOH Headquarters.
  - Automatically enforce P3 score recommendation to **Skor 5** ($> 300\text{ km}$ / Antara Negeri).

### **4.3 Section 3: Objective Scoring Parameters (P1 – P9)**
- **FR-3.1 (P1 - Alasan):** 5-point Likert scale evaluating grounds (No urgent reason = 1; Career = 2; Sick parents = 3; Spouse/PJJ = 4; Safety threat/Illness = 5).
- **FR-3.2 (P2 - Tempoh Semasa):** 5-point scale based on cumulative tenure ($<2\text{ yrs} = 1$ to $>8\text{ yrs} = 5$).
- **FR-3.3 (P3 - Beban Logistik & Jarak Harian):**
  - Interactive sub-module inside the P3 card.
  - Option A: Manual distance entry (`manualKm`) in km/day with automatic P3 tier mapping.
  - Option B: Geocoding search box via Nominatim OpenStreetMap with multi-query fallback cleaning.
  - Option C: Browser Geolocation API (`📍 Lokasi Semasa`) retrieving device GPS coordinates.
  - Option D: Clickable/draggable interactive Leaflet map pin for custom home placement.
  - OSRM API routing generating highway lines and round-trip calculation: $\text{Total km} = 2 \times \text{One-way driving km}$.
  - Route type modifier: Normal route vs. Interior/Hilly route (+1 score band adjustment).
- **FR-3.4 (P4 - Tanggungan Anak - Kuantiti):** 5-point scale (No children/Single = 1 to $\ge 4$ children = 5).
- **FR-3.5 (P5 - Tanggungan Anak - Umur):** 5-point scale (Secondary school = 2; Mixed = 3; Toddler/Primary = 4; Special needs/OKU = 5).
- **FR-3.6 (P6 - Tanggungan Lain):** 5-point scale evaluating caretaking of chronically ill/bedridden spouse or elderly parents.
- **FR-3.7 (P7 - Implikasi Sumber Manusia):** Discrete 3-point scale (**1, 3, 5**) evaluating operational impact without immediate replacement.
- **FR-3.8 (P8 - Kelangsungan Perkhidmatan):** Discrete 3-point scale (**1, 3, 5**) evaluating potential service shutdown (MTAC, CDR, Stor).
- **FR-3.9 (P9 - Perakuan Pengurusan):** Discrete 4-point scale (**1, 3, 4, 5**) capturing administrative recommendation.

### **4.4 Section 4: Administrative Governance & Document Audits**
- **FR-4.1:** Capture disciplinary inquiry status (`tatatertib_status` & `tatatertib_nota`).
- **FR-4.2:** Capture specialized portfolio succession training (`latih_status` & `latih_nota`).
- **FR-4.3:** Multi-select audit checkboxes for verified supporting documents (PJJ Employer Letter, Marriage Cert, Medical Reports, OKU Card, Clinic Appointments).

### **4.5 Section 5: Assessor Authentication & Narrative Review**
- **FR-5.1:** Assessor identity tied to two-stage dependent selection: Assessor PTj (`ptj_penilai`) must be chosen first to unlock and filter Assessor Designation (`jawatan_penilai`).
- **FR-5.2:** Assessor email validation via Google Identity Services (GIS) Sign-In button, strictly accepting `@moh.gov.my` domains only.
- **FR-5.3:** Narrative management justification box (`ulasan_kjf`) for mandatory remarks on borderline or low-scoring appeals.

### **4.6 Section 6: Data Dispatch & Administrative Dashboard**
- **FR-6.1:** Submissions package payloads via HTTP POST (`Content-Type: text/plain;charset=utf-8`) to prevent CORS preflight failures on Google Apps Script.
- **FR-6.2:** Administrative dashboard accessible under `#admin` route secured by authorization password, providing year/facility filtering and real-time status updates (`Dalam Proses`, `Diterima`, `Ditolak`).

---

## **5. Technical Architecture & Database Schema**

### **5.1 Architecture Stack**
- **Frontend:** Single-file Progressive Web Application (`index.html`) utilizing semantic HTML5, modern CSS3 with glassmorphism, animated Web Component grain gradients (`gradients.sh`), and ES6 JavaScript.
- **Geospatial & Mapping:** Leaflet.js (v1.9.4), OpenStreetMap Tile Layer, Nominatim Geocoding API, OSRM Public Driving Routing API.
- **Authentication:** Google Identity Services (GIS) OAuth2 client.
- **Middleware / Cloud Logic:** Google Apps Script Web App (`Code.gs`) bound to the target Google Sheet.
- **Persistence Layer:** Google Sheets (`Responses` tab locked via `getSheetByName("Responses")`).
- **Document Rendering:** Google Docs API via `DriveApp` & `DocumentApp` producing PDF binaries.
- **Email Dispatch:** GmailApp API sending multipart emails with PDF attachments.

### **5.2 33-Column Google Sheet Database Schema**

| Column Index | Column Header | Data Type | Source Field / Logic |
| :---: | :--- | :--- | :--- |
| **A (1)** | `ID Permohonan` | String | Generated unique ID: `MV` + timestamp |
| **B (2)** | `Tarikh` | DateTime | Formatted timestamp (`dd/MM/yyyy HH:mm`) |
| **C (3)** | `Nama Penuh` | String | Input: `nama` |
| **D (4)** | `No. KP` | String | Input: `kp` |
| **E (5)** | `Gred` | String | Dropdown: `UF9` to `UF14` |
| **F (6)** | `PTj Semasa` | String | Dropdown: `ptjSemasa` |
| **G (7)** | `Fasiliti Semasa` | String | Resolved clinic/branch/hospital |
| **H (8)** | `Jenis Pertukaran` | String | Radio: `Dalam Negeri` / `Luar Negeri` |
| **I (9)** | `PTj Diminta` | String | Dropdown: `ptjDiminta` or `'Pertukaran Luar Negeri'` |
| **J (10)** | `Fasiliti Diminta` | String | Resolved requested facility or `'Negeri: [X]'` |
| **K (11)** | `Jarak (km)` | Decimal / String | Round-trip km / `'> 300'` |
| **L (12)** | `Sumber Jarak` | String | `'OSRM (Pergi-Balik)'` / `'Manual'` / `'Antara Negeri'` |
| **M (13)** | `Jenis Laluan` | String | `'Biasa'` / `'Pedalaman/Berbukit'` / `'Antara Negeri'` |
| **N–V (14–22)** | `P1` to `P9` | Integer | Individual Likert/Discrete parameter scores |
| **W (23)** | `Cadangan P3` | Integer | Algorithmically suggested P3 score band (1–5) |
| **X (24)** | `Jumlah Skor` | Integer | Sum total of P1 through P9 (9 to 45) |
| **Y (25)** | `Tatatertib` | String | Disciplinary record status & notes |
| **Z (26)** | `Latih Pegawai Baru` | String | Portfolio training status & replacement name |
| **AA (27)** | `Dokumen Sokongan` | String | Comma-delimited list of verified documents |
| **AB (28)** | `Nama Penilai` | String | Input: `nama_penilai` |
| **AC (29)** | `Jawatan Penilai` | String | Dropdown: `jawatan_penilai` |
| **AD (30)** | `PTj Penilai` | String | Dropdown: `ptj_penilai` |
| **AE (31)** | `E-mel Penilai` | String | Authenticated `@moh.gov.my` email address |
| **AF (32)** | `Ulasan KJF` | String | Textarea: `ulasan_kjf` |
| **AG (33)** | `Status` | String | Default: `'Dalam Proses'` |

---

## **6. Non-Functional Requirements**

### **6.1 Security & Privacy**
- **Strict Single Sign-On:** System prohibits submission unless the assessor completes Google Identity validation with a verified `@moh.gov.my` account.
- **Privacy by Design:** Officer residential street addresses are transiently utilized in client-side memory for route coordinates and are **explicitly excluded** from the final generated PDF slip.
- **Data Encryption:** All in-transit network communication utilizes HTTPS/TLS 1.3. Google Drive archives are encrypted at rest under Google Workspace Enterprise infrastructure.

### **6.2 Performance & Responsiveness**
- Client interface must execute first meaningful paint in $< 1.5$ seconds on standard 4G mobile networks.
- Apps Script backend execution (`doPost`) must complete PDF cloning, tag substitution, blob compilation, email transmission, and spreadsheet append within $< 15$ seconds.

### **6.3 Usability & Glassmorphism Aesthetics**
- Visual theme utilizes a calm, low-frequency animated background gradient (`#BCCFF6` to `#14204a` at `speed: 0.4`), overlaid with frosted glass containers (`backdrop-filter: blur(16px)`).
- Mobile responsive layout accommodating smartphone and tablet resolutions used by hospital supervisors during rounds.

---

## **7. Assumptions, Constraints & Dependencies**

### **7.1 Assumptions**
- Assessors have active Ministry of Health Google Workspace credentials (`@moh.gov.my`).
- Participating health facilities possess stable network connectivity during assessment sessions.
- The Google Sheet destination tab remains named `Responses`.

### **7.2 Constraints**
- Google Apps Script enforces a 6-minute maximum execution time per trigger and daily email dispatch quotas (1,500 emails/day for Workspace accounts).
- OpenStreetMap and Nominatim rate-limits require debounced client requests and fallback queries.

### **7.3 Dependencies**
- Upstream availability of Leaflet CDN, Gradients.sh embed script, and Google Identity Services SDK.
- Valid Google Doc Template ID (`TEMPLATE_DOC_ID`) and Target Drive Folder ID (`DESTINATION_FOLDER_ID`) configured in `Code.gs`.

---

## **8. Risk Management & Mitigation**

| Risk Description | Severity | Likelihood | Mitigation Strategy |
| :--- | :---: | :---: | :--- |
| **Nominatim Geocoding Miss (New residential areas)** | Medium | High | Implemented multi-tier query cleaning, manual coordinate pin drag on Leaflet, and explicit manual km input override (`manualKm`). |
| **Apps Script Execution Writing to Wrong Tab** | High | Low | Hardcoded tab binding using `getSheetByName("Responses")` with auto-creation fallback instead of volatile `getActiveSheet()`. |
| **CORS Preflight Blocking on Web App URL** | High | Low | Forced HTTP POST encoding to `text/plain;charset=utf-8` on client fetch, accepted by `e.postData.contents`. |
| **Unauthorized Assessor Submissions** | High | Low | Enforced JWT token decoding and regex verification rejecting non-`@moh.gov.my` domains before payload assembly. |

---

## **9. Project Deliverables & Implementation Schedule**

```
Sep 2026 ──────> Oct 2026 ──────> Nov 2026 ──────> Dec 2026 ──────> Jan 2027 ──────> Feb 2027
 Protocol         NMRR/MREC        Web Platform    Ownership        e-Delphi         Pilot Trial &
 Drafting         Review           Deployment      Migration        Consensus        Final Report
```

1. **D1: Interactive Web Client (`index.html`)** - Fully responsive, Leaflet/OSRM-enabled, GIS-authenticated single-page application.
2. **D2: Automation Serverless Backend (`Code.gs`)** - 33-column Google Apps Script handler with automated Docs-to-PDF pipeline.
3. **D3: Standardized PDF Template (`Templat_Penilaian_SPPF-PHG_MOVE-Rx_PHG_v2.docx`)** - Pre-tagged official verification slip.
4. **D4: Clean Healthcare Dataset (`Pahang_Healthcare_Facilities_Data_Updated.csv`)** - Geocoded dataset of hospitals, clinics, and health offices in Pahang.
5. **D5: MREC Compliance Dossier** - Research protocol, Participant Information Sheet (PIS/ICF), and Investigator Response Table.

---

## **10. Sign-Off & Approvals**

| Name & Designation | Role | Signature | Date |
| :--- | :--- | :---: | :---: |
| **Muhammad Hafizul Aniq bin Khairul Hafidz**<br>Registered Pharmacist (RPh 25588) | Principal Investigator & Lead Developer | _______________ | 05/11/2026 |
| **Dr. Mastura binti Ahmad**<br>Senior Principal Assistant Director | Co-Investigator | _______________ | 05/11/2026 |
| **Zaidah binti Abd Karim**<br>Deputy State Health Director (Pharmacy) | Co-Investigator & Department Head | _______________ | 05/11/2026 |