# Phishing Email Analysis — The Greenholt Phish Challenge (TryHackMe)

This project documents a full phishing investigation performed on the **Greenholt Phish** email sample from TryHackMe.

## Objective  
Perform a phishing investigation on a malicious email sample to identify indicators of compromise (IOCs) and malicious attachments.

---

## Key Tasks Performed

### Extracted full email headers  
Analyzed the `.eml` headers using Google Admin Toolbox Message Header Analyzer and Microsoft Header Analyzer.

![Microsoft Header Analyzer (MHA)](images/mha.png)

---

### Identified the Return-Path and SPF domain  
Reviewed the header fields to find the Return-Path domain and the SPF-checking domain.

---

### Analyzed SPF, DKIM, and DMARC authentication results  
Authentication results indicated that SPF and DMARC checks failed.

![SPF and DMARC Failure](images/google-admin-toolbox-message-header.png)

---

### Examined Received headers to trace the sender’s origin IP  
The originating sender IP was extracted from the Received chain and investigated using ipinfo.io.

![IPInfo Lookup](images/ipinfo.png)

---

### Safely extracted attachments using ripmime  
The attachment was extracted safely from the `.eml` file using ripmime.

---

### Calculated SHA-256 hash and performed Threat Intelligence lookup  
The attachment hash was calculated using `sha256sum` and submitted to VirusTotal for analysis.

![ripmime and sha256](images/ripmime-sha256-file.png)

![Attachments](images/attachments.png)

![VirusTotal Report](images/virustotal-report.png)

---

## Findings

- SPF and DMARC authentication failed, indicating spoofing  
- VirusTotal results showed the attachment was malicious

---

## Indicators of Compromise (IOCs)

| Type               | Value                                                                 | Source / Evidence                     |
|--------------------|-----------------------------------------------------------------------|----------------------------------------|
| File Hash (SHA-256)| `2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f`    | Calculated using `sha256sum`           |
| Attachment Name    | `SWT_#09674321____PDF__.CAB`                                          | Extracted using ripmime                |
| SPF Status         | `fail`                                                                | Google Admin Toolbox                   |
| DMARC Status       | `unknown`                                                             | Google Admin Toolbox                   |
| VirusTotal Verdict | `malicious`                                                           | VirusTotal report screenshot           |

---

## Tools Used

- ripmime  
- sha256sum  
- file  
- VirusTotal  
- ipinfo.io  
- Microsoft Header Analyzer 
- Google Admin Toolbox Header Analyzer  

---

## Conclusion

SPF and DMARC checks failed (indicating sender spoofing), the originating IP is unrelated to the claimed sender, and the attachment's SHA-256 was flagged by Threat Intelligence — together confirming a phishing attack designed to compromise the recipient’s system and/or credentials.
