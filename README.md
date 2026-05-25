# Full-malicious-URL-investigation-and-threat-correlation-

# Objective

Analyze malicious URLs using VirusTotal and ANY.RUN in order to identify phishing activity, redirection chains, payload delivery, and hidden infrastructure, and extract IOCs while assessing risk.

---

# tool used 
- VirusTotal  
- ANY.RUN

---

# Tasks

## Phase 1: Initial Analysis
1. Submit each URL to ANY.RUN and VirusTotal  
2. Expand shortened URLs (if any)  
3. Track full redirection chain  

## Phase 2: Deep Analysis
Monitor:
- DNS requests  
- HTTP/HTTPS traffic  
- Process activity  
- File execution (if any)  

## Phase 3: Identify Techniques
Check for:
- URL obfuscation  
- URL shortening  
- Domain shadowing  
- Use of cloud services  
- HTTPS abuse  

## Phase 4: Risk Assessment
Answer:
- Is it phishing / malware / exploit kit?  
- Is credential theft possible?  
- Are users at risk?  

## Phase 5: Reporting
- Findings  
- Indicators of Compromise (IOCs)  
- Risk level (Low / Medium / High)  
- Recommendations  

---
# Introduction

Malicious URL is a primary attack vector. A URL may appear legitimate at the beginning then become malicious. In this report I used VirusTotal and ANY.RUN to analyze given malicious links and understand their behavior.
The analysis identified:

- Phishing campaign  
- Multi-stage redirection  
- Potential payload delivery  
- Hidden malicious infrastructure  
- Sandbox evasion 

---
 
# 1. Target 1: https://informacoes-limpabrasil2026.netlify.app/ 
**1.1. VirusTotal investigation**
**1.1.1. Phase1: initial analysis**

The investigation of target https://informacoes-limpabrasil2026.netlify.app/ using virustotal indicates that:
- The URL was flagged as malicious by 8 of 95 security vendors and is associated with phishing activities. The detection score is not static and may evolve over time as additional security vendors analyze the link and update their assessment.
- The HTTP response returned a 404 code status, indicating that the resource is not available.
  
<img width="945" height="452" alt="image" src="https://github.com/user-attachments/assets/5d372ebe-f75b-415b-8d5a-1c55833de1e8" />


**1.1.2. Phase2: Deep Analysis**
Further investigation shows that:
- No DNS requests, HTTP/HTTPS traffic, and process activities were observed.
- The URL was recently created

<img width="945" height="450" alt="image" src="https://github.com/user-attachments/assets/dc66ddab-139b-48f4-b037-600284fb1afa" />


**1.2.Any.run investigation**
**1.2.1. Phase1: Initial analysis**

Initial analysis reveals that:
- The response indicates the resource is not available (code status 404).
- The response suggests that the link is invalid or doesn’t exist in Netlify and provides a troubleshooting link for sites not found
 
<img width="945" height="450" alt="image" src="https://github.com/user-attachments/assets/accc8943-53fc-45ce-a8a3-47b4a12c1899" />


Further investigation using a virtual machine indicates that the link redirects the victim to a page containing login interface. This page tricks the victim to copy a prompt and paste it into another page (AskNetlify).
 
<img width="983" height="342" alt="image" src="https://github.com/user-attachments/assets/ab271200-cc9e-4c01-8d7e-3b543d5e1647" />


The AskNetlify page is an AI chatbot assistant. This behavior is suspicious and lure the victim into interacting with the attacker

<img width="983" height="527" alt="image" src="https://github.com/user-attachments/assets/9adc0390-6599-468c-b434-a61035f42fb1" />
 
All these activities confirm that the URL is malicious and part of a phishing campaign.

**1.1.2. Phase2: Initial Analysis**

**DNS requests**
Multiple DNS requests were observed. All contacted domains were whitelisted and considered safe, except informacoes-limpabrasil2026.netlify.app domain whose reputation is unknown.
 
<img width="983" height="231" alt="image" src="https://github.com/user-attachments/assets/5d8a2924-7252-4d55-a872-d316746a5a6b" />

<img width="945" height="260" alt="image" src="https://github.com/user-attachments/assets/cb091f54-fdae-410a-bdc1-3458651c6318" />

**HTTP/HTTPS requests**

Multiple HTTPS requests were observed. All contacted URL appeared legitimate except:
- Failed Get query requesting https://informacoes-limpabrasil2026.netlify.app/ and indicating that the site is not available.
- Repetitive Failed POST queries to https://login.live.com/ppsecure/deviceaddcredential.srf  initiated by a Windows process named svchot.exe (ID 5316). These requests show unsuccessful attempts to add new credentials to the device.

<img width="983" height="241" alt="image" src="https://github.com/user-attachments/assets/fc6c344b-f719-4449-9dfd-c97bfba8a98e" />

<img width="945" height="224" alt="image" src="https://github.com/user-attachments/assets/0ac920b7-9861-47fb-9ddc-d83d7e8584d0" />

<img width="945" height="202" alt="image" src="https://github.com/user-attachments/assets/e5f3f189-7732-41e6-8083-6b76960437a1" />

<img width="945" height="359" alt="image" src="https://github.com/user-attachments/assets/4e726acc-e633-420e-acd4-96b2d81bb35d" />

<img width="945" height="352" alt="image" src="https://github.com/user-attachments/assets/0a2d0c4a-b9c4-4d99-873f-73e36386db4e" />

<img width="945" height="331" alt="image" src="https://github.com/user-attachments/assets/99246215-b5a3-4d50-91fb-6a582ada38f1" />

<img width="945" height="340" alt="image" src="https://github.com/user-attachments/assets/d6943780-992e-4157-ad0b-baf76fe02428" />

**Process activity**

Monitoring the process activity revealed that process ID 2364 was involved in Discovery tactics targeting the admin account, using the following techniques: 
- T012 --> Query registry
- T082 --> System information discovery 

<img width="945" height="683" alt="image" src="https://github.com/user-attachments/assets/a06480c2-7176-4720-a2ee-edb86919182b" />

<img width="945" height="448" alt="image" src="https://github.com/user-attachments/assets/23196d6b-c1b6-4c28-a550-a1939bb143c4" />


**1.3. Techniques identification**

The malicious URL https://informacoes-limpabrasil2026.netlify.app/ uses the following techniques to evade detection:
- Abuse of a trusted hosting platform (Netlify)
- HTTPS

**1.4. Risk assessment**

The assessment identified the following threats:
- Phishing 
- Potential credential thefts are possible as the initial page redirects the victim to a page  containing login interface. Users who open the link and follow redirection chain are at risk
- Risk level: high

**1.5. Finding**

The analysis of https://informacoes-limpabrasil2026.netlify.app/ results in:
- Discovery Tactic TA007, techniques T10012 and T1082
- Multi-stage redirection
- Attempts to add new credentials to the device

**1.6.IOC**

**URLs:**
- https://informacoes-limpabrasil2026.netlify.app/
- https://clients2.googleusercontent.com/crx/blobs/AQx-wa5gNjUbZEbhJvWB8rq8EGp-U7V18pcxMMBvAEPLmtMndDMphKwrvuJ5LT_TatNkvmvRK0k-gFR-mTEBKmjLyQHlP-OqJ_PO_yKnLnl5P-LIP-vs0RUl-m5v2pZkKwoAxlKa5d7akqSM9-Uq0gw7UzwwbrrcTXbu/GHBMNNJOOEKPMOECNNNILNNBDLOLHKHI_1_102_1_0.crx
- https://login.live.com/ppsecure/deviceaddcredential.srf

**IPs**
- 35.157.26.135
- 63.176.8.218
- 142.251.13.132
- 98.84.224.111

**Domain**
  informacoes-limpabrasil2026.netlify.app

# 2. Target 2: https://Skillwicked.cc/ 
**2.1. VirusTotal investigation**
**2.1.1. Phase1: initial analysis**

The investigation of target https://Skillwicked.cc/ using virustotal indicates that:
- The URL was identified as malicious by 8 of 95 security vendors and is associated to phishing activity. The detection score is not static and may change over time, as additional security vendors analyze and update their assessment.
The requested returned a 200 code status, indicating successful request
 
<img width="945" height="452" alt="image" src="https://github.com/user-attachments/assets/646811bd-2c09-4187-bd5b-fc4106196ae1" />

**2.1.2. Phase2: Deep Analysis**

Further investigation shows that:
- No DNS requests, HTTP/HTTPS traffic, and process activities were observed.
- The URL was recently created.
 
<img width="945" height="449" alt="image" src="https://github.com/user-attachments/assets/c4404bb3-0a2d-4b4c-af6a-22779495a5ca" />

**2.2.Any.run investigation**
**2.2.1. Phase1: Initial analysis**

Initial analysis of https://Skillwicked.cc/ reveals that:
- The site is active and under construction.
- The request returned a 200 code status, indicating that the resource is available
 
<img width="983" height="134" alt="image" src="https://github.com/user-attachments/assets/704a2e01-b298-4ddf-8f02-ca48fd1dab97" />

**2.1.2. Phase2: Deep Analysis**

**DNS requests**
Multiple DNS requests were observed. All contacted domains were whitelisted and considered safe, except skillwicked.cc domain whose reputation is unknown.
 
<img width="945" height="180" alt="image" src="https://github.com/user-attachments/assets/cb6a8812-ca1e-45fb-b1e1-35673f8dd6a2" />
 
<img width="945" height="180" alt="image" src="https://github.com/user-attachments/assets/224bf17d-205e-4136-b9f6-e1c875aa8882" />

**HTTP/HTTPS requests**
Multiple HTTPS requests were observed. All contacted URL appeared legitimate except:
- Repetitive failed POST queries to https://login.live.com/ppsecure/deviceaddcredential.srf  initiated by a process named svchost.com process (ID 5316). These requests indicate unsuccessful attempts to add new device credentials.
- Multiple successful GET queries from https://Skillwicked.cc/  requesting components of the landing page, which has unknown reputation. 

<img width="945" height="158" alt="image" src="https://github.com/user-attachments/assets/ec9ae5e9-ebfd-4731-baa9-579a2ce690d1" />

<img width="945" height="161" alt="image" src="https://github.com/user-attachments/assets/671953e1-6f28-46ff-b7c6-132837d6f02d" />

<img width="945" height="171" alt="image" src="https://github.com/user-attachments/assets/c2b0e345-5e9e-4edf-a449-64d05bdcbc80" />

<img width="945" height="341" alt="image" src="https://github.com/user-attachments/assets/f600d035-893c-4e4d-a2c6-4bfb8cbda7d8" />

**Process activity**
Monitoring the process activity revealed that process ID 4776 was involved in Discovery tactics, using the following techniques: 
- T012 --> Query registry
- T082 --> System information discovery 

<img width="945" height="455" alt="image" src="https://github.com/user-attachments/assets/930808a1-9601-4fba-9016-d4b7ca6b772e" />

<img width="945" height="447" alt="image" src="https://github.com/user-attachments/assets/830feeff-6390-40ac-87a2-b1697d52161f" />

Further investigation within an isolated virtual machine indicates that the URL mimics legitimate online game websites. Analysis of the web page indicates that before any mods installation the following steps are required:
- Disable the antivirus
- Close the game to run the installer
- Run with administrator privilege
All these behaviors are suspicious and strong evidence of phishing campaign.Also, the URL behaves differently when analyzed with sandbox environments, confirming the use of evasion techniques to evade detection.

<img width="983" height="503" alt="image" src="https://github.com/user-attachments/assets/73432712-738b-419d-9aea-6a1f030de3ae" />

<img width="983" height="503" alt="image" src="https://github.com/user-attachments/assets/827d1fd4-23c0-439d-9cef-01ecdec005a6" />

<img width="983" height="503" alt="image" src="https://github.com/user-attachments/assets/740544e0-84a1-4e74-9a1b-98f74d2dae52" />

**2.3. Techniques identification**

The malicious URL uses the HTTPS technique to evade detection.

**2.4. Risk assessment**

The assessment identified the following threats:
- Phishing
- Potential malware delivery
- Risk level: high

**2.5. Finding**

The analysis of https://Skillwicked.cc/ results in:
- Hidden malicious infrastructure
- Discovery: tactic TA007, techniques T1012 and T1082
- Brute force attempts

**2.6.IOC**

**URLs:**
- https://Skillwicked.cc/
- https://login.live.com/ppsecure/deviceaddcredential.srf

**IPs**
- 188.114.96.3
- 188.114.97.3
- 172.67.209.143
  
**Domain**
- skillwicked.cc
  
# 3. Target 5–6: https://trzr-wallt-io.pages.dev/ , http://trzr-wallt-io.pages.dev 

The target 6 exhibits similar activity patterns and malicious behavior as https://trzr-wallt-io.pages.dev/.

**3.1. VirusTotal investigation**
**3.1.1. Phase1: initial analysis**

The investigation of target https://trzr-wallt-io.pages.dev/ using virustotal indicates that:
- The URL was identified as malicious by 18 of 95 security vendors and associated with phishing attempts. This detection score may increase over time, as additional security vendors analyze and update their assessment.
- The request returned a 200 status code, indicating a successful request.

<img width="945" height="451" alt="image" src="https://github.com/user-attachments/assets/47993607-4409-4a9f-ab36-e767f7f5c6d9" />

**3.1.2. Phase2: Deep Analysis**
Further investigation shows that:
- No DNS requests, HTTP/HTTPS traffic, and process activities were observed.
- The URL was recently created.

<img width="945" height="450" alt="image" src="https://github.com/user-attachments/assets/fd213382-9db7-4ccc-a05f-2e3cd8076376" />

**3.2.Any.run investigation**
**3.2.1. Phase1: Initial analysis**

Initial analysis of https://trzr-wallt-io.pages.dev/  reveals that:
- The site is active, mimics legitimate cryptocurrency websites and appears like hardware that provides maximum security for cryptocurrency.
- The request returned a 200 status code, indicating that the request is successful 

<img width="945" height="450" alt="image" src="https://github.com/user-attachments/assets/ca9a4755-a9ed-4d15-80c1-8306f18aedb6" />

**3.1.2. Phase2: Deep Analysis**
**DNS requests**

Multiple DNS requests were observed. All contacted domains were whitelisted and considered safe, except trzr-wallt-io.pages.dev domain whose reputation is unknown.

<img width="983" height="169" alt="image" src="https://github.com/user-attachments/assets/a6f66e6c-d165-4f2a-b9e6-d8a234823497" />

<img width="989" height="186" alt="image" src="https://github.com/user-attachments/assets/49bf89b7-9f13-45f4-9baf-b8e40eaef7eb" />

**HTTP/HTTPS requests**
Multiple HTTPS requests were observed. All contacted URL appeared legitimate except:
- Repetitive failed POST queries to https://login.live.com/ppsecure/deviceaddcredential.srf  initiated by a process named svchost.com process (ID 5316). These requests indicate unsuccessful attempts to add new device credentials.
- Successful GET queries to https://trzr-wallt-io.pages.dev/ which has unknown reputation.
- Successful query to https://clients2.googleusercontent.com/crx/blobs/AQx-wa5gNjUbZEbhJvWB8rq8EGp-U7V18pcxMMBvAEPLmtMndDMphKwrvuJ5LT_TatNkvmvRK0k-gFR-mTEBKmjLyQHlP-OqJ_PO_yKnLnl5P-LIP-vs0RUl-m5v2pZkKwoAxlKa5d7akqSM9-Uq0gw7UzwwbrrcTXbu/GHBMNNJOOEKPMOECNNNILNNBDLOLHKHI_1_102_1_0.crx

<img width="945" height="170" alt="image" src="https://github.com/user-attachments/assets/546ac38c-11bd-4d19-b5f7-b0dba37bb43f" />

<img width="945" height="158" alt="image" src="https://github.com/user-attachments/assets/fb31a8a5-ec67-4796-b245-5e45916852fd" />

<img width="945" height="83" alt="image" src="https://github.com/user-attachments/assets/ad818fe6-d791-450e-af69-4a3e16cd5da0" />

**Process activity**
Monitoring the process activity revealed that process ID 4324 was involved in Discovery tactics, using the following techniques: 
- T012 --> Query registry
- T082 --> System information discovery 

<img width="945" height="398" alt="image" src="https://github.com/user-attachments/assets/6d507a66-d519-4f18-9120-e833d5c8d6ae" />

<img width="945" height="447" alt="image" src="https://github.com/user-attachments/assets/876a1745-6da0-4689-b9ad-7fceffbe0e0f" />

**3.3. Techniques identification**

The malicious URL uses the following techniques to evade detection 
- Abuse of a trusted hosting platform (Cloudflare)
- HTTPS

**3.4. Risk assessment**

The assessment identified the following threats:
- Phishing 
- Potential credentials thefts 
- Risk level: high

**3.5. Finding**

The analysis of https://trzr-wallt-io.pages.dev/ results in :
- Discovery: tactics TA007 and techniques T1012 and 1082
- Brute force attempts

**3.6.IOC**

**URLs:** 

- https://trzr-wallt-io.pages.dev/ 
- http://trzr-wallt-io.pages.dev/ 
- https://clients2.googleusercontent.com/crx/blobs/AQx-wa5gNjUbZEbhJvWB8rq8EGp-U7V18pcxMMBvAEPLmtMndDMphKwrvuJ5LT_TatNkvmvRK0k-gFR-mTEBKmjLyQHlP-OqJ_PO_yKnLnl5P-LIP-vs0RUl-m5v2pZkKwoAxlKa5d7akqSM9-Uq0gw7UzwwbrrcTXbu/GHBMNNJOOEKPMOECNNNILNNBDLOLHKHI_1_102_1_0.crx
- https://login.live.com/ppsecure/deviceaddcredential.srf

**IPs**

- 172.66.44.172
- 172.66.47.84
- 142.251.14.132

**Domain**
- trzr-wallt-io.pages.dev

# 4. Target 9–10: https://en-uphuuld-walet-us.pages.dev/ , http://en-uphuuld-walet-us.pages.dev 

The target 10 exhibits similar activity patterns and malicious behavior as https://en-uphuuld-walet-us.pages.dev/.

**4.1. VirusTotal investigation**
**4.1.1. Phase1: initial analysis**

The investigation of target https://en-uphuuld-walet-us.pages.dev/ using virustotal indicates that:
- The URL was identified as malicious by 7 of 95 security vendors and associated with phishing attempts. This detection score may increase over time, as additional security vendors analyze and update their assessment.
- The request returned a 200 status code, indicating a successful request.

<img width="945" height="449" alt="image" src="https://github.com/user-attachments/assets/3aa3386d-a9f6-4697-bee7-f4ac0038369b" />

**4.1.2. Phase2:Deep Analysis**

Further investigation shows that:
- No DNS requests, HTTP/HTTPS traffic, and process activities were observed.
- The URL was recently created.

<img width="945" height="449" alt="image" src="https://github.com/user-attachments/assets/4b74896b-9c99-4366-bdab-3f1c29daf352" />

**4.2.Any.run investigation**
**4.2.1. Phase1:Initial analysis**

Initial analysis of as https://en-uphuuld-walet-us.pages.dev/ reveals that:
- The request returned a 451 status code, indicating that the request is unavailable for legal reasons 

<img width="945" height="448" alt="image" src="https://github.com/user-attachments/assets/5c65ec73-51c0-40b1-a5ef-76e3d0d847c3" />

**4.1.2. Phase2:Deep Analysis**

**Connections**
Multiple suspicious connections were observed to the following domains:
- 1a4s4dv-m.ns1pcdn.net
- 9y49n2-m.ns1pcdn.net
- 1xtsor1-m.ns1pcdn.net
- testingcf.jsdelivr.net

<img width="983" height="103" alt="image" src="https://github.com/user-attachments/assets/65e31741-16c6-47e4-8eb0-1d225cae4a0d" />

<img width="983" height="175" alt="image" src="https://github.com/user-attachments/assets/eaedb6cd-6d4f-44fe-9126-3b5c7dd96ae1" />

**DNS requests**
Multiple DNS requests with unknown reputations were observed:
- benchmarks.cdn.compute-pipe.com
- benchmarks.cdn-b.compute-pipe.com
- kgnvry-ns1p.b-cdn.net
- 1a4s4dv-m.ns1pcdn.net 
- 9y49n2-m.ns1pcdn.net
- 1xtvhvx-m.ns1pcdn.net and others

<img width="983" height="411" alt="image" src="https://github.com/user-attachments/assets/97f73830-91e5-44df-8f5a-910c08b5f280" />

<img width="945" height="165" alt="image" src="https://github.com/user-attachments/assets/66caed3a-6e7a-4b19-b587-64c12608044b" />

<img width="945" height="165" alt="image" src="https://github.com/user-attachments/assets/4765b325-cfdc-415a-bc78-51b8c8371ccb" />

<img width="945" height="182" alt="image" src="https://github.com/user-attachments/assets/0797fd19-87e1-4131-b2b7-f172c3cba39b" />


**HTTP/HTTPS requests**

Multiple HTTPS requests were observed. All contacted URL appeared legitimate except:
- Failed GET query to https://en-uphuuld-walet-us.pages.dev/ due to legal reasons. The non standard name of the host and the HTTP code (451) suggest that the request may be part of malicious activity.

<img width="945" height="185" alt="image" src="https://github.com/user-attachments/assets/80ac77b8-3bc0-4b3a-8816-bb3304cf9fa3" />

<img width="945" height="183" alt="image" src="https://github.com/user-attachments/assets/b308bb40-da44-47d6-ad80-6ae9d2bcd8c9" />

<img width="945" height="594" alt="image" src="https://github.com/user-attachments/assets/e798478b-0c15-411d-bcd9-209cee6d7307" />

- Successful GET query to https://performance.radar.cloudflare.com/api/beacon, indicates malicious C2 communication

<img width="945" height="257" alt="image" src="https://github.com/user-attachments/assets/c5de97c3-e138-44ca-a965-a1de48fe83e5" />

<img width="945" height="235" alt="image" src="https://github.com/user-attachments/assets/2c1824ae-702a-4ff3-aa72-1e908cb96c42" />

- Multiple images are fetched from unknown domains, confirming a suspicious activity.

<img width="945" height="188" alt="image" src="https://github.com/user-attachments/assets/3c94c7c4-a50e-40d7-9e8d-4ea413bd63d4" />


**Process activity**

Monitoring the process activity revealed that process ID 6556 was involved in Discovery tactics, using the following techniques: 
- T012 --> Query registry
- T082 --> System information discovery 

<img width="945" height="450" alt="image" src="https://github.com/user-attachments/assets/2f2f4840-f754-45dd-b16d-a1645c1b2ca4" />

<img width="945" height="447" alt="image" src="https://github.com/user-attachments/assets/ef65d3d7-0e97-4873-bf4a-024ab0e88326" />

**4.3. Techniques identification**

The malicious URL uses the following technique to evade detection:
- Abuse of a trusted hosting platform (cloudflare)
- HTTPS

**4.4. Risk assessment**

The assessment identified the following threats:
- Phishing
- Possible credentials theft 
- Potential malware delivery
- Risk level: high

**4.5. Finding**

The analysis of as https://en-uphuuld-walet-us.pages.dev/ results in :
- Discovery: tactics TA007 and techniques T1012 and 1082
- Hidden malicious infrastructure
- C2 communication

**4.6.IOC**

**URLs:**
- https://en-uphuuld-walet-us.pages.dev/
- http://en-uphuuld-walet-us.pages.dev
- https://clients2.googleusercontent.com/crx/blobs/AQx-wa5gNjUbZEbhJvWB8rq8EGp-U7V18pcxMMBvAEPLmtMndDMphKwrvuJ5LT_TatNkvmvRK0k-gFR-mTEBKmjLyQHlP-OqJ_PO_yKnLnl5P-LIP-vs0RUl-m5v2pZkKwoAxlKa5d7akqSM9-Uq0gw7UzwwbrrcTXbu/GHBMNNJOOEKPMOECNNNILNNBDLOLHKHI_1_102_1_0.crx
- https://login.live.com/ppsecure/deviceaddcredential.srf

**IPs**
- 188.114.96.3
- 104.18.30.78
- 172.66.46.248
  
**domains**
- en-uphuuld-walet-us.pages.dev

# 5. Target 19–28: http://185.208.159.132/harm7

The target 19 to target 28 exhibits similar activity patterns and malicious behavior as http://185.208.159.132/harm7.

**5.1. VirusTotal investigation**
**5.1.1. Phase1: initial analysis**

The investigation of target http://185.208.159.132/harm7 using virustotal indicates that:
- 13 of 95 security vendors flagged it as malicious and associated with malware. This detection is not static and may increase over time, as additional security vendors analyses and update their assessment.
- The request returned 404 a code status. This indicates that the request fails and the resource is not available.

<img width="945" height="451" alt="image" src="https://github.com/user-attachments/assets/9d5081a3-90de-4d5b-bbf7-6123c01bea1c" />

**5.1.2. Phase2: Deep Analysis**

Further investigation shows that:
- No DNS requests, HTTP/HTTPS traffic, and process activities were observed.
- The URL was recently created.

<img width="945" height="407" alt="image" src="https://github.com/user-attachments/assets/ad00591d-3781-4358-8583-9ecb4d33860b" />

**5.2.Any.run investigation**
**5.2.1. Phase1: Initial analysis**

Initial analysis of http://185.208.159.132/harm7 reveals that:
- The request returned 404 code status, indicating that the resource is not available.

<img width="945" height="450" alt="image" src="https://github.com/user-attachments/assets/a1d0616e-64fc-4e2c-b7eb-70190e7ccead" />

Investigating the IP using abuselPDB confirms that the IP used is 100% malicious.

<img width="983" height="457" alt="image" src="https://github.com/user-attachments/assets/6cfa7ea1-23ad-4fc1-b934-1f6a07ab492a" />

**5.1.2. Phase2: Deep Analysis**

**HTTP/HTTPS requests**
Multiple HTTPS requests were observed. All contacted URL appeared legitimate except:
- Repetitive Failed queries to https://login.live.com/ppsecure/deviceaddcredential.srf  initiated by a process named svchost.com process (ID 5316). These requests indicate unsuccessful attempts to add binaries to the device.
- Failed query to http://185.208.159.132/harm7 requesting harm7 file text is a malicious activity, indicating attempts to download suspicious code.

<img width="945" height="158" alt="image" src="https://github.com/user-attachments/assets/473dc5a5-26d7-422d-b894-03d394efea9a" />

<img width="945" height="188" alt="image" src="https://github.com/user-attachments/assets/071288c0-2d04-418e-9fdd-25613c3649e9" />

<img width="945" height="581" alt="image" src="https://github.com/user-attachments/assets/155c48f4-8432-483c-8e22-98c73dd95392" />

**Process activity**
Monitoring the process activity revealed that process ID 5400 was involved in Discovery tactics, using the following techniques: 
- T012 --> Query registry
- T082 --> System information discovery 

<img width="945" height="451" alt="image" src="https://github.com/user-attachments/assets/ceb643c1-5d8b-4f0b-97c0-507f3c4710b6" />

<img width="945" height="340" alt="image" src="https://github.com/user-attachments/assets/7f5fe68d-be56-4d21-b4ea-4c7fbd691dfc" />

**5.3. Techniques identification**

The malicious URL uses cloud services technique to evade detection.

**5.4. Risk assessment**

The assessment identified the following threats:
- Phishing 
- Potential payload delivery
- Possible credential thefts
- Risk level: high

**5.5. Finding**
The analysis of as http://185.208.159.132/harm7 results in :
- Discovery: tactics TA007 and techniques T1012 and 1082
- Possible payload delivery
  
**5.6.IOC** 

**URLs:** 
- http://185.208.159.132/harm7
- https://login.live.com/ppsecure/deviceaddcredential.srf

**IPs**
- 185.208.159.132

---
# Conclusion

The analysis identified that examined URLs are malicious and associated with phishing attempts and malware delivery or malicious payload delivery. These indicators should be handled cautiously to prevent compromises.

---

# Recommendations

To mitigate threats caused by malicious URL, defense in depth strategy is recommended:

- URL DISRM/rewrite: a Security Email Gateway feature allows neutralizing links before they reach the user’s inbox making them unclickable.
- URL reputation filters  
- URL sandboxing: analyzing the URL in an isolated, controlled, and safe environment such as any.run or virustotal.
- URL click-time evaluation: allow the inspection of the URL twice and help catch delayed redirections.
- Remote Browser Isolation RBI: protect users by rendering web content in a secure, cloud-based environment.
- Encourage users to hover the link to preview the destination before clicking.
- Multi factor authentication MFA: help add additional verification steps.
- User awareness training: educating people how to recognize phishing URL. 
- DNS skinholing: redirect malicious domain to a controlled server.
- Perform regular threat hunting exercises: train SOC analysts to identify malicious URL patterns using threat hunting tools and manual analysis!
