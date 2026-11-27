# Lab Note — MVP Plan

An electronic lab notebook (ELN) for **small labs** with a few experimental setups,
some of them shared with other labs. Any lab can register its own setups; data can
come from those setups or from outside (other labs, central facilities, collaborators).

- **Pilot:** one lab first, but built as a general platform for any lab.
- **Timeline:** 4 weeks.
- **Principle:** keep every feature, each in a simple form.

---

## 1. Decisions

| Topic | Decision |
|---|---|
| First users | One pilot lab; the design works for any lab |
| Setups | Each lab registers its own; PI or Lab Manager defines the template and fields |
| Data source | Registered setups **or** external; upload any file |
| Analysis | Standard fits **and** AI, both in the MVP |
| AI features | Explain the result, suggest a fit, advise the next experiment, daily summary and plan |
| AI privacy | Analysis AI runs only when the user clicks; provider must not train on our data |
| Daily summary sharing | User is asked daily whether to share **and** the PI can request an update at any time (user approves) |
| Login | Email and password, or Google |
| Hosting | Cloud |

---

## 2. Roles and permissions

```
PI (lab owner)
 ├── Lab Manager(s)    → all setups + members
 └── Setup Manager(s)  → only the setups assigned to them
       └── Members      → use the setups they are certified for
```

| Role | Assigned by | Can do |
|---|---|---|
| **PI (owner)** | Whoever creates the lab | Everything: assign or remove Lab Managers and Setup Managers, manage members and settings, see lab data by lab policy |
| **Lab Manager** | PI | Manage all setups, certify users, approve join and access requests, manage bookings |
| **Setup Manager** | PI or Lab Manager | For their assigned setups only: edit the template, certify users, approve outside access requests, manage bookings and maintenance, see the usage log |
| **Member** | PI or Lab Manager (invite or approved request) | Experiments, analysis, booking certified setups |
| **Outside user** | Approved by the setup's manager | Book a shared setup only; their data stays in their own ELN |

Rules:
- A lab can have several Lab Managers; a setup can have several Setup Managers, and one person can manage several setups.
- A setup with no Setup Manager is handled by the Lab Managers and PI.
- Lab Managers cannot remove or change the PI.
- Setup Managers cannot see other people's experiment data.

### Joining a lab
- Anyone can create an account. A personal notebook works without a lab.
- Join by **invite** (link or email, with an expiry) **or** by **request**, which the PI or Lab Manager approves and assigns a role.
- No lab data is visible until approved. Every request, approval and rejection is logged.

### Data ownership
- The person who did the experiment always has access to it.
- The PI has access according to lab policy.
- Other members see an entry only if it is shared with the lab.
- Setup Managers and host labs see usage logs only, not the content.

---

## 3. MVP features

### 3.1 Accounts and labs
- Sign up (email and password, or Google), create a lab, or request to join one
- Invite members; approve or reject requests
- Assign and remove Lab Managers and Setup Managers

### 3.2 Setup registry
- Add a setup: name, type, description, location
- **Custom fields template** (number with unit, text, dropdown, date), e.g. voltage, frequency range, temperature
- Setup Manager(s)
- Booking on or off
- Certificate required: yes or no
- **Visibility:** lab only, or shared with other labs
- Status: available, under maintenance, or out of order (blocks booking)
- **QR label:** the ELN generates a QR per setup and the manager prints it as a sticker for the instrument
  - Scanning it with a phone camera opens the ELN (login required) and shows your current booking/entry, otherwise the setup page (status, calendar, book, request access)
  - The QR is only a link; access still needs login and permission

### 3.3 Certificates and access
- The manager picks **certified users** from the lab member list, with an optional expiry and an optional certificate file
- **Outside users** (other labs) can request access to shared setups, giving a reason and optionally a certificate. The manager approves or rejects and sets an expiry.

### 3.4 Experiment notebook
- Entry: title, setup (or "external"), the setup's fields, sample, notes, tags
- Upload files: CSV, TXT, XLSX, images
- Automatic timestamps and edit history
- Private, or shared with the lab
- Search across titles, notes, tags and field values

### 3.4a Note editor and import
- Note editor in the style of Notion and Obsidian:
  - Markdown-based (plain text, never locked in)
  - Blocks: headings, checklists, tables, images, code, LaTeX equations, callouts
- `[[links]]` between notes and entries, with backlinks
- Embed data plots, fit results and bookings in notes
- Templates (e.g. "Daily lab note", "Experiment write-up")
- **Import:** an Obsidian vault (Markdown + attachments) and Notion's Markdown/CSV export
- The ELN is the main record. External apps get copies; anything synced back comes in as a new, timestamped version.

### 3.4b Getting data from instruments (most are offline)
Registering an instrument creates its digital record: identity, access, booking, template and where its data goes. Moving the data itself is a separate step that must work without internet on the instrument.
- **Manual upload:** USB → upload to the entry from any computer or phone
- **QR code on the instrument:** scan → opens your current booking or entry → upload a file or a photo of the screen or printout
- **Manual entry:** type readings into the setup's template fields
- **Matching by booking:** on upload, the ELN suggests the matching booking and experiment (who, which instrument, when)
- **Booking → entry → data follow-up:**
  - An entry is linked to its booking; a booking without an entry gets a draft entry automatically
  - When the booking ends, the ELN asks for the data: upload, QR/phone, enter values, or "no data" with a reason
  - Until then the entry shows **Data pending**, with reminders later that day and the next morning
  - Experiments without a booking: the ELN asks "Any data to add?" when the entry is marked done
  - Setup Managers see "booked but no data uploaded"; PIs see it only if lab policy allows
- **Raw file kept unchanged:** the original is stored with a checksum; analysis uses a copy

### 3.5 Analysis
- Data preview, then pick X and Y columns, then plot
- Standard fits: linear, polynomial, exponential, power law, Gaussian and Lorentzian peaks, Arrhenius, custom equation
- Output: fit parameters with errors, R², and the residuals plot
- Save the plot and fit results to the entry
- **AI buttons** (on click only):
  - **Suggest fit:** recommends a model and explains why
  - **Explain result:** what the data and fit show, fit quality, anomalies
  - **Next experiment:** suggestions based on this result and earlier entries

### 3.6 Booking
- Calendar per bookable setup
- No double booking: when two people book at once, the first one wins and the other sees "slot just taken" with other free slots
- Only certified users (or approved outside users) can book
- Managers can block slots for maintenance and cancel bookings
- Usage log: who, when, how long

### 3.7 Daily summary and plan
- The AI drafts a summary of the day's entries and a plan for tomorrow
- **User-initiated:** each day the user is asked "Share with PI?", reviews and edits the draft, then clicks **Share** or **Not today**
- **PI-initiated:** the PI can request an update from a member at any time; the member gets the AI draft, reviews it and sends it
- The PI sees a simple list of received summaries per member

---

## 4. Four-week schedule

| Week | Build | Done when |
|---|---|---|
| **1** | Project setup, login, labs, roles (PI, Lab Manager, Setup Manager, Member), invites and join requests, setup registry with custom fields | A lab can be created, a member invited, and a setup registered with its own fields and manager |
| **2** | Experiment entries, note editor (Markdown/blocks, `[[links]]`, templates), Obsidian and Notion import, file upload (USB/phone/QR), manual entry, booking matching, history, sharing, search | A real experiment can be recorded with its data files and old notes imported |
| **3** | Analysis (plots and fit library); booking calendar, certificates, outside access requests, usage log | Real data (e.g. LCR or I-V) can be fitted, and a shared setup booked by a certified user |
| **4** | AI features (suggest fit, explain, next experiment, daily summary with both sharing flows), polish, pilot | Pilot lab members use it for a week and give feedback |

---

## 5. Suggested technology

- **Web app:** Next.js (works on desktop, tablet and phone)
- **Database:** managed PostgreSQL
- **Files:** cloud object storage
- **Login:** an auth provider with email and password plus Google
- **Fitting:** Python with SciPy (standard scientific stack)
- **AI:** Claude API, through a provider setup with no training on customer data
- **Hosting:** Vercel (free tier to start)

---

## 6. Version 2 (after the MVP)

- Roundtable: several AI models critique a plan or result and give a combined verdict
- PI agent: status questions about members' work (within their sharing), blocker detection, meeting prep and calendar scheduling
- Waitlists and fair-use limits for booking
- Booking-ID login on instrument PCs, with automatic data upload from instrument software
- Automatic instrument data transfer:
  - **Watched folder:** an instrument PC on the local network saves to a shared folder, and a connected PC uploads new files
  - **Sync agent:** a small app on the instrument PC queues files while offline and uploads them when connected
  - **Bridge device:** a Raspberry Pi next to an offline instrument uploads its files over Wi-Fi or Ethernet
  - **Direct API:** homemade instrument software (e.g. the LCR app) uploads straight to the ELN
- Notes: Evernote (`.enex`) import, Notion API sync, Obsidian plugin, export to Markdown or PDF
- Library of setup templates (LCR, I-V, UV-Vis, XRD, …)
- Export in open formats (JSON, CSV, PDF, RO-Crate) and an API
- Institute-level Super Admin; transfer of lab ownership
- Inventory (chemicals, samples, consumables) with hazard and quantity tracking
- Self-hosted option for institutes that require it

---

## 7. Needed to start

- GitHub repo (`lab_note`)
- Cloud hosting account and AI API credits
- 2–3 real data files from different setups for testing the fits
- 2–3 pilot lab members for week 4
