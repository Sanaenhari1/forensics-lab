\# 🔍 Digital Forensics Investigation Lab



Step-by-step investigation of public forensic disk images from \[Digital Corpora](https://digitalcorpora.org/), analysed with \*\*Autopsy\*\*. Every step is documented with screenshots so the work can be reproduced.



> ⚠️ Educational / self-study project. Solutions to these scenarios are widely available online; this repository documents my own analysis.



\---



\## 📁 Repository Structure



```

forensics-lab/

├── README.md

├── case-01-m57-jean/

│   ├── report.md

│   ├── timeline.md

│   └── screenshots/

│       ├── 01-case-created.png

│       └── ...

├── case-02-nps-2011-scenario4/

│   ├── report.md

│   ├── timeline.md

│   └── screenshots/

└── tools-and-notes/

&#x20;   └── autopsy-setup.md

```



\---



\## 🧰 Tools



| Tool | Purpose |

|------|---------|

| Autopsy | Main forensic analysis platform |

| The Sleuth Kit (`mmls`, `fls`, `icat`) | Command-line analysis |

| libewf (`ewfverify`, `ewfinfo`) | Read / verify E01 images |

| FTK Imager (optional) | Convert E01 to raw (dd) |

| Hex viewer | Byte-level inspection |



\---



\## 🗂️ Evidence Overview



| # | Case | Image file | Source | MD5 |

|---|------|-----------|--------|-----|

| 01 | M57-Jean | `nps-2008-jean.E01` + `nps-2008-jean.E02` | \[Scenario page](https://digitalcorpora.org/corpora/scenarios/m57-jean/) | \_to fill\_ |

| 02 | NPS 2011 Scenario 4 | `nps-2011-scenario4.E01` | Digital Corpora (2011 NPS Language Drives) | `f164e98cb684e00b767e1f05654a4638` |



\---



\# 🕵️ Case 01 — M57-Jean



\## Scenario (English)



M57-Jean is a single-disk scenario about the exfiltration of corporate documents from the laptop of a senior executive. M57.Biz is a small start-up. A few weeks after its creation, a confidential spreadsheet with the names and salaries of key employees was found posted in the comments section of a competitor's website. The spreadsheet existed only on the laptop of one officer, \*\*Jean\*\*.



Jean claims she has no idea how the data left her laptop and that she must have been hacked.



\*\*Objective:\*\* determine how the data was stolen, or whether Jean is as innocent as she claims.



\## السيناريو (العربية)



سيناريو M57-Jean يعتمد على صورة قرص واحدة، ويتناول تسريب وثائق سرية من حاسوب مديرة تنفيذية. في شركة ناشئة اسمها M57.Biz، عُثر بعد أسابيع من تأسيسها على جدول بيانات سري (أسماء الموظفين ورواتبهم) منشور في تعليقات موقع شركة منافسة. كان الجدول موجوداً فقط عند \*\*جين (Jean)\*\*، وهي تدّعي أنها تعرضت للاختراق.



\*\*الهدف:\*\* معرفة كيف سُرّبت البيانات، أو التأكد مما إذا كانت جين بريئة.



\## Investigation Questions



\- \[ ] Where is the confidential spreadsheet on the disk?

\- \[ ] Was it sent by email? By whom, to whom, and when?

\- \[ ] Is there evidence of an external intrusion, or was it an insider?

\- \[ ] What do web history, deleted files and USB devices show?



\## Step-by-Step Log



\### Step 1 — Prepare and verify the evidence

\- Put `.E01` and `.E02` in the \*\*same folder\*\*.

\- Verify integrity and record hashes.



```bash

ewfverify nps-2008-jean.E01

ewfinfo nps-2008-jean.E01

```



📸 `screenshots/01-hash-verification.png`



\*\*Result:\*\* \_write what you found\_



\---



\### Step 2 — Create the case in Autopsy

\- \*New Case\* → name `M57-Jean`, set the output folder.

\- \*Add Data Source\* → \*Disk Image or VM File\* → select the `.E01`.

\- Note the timezone used.



📸 `screenshots/02-case-created.png`



\*\*Result:\*\* \_write what you found\_



\---



\### Step 3 — Run ingest modules

Enabled: Recent Activity, Hash Lookup, File Type Identification, Keyword Search, Email Parser, Extension Mismatch Detector, Interesting Files Identifier.



📸 `screenshots/03-ingest-modules.png`



\*\*Result:\*\* \_write what you found\_



\---



\### Step 4 — Disk and partition layout

```bash

mmls nps-2008-jean.E01

fsstat -o <offset> nps-2008-jean.E01

```



📸 `screenshots/04-partitions.png`



\*\*Result:\*\* \_write what you found\_



\---



\### Step 5 — Locate the spreadsheet

\- Keyword search: `salary`, `salaries`, `.xls`, `.xlsx`.

\- Record path, size, MAC times (Modified / Accessed / Created), MD5.



📸 `screenshots/05-spreadsheet-found.png`



| Field | Value |

|-------|-------|

| Path | |

| Size | |

| Created | |

| Modified | |

| Accessed | |

| MD5 | |



\---



\### Step 6 — Email analysis

\- \*Email Messages\* in the tree view.

\- Who sent the file, to whom, when, with which attachment?



📸 `screenshots/06-emails.png`



\*\*Result:\*\* \_write what you found\_



\---



\### Step 7 — Web activity

\- Web History, Web Downloads, Cookies, Search terms.



📸 `screenshots/07-web-history.png`



\*\*Result:\*\* \_write what you found\_



\---



\### Step 8 — USB devices and deleted files

\- \*Devices Attached\*, \*Deleted Files\*.



📸 `screenshots/08-usb-deleted.png`



\*\*Result:\*\* \_write what you found\_



\---



\### Step 9 — Timeline

\- Use Autopsy \*Timeline\* to correlate events around the leak.



📸 `screenshots/09-timeline.png`



| Date/Time (UTC) | Event | Source artifact |

|-----------------|-------|-----------------|

| | | |



\---



\## ✅ Findings — Case 01



| # | Finding | Evidence | Confidence |

|---|---------|----------|------------|

| 1 | | | |



\## 🧾 Conclusion — Case 01

\_Was Jean hacked or did the data leave another way? Summarise with evidence.\_



\---



\# 🕵️ Case 02 — NPS 2011 Scenario 4



| Field | Value |

|-------|-------|

| Image | `nps-2011-scenario4.E01` |

| Type | Image |

| Sector size | 512 |

| Timezone | Etc/GMT |

| MD5 | `f164e98cb684e00b767e1f05654a4638` |



\## Scenario

\_Paste the scenario description from the Digital Corpora page here.\_



\## Investigation Questions

\- \[ ] \_to define after reading the scenario\_



\## Step-by-Step Log



\### Step 1 — Load the image

📸 `screenshots/01-image-loaded.png` (Autopsy Directory Listing with the hash shown)



\*\*Result:\*\* \_write what you found\_



\### Step 2 — File system and partitions

📸 `screenshots/02-partitions.png`



\### Step 3 — Ingest results

📸 `screenshots/03-ingest-results.png`



\### Step 4 — Key artifacts

📸 `screenshots/04-artifacts.png`



\### Step 5 — Timeline

📸 `screenshots/05-timeline.png`



\## ✅ Findings — Case 02



| # | Finding | Evidence | Confidence |

|---|---------|----------|------------|

| 1 | | | |



\## 🧾 Conclusion — Case 02

\_Summary.\_



\---



\## 🔐 Chain of Custody



| Date | Action | Performed by | Hash after action |

|------|--------|--------------|-------------------|

| | Downloaded from Digital Corpora | | |

| | Hash verified | | |

| | Opened read-only in Autopsy | | |



\## 📸 Screenshot Convention



\- Name files `NN-short-description.png` and keep them in each case's `screenshots/` folder.

\- Embed them in the report like this:



```markdown

!\[Spreadsheet found](screenshots/05-spreadsheet-found.png)

```



\## 📚 References



\- \[Digital Corpora](https://digitalcorpora.org/)

\- \[Autopsy documentation](https://sleuthkit.org/autopsy/docs/)

\- \[The Sleuth Kit](https://www.sleuthkit.org/)

\- \[libewf](https://github.com/libyal/libewf)



\## 📄 License



Documentation in this repository: MIT. The disk images belong to Digital Corpora and are \*\*not\*\* included here; download them from the source.

