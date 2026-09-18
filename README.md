# Malware-Analysis

## *INTRODUCTION* :
In a real Security Operations Centre, an alert rarely arrives with a clean label saying "this is
ransomware" or "this is a trojan." Instead, a SOC Analyst receives a suspicious file, an unusual process,
or an EDR detection, and must quickly answer three questions: What does this file actually do? How did
it get here? What else in the environment might be affected? Malware analysis is the discipline that
answers these questions in a structured, repeatable way.
- Static analysis answers: What is this file, and what does its structure suggest about its capability
without any risk of it running.
- Dynamic analysis answers: What does this file actually do when it is allowed to run — which
processes, files, registry keys, and network connections does it touch.
- Together, they produce IOCs (hashes, IPs, domains, file paths, registry keys) that can be pushed
into a SIEM (e.g. Wazuh), firewall, or EDR to detect and block the same threat across the
organisation.

### Lab Environment and Setup :
- The lab used for this project reuses the same VirtualBox lab environment built for earlier projects
(Wazuh SIEM lab, Metasploit shell lab), extended with malware-analysis-specific tooling.

**Network Isolation**: 
Network isolation is the single most important safety control in this lab. The detonation VM must never
be allowed direct, unrestricted internet access while a live sample is running, otherwise the sample could
reach a real command-and-control (C2) server, download a second-stage payload, or spread to real
infrastructure.
- The Windows 11 VM network adapter is set to Host-Only (the same vboxnet0 / 192.168.56.x
network used in the Wazuh lab) or fully disabled for the highest-risk samples.
- For dynamic analysis where fake network responses are needed (so the malware "believes" it has
internet access), INetSim or FakeNet-NG is run on the Kali/analyst side to simulate DNS, HTTP,
and HTTPS responses without any real outbound connection.
<img width="857" height="954" alt="Screenshot_20260918_215244" src="https://github.com/user-attachments/assets/f67a64cf-6432-4515-8b71-c9f6359ca6ec" />

- A clean VM snapshot is taken before every detonation, so the machine can be reverted to a
known-good state after each sample — malware execution is never done on a machine that will
be reused without reverting.

## Part A — Static Malware Analysis
**What Is Static Analysis**
- Static analysis examines a file's structure, code, and metadata without executing it. It is always the first
- step in the workflow because it is safe, fast, and often gives enough information (a known hash, a known
packer, embedded strings) to triage the sample before deciding
whether a full dynamic detonation is even necessary.

- I Downloaded the EICAR Malicious file for static analysis in my VM windows machine


- i downloaded the Malicious file in My windows
- Now start analysing the malicious file 

<img width="852" height="713" alt="Screenshot_20260918_222128" src="https://github.com/user-attachments/assets/2adcb119-0d74-44e4-a4fd-a81970f42cce" />

- find the Hash with using: Get-FileHash file_name
- find string: Get-Content File_name

- Use Pestudio for analysing the malwaree 

<img width="994" height="982" alt="Screenshot_20260918_223845" src="https://github.com/user-attachments/assets/dd5f5695-3247-4020-9383-cd8e91aaeac7" />

- PEstudio use for static inspection
- DIE Detect it easy tool use for identify the technology packer
- strings tool use to find redable text inside the binary
- Ghidra use  for reverse-engineering
