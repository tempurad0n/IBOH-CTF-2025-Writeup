# Writeup - The Whistle Blower

## Challenge Description
> **Challenge:** The Whistle Blower  
> **Category:** OSINT  
> **Challenge Creator:** JaydenZy  
>
> An internal investigation into Panopticon Systems has gone public—sort of. The investigator, Smithing-John thought they were being careful, but digital breadcrumbs remain scattered across multiple platforms. The researcher tried, but the trail is there for those who know how to look. To begin, just know that John is careless and always **forgitful**.
> 
> **Your Mission:** Follow the investigator’s footsteps and find the evidence they were trying to protect. Reconstruct the investigation and locate the final evidence.
>
> **Flag Format:** `BOH25{flag_content}`
> 
> <img width="397" height="900" alt="image" src="https://github.com/user-attachments/assets/31d93f7e-807c-43e5-84d4-a7bc06237102" />

---

## Initial Analysis & Reconnaissance
From the description, I identified several key hints that set the direction for the investigation:
* **Panopticon Systems** → likely the company under investigation.
* **Smithing-John** → the investigator and target username.
* **"Careless and always forgitful"** → Intentional misspelling of "forgetful". "For-git-ful" strongly hints at **GitHub**. 💡

---

## Step 1: Tracking Smithing-John on GitHub
Starting with GitHub, I searched for the user `Smithing-John` and found only one account. 

<img width="746" height="422" alt="image" src="https://github.com/user-attachments/assets/33db3730-d108-43f8-99c0-5445e5cc48a7" />

The account had activity spread over two days, with 13 contributions to a single repository.

<img width="711" height="399" alt="image" src="https://github.com/user-attachments/assets/84c32dbb-6eaf-40d9-927b-a1ceb87c1fc9" />


Inside that repo, there were multiple files. Since the challenge description emphasized carelessness, I suspected critical data might be hidden in the history. 

Using **Insights → Network**, I explored the commit history. The most suspicious commit was titled:
> *"adding investigation notes and evidence tracking"*

<img width="940" height="461" alt="image" src="https://github.com/user-attachments/assets/a2faa809-e93c-4e9f-be41-76330803e0fe" />

---

## Step 2: Investigation Notes

<img width="940" height="681" alt="image" src="https://github.com/user-attachments/assets/016a1a94-c23b-4d83-bc17-083bba5e817b" />

Inside that commit, I found two files:
1.  `evidence_checklist.md`
2.  `investigation.txt` — This immediately looked suspicious because of the header: `PERSONAL INVESTIGATION NOTES — CONFIDENTIAL`.

In `investigation.txt`, I found a broken Google Drive link. One section of the URL appeared to be **Base64 encoded** due to the `=` padding at the end:
`[https://drive.google.com/drive/folders/MW1kNUY2aG5wMGwtQzdFQUNiTFFuZjlfVGJ2TlZiMHA2P3VzcA=sharing]`

<img width="940" height="621" alt="image" src="https://github.com/user-attachments/assets/5bf34d81-2949-4188-a46c-1c86c3ba1a9f" />

I used **CyberChef** to decode the string `MW1kNUY2aG5wMGwtQzdFQUNiTFFuZjlfVGJ2TlZiMHA2P3VzcA=`, which gave me the corrected ID: `1md5F6hnp0l-C7EACbLQnf9_TbvNVb0p6?usp`.

Reconstructing the link led me to the real Google Drive folder:
👉 [Google Drive Link](https://drive.google.com/drive/folders/1md5F6hnp0l-C7EACbLQnf9_TbvNVb0p6?usp=sharing)

---

## Step 3: The Google Drive Rabbit Hole

<img width="940" height="310" alt="image" src="https://github.com/user-attachments/assets/86b5a05b-c383-4816-9415-80608c38745f" />

The Drive folder contained six files, most of which were rabbit holes containing fake flags:

* `r3ad_me_f1rst.txt`
* `backup_logs.txt` (Fake flag)
* `INCIDENT REPORT - Q1` (Fake flag + Hint: *"check_all_documents_thoroughly"*)
* `investigation_report`

The file `investigation_report` stated that the document was moved to a "more secure location." The hint *"check_all_documents_thoroughly"* prompted me to check the **Version History** of the Google Doc.

---

## Step 4: Digging Into File Versions
By looking at the earlier versions of the `investigation_report`, I found two major clues:

1. **Pastebin Creds:**
   
    <img width="940" height="494" alt="image" src="https://github.com/user-attachments/assets/fce4e68a-ff85-448e-a93e-0c5ed9b6532b" />
    
   > "All detailed findings documented and password protected on Pastebin."
   > * **Username:** `john-is-smithing`
   > * **Paste ID:** `LQ66NRap`


2. **Vercel Migration:**
   
   <img width="940" height="214" alt="image" src="https://github.com/user-attachments/assets/f1b70a86-21a1-482f-ae96-63f868b55b24" />
   
   > "Moving documentation to a more secure hosting solution on Vercel."

---

## Step 5: Pastebin Analysis
The direct Paste link (`pastebin.com/LQ66NRap`) was password locked. However, checking the public profile for `john-is-smithing` revealed another paste titled **"WHISTLEBLOWER INVESTIGATION STEPS – DRAFT"**.

<img width="600" height="700" alt="image" src="https://github.com/user-attachments/assets/7b369bf8-b526-4c10-8724-344483e9020a" />

This draft contained critical info:
* Mentions of **Forms A–D**.
* **Backup Deployment Config:**
    * **Subdomain:** `js-2847-archive`
    * **Hosting:** Vercel

This led me to the next target: `https://js-2847-archive.vercel.app/`

---

## Step 6: The Vercel Portal

<img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/66fc5d96-f4f7-4d39-8e5a-e4e6e377541f" />

The website was titled **"Panopticon Investigation Portal"**. It had several buttons, but investigating the page revealed:

* **About This Portal:** Warned that Google Form responses might be visible to others.

<img width="868" height="185" alt="image" src="https://github.com/user-attachments/assets/4a0c8bfc-9bf4-4092-9f5f-af458ccdded5" />

* **Contact Investigator:** Instructed to use **Form C (Employee Testimonies)** and mark it as **"URGENT"**.
  
<img width="872" height="469" alt="image" src="https://github.com/user-attachments/assets/66835985-72dd-44a3-ad4f-38628f9d778f" />


The **"Submit Testimony"** button redirected to a Google Form.

---

## Step 7: Submitting the Testimony

<img width="940" height="446" alt="image" src="https://github.com/user-attachments/assets/820f3232-4010-4e1e-8add-e1f93076b056" />

Following the instructions found on the portal:

1. I accessed **Form C**.
2. I filled out the testimony and marked the urgency as **"URGENT"**.
3. After submitting, the form displayed a link to **"See previous responses"**.
   
   <img width="736" height="213" alt="image" src="https://github.com/user-attachments/assets/b309574f-3433-49a1-9d3c-116e07773190" />


Since the portal warning hinted that responses were public, I checked the previous responses from other "employees." Buried in the list of responses was the investigator's final note containing the flag.

<img width="806" height="366" alt="image" src="https://github.com/user-attachments/assets/aabccc5b-0e04-42a1-972d-74edc8b05ed6" />

---

## Final Flag
**Flag:** `
BOH25{wh1stl3bl0w3r_tr4c3s_r3v34l_truth}`

## Conclusion
This challenge was an excellent exercise in OSINT pivoting. It required tracking a target across multiple platforms (GitHub → Google Drive → Pastebin → Vercel → Google Forms) by systematically exploiting the "carelessness" of the user.




