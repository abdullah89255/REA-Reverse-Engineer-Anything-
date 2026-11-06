# REA-Reverse-Engineer-Anything-
### REA (Reverse Engineer Anything) — সম্পূর্ণ ব্যবহারবিধি

REA হলো একটি reverse-engineering টুল, যা AI coding agent-এর সাহায্যে কোনো অ্যাপ, JavaScript/Electron প্রোগ্রাম বা native binary কীভাবে কাজ করে তা বিশ্লেষণ করতে সাহায্য করে। এর GitHub repository: [morluto/rea](https://github.com/morluto/rea?utm_source=chatgpt.com)।

আপনি যেহেতু Kali Linux ও cybersecurity tools নিয়ে কাজ করেন, REA আপনার কাজে লাগতে পারে—বিশেষ করে কোনো অ্যাপের গঠন বোঝা, code analysis, function ও module-এর সম্পর্ক খুঁজে বের করা এবং অনুমোদিত reverse engineering শেখার ক্ষেত্রে।

![Intro to reverse engineering with Ghidra – Cyberbit](https://images.openai.com/static-rsc-4/pFdp17dabbpcSkKrHnmSY7QVu9eYwYSP6iac_seiVNH537pwiIvZLD-7kA5pjZuN0EUd3ZmO4y7dXSiK1H0irSR1P0VXKQddcMMp5M8jp1MyyfR_diwbgd1fGgsoX-pOO1XMI7xjNV-79LTJVF-7RscUgQrN13LHikZto5MgwEkBxBgSxVKNbsUH6SGMKX_j?purpose=fullsize)

1\. Native binary analysis

Executable বা binary-এর pseudocode, assembly এবং function সম্পর্ক বিশ্লেষণ। এর জন্য উপযুক্ত analysis provider দরকার হতে পারে।

![JavaScript Modules: An Introduction - DEV Community](https://images.openai.com/static-rsc-4/tzsRu3KXFNc9RkD4FPPfKQ3BwLRr5DlVmMG91C84NCi-BXlIKuSeIV-0me-Qhntr_ICXJTpB7x7zAeXInAhhkwEXf2nFFN_3qTO7bxUc4fHZzWOtPA0bGpbIra-5lbpcVwebh7OEM0assuJf2n1zzp3KnGeyKP-7BvAivW19zUf2yBpnIAq3gay3SE_vU_BR?purpose=fullsize)

2\. JavaScript ও Electron analysis

অ্যাপের modules, imports, Electron boundaries এবং code structure পরীক্ষা করা।

![Add MCP Servers to Claude Code with MCP Toolkit | Docker](https://images.openai.com/static-rsc-4/RqfYU9PZtpuan8Zj2-8-0m7L0oegQbZsZhf5aeCW-ig2IwAXn5w47XZ0z61L9O9I6UkwK2Qvw70LfDWVwIiPv5JfhJwMU981RVjoJlMw5FylPv5Mxt1_9_sUucdvZ0YiX7H-07hpsPbxXQTiHKTL8lVBEPsNHejdYcV2nMgaYg19oalTQtc1ajWJRQ0TAV-T?purpose=fullsize)

3\. AI agent integration

MCP-এর মাধ্যমে সমর্থিত coding agent-কে REA-এর analysis tools ব্যবহার করতে দেওয়া।

গুরুত্বপূর্ণ: REA নিজে কোনো magic vulnerability scanner নয়। এটি মূলত reverse engineering ও evidence-based code investigation-এর জন্য।

### 1\. Kali Linux-এ REA ইনস্টল করুন

REA-এর জন্য Node.js-এর সমর্থিত সংস্করণ হলো 22.x (কমপক্ষে 22.19), 24.x (কমপক্ষে 24.11), অথবা 26+।

### ধাপ ১ — প্রয়োজনীয় সফটওয়্যার পরীক্ষা

টার্মিনালে চালান:

```bash
node --version
npm --version
git --version
```

Node.js বা npm না থাকলে:

```bash
sudo apt update
sudo apt install -y nodejs npm git curl
```

এরপর আবার `node --version` চালিয়ে সংস্করণ পরীক্ষা করুন। Kali-র repository থেকে ইনস্টল করা Node.js সমর্থিত সংস্করণের চেয়ে পুরোনো হলে, Node.js-এর অফিসিয়াল ইনস্টলেশন নির্দেশনা অনুসরণ করে উপযুক্ত সংস্করণ ইনস্টল করুন।

### ধাপ ২ — REA-এর agent setup চালান

```bash
npx rea-agents@latest setup
```

এরপর:

* আপনার ব্যবহৃত coding agent নির্বাচন করুন।

* কোন configuration পরিবর্তন হবে তা পর্যালোচনা করুন।

* প্রয়োজনীয় পরিবর্তন অনুমোদন করুন।

* সেটআপ শেষে সংশ্লিষ্ট agent বন্ধ করে আবার চালু করুন।

REA setup করতে পারলেও সব analysis provider স্বয়ংক্রিয়ভাবে ইনস্টল হয় না।

### ধাপ ৩ — REA CLI ইনস্টল করুন

```bash
npm install --global rea-agents
```

ইনস্টলেশন যাচাই:

```bash
rea --help
rea doctor
```

প্রয়োজনে JSON রিপোর্ট:

```bash
rea doctor --json
```

### 2\. REA-এর গুরুত্বপূর্ণ কমান্ডসমূহ

নিচের কমান্ডগুলো তাদের কাজ অনুযায়ী সাজানো হয়েছে। মনে রাখবেন, `rea --help` এবং `rea doctor --json` দিয়ে শুরু করাই সবচেয়ে ভালো; প্রতিটি analysis command-এর সুনির্দিষ্ট argument আপনার ইনস্টল করা সংস্করণের CLI documentation-এর সঙ্গে মিলিয়ে নেবেন।

কমান্ড কপি করতে ডান পাশের Copy আইকন ব্যবহার করুন, যদি আপনার অ্যাপে সেটি দেখা যায়।

### A. Setup ও system status

| কমান্ড                     | কাজ                                                      |
| -------------------------- | -------------------------------------------------------- |
| rea --help                 | প্রধান help ও কমান্ডের তালিকা                            |
| rea doctor                 | পরিবেশ, agent configuration ও provider readiness পরীক্ষা |
| rea doctor --json          | machine-readable diagnostic রিপোর্ট                      |
| rea providers --json       | analysis provider-এর তথ্য                                |
| rea setup                  | agent integration setup                                  |
| rea setup --dry-run        | পরিবর্তন না করে setup পরিকল্পনা দেখা                     |
| rea update                 | global npm/Bun installation আপডেট                        |
| rea uninstall              | REA-এর পরিচালিত integration ও skill সরানো                |
| rea uninstall --purge-data | integration সরানোর পাশাপাশি REA-এর cache/state মুছে ফেলা |

সতর্কতা: `rea uninstall --purge-data` ব্যবহার করার আগে প্রয়োজনীয় REA data সংরক্ষণ করুন।

### B. নির্দিষ্ট coding agent-এর জন্য setup

Codex-এর উদাহরণ:

```bash
rea setup --client codex
rea doctor --client codex --json
```

পরিবর্তনের পরিকল্পনা আগে দেখতে:

```bash
rea setup --client codex --dry-run --json
```

Cursor-এর জন্য:

```bash
rea setup --client cursor
rea doctor --client cursor --json
```

Claude Code-এর জন্য:

```bash
rea setup --client claude_code
rea doctor --client claude_code --json
```

আপনার কম্পিউটারে যে agent ব্যবহার করেন, শুধু সেটির জন্য setup করাই ভালো। `--dry-run` হলো পরিবর্তনের পরিকল্পনা দেখার পদ্ধতি; এটি বাস্তবে integration সম্পন্ন করে না।

### C. JavaScript/Electron অ্যাপ বিশ্লেষণ

ধরা যাক, আপনার নিজের পরীক্ষামূলক অ্যাপের ফাইল আছে `/home/kali/lab/myapp`-এ।

```bash
npx -y rea-agents@latest analyze-javascript-application /home/kali/lab/myapp --json
```

এই কমান্ডের ফলাফলে পাওয়া যেতে পারে:

* অ্যাপের module ও file structure

* কোন module কোন module import করে

* Electron-related boundaries

* বিশ্লেষণের পক্ষে প্রমাণ ও সীমাবদ্ধতা

আপনার অ্যাপের প্রকৃত path বসাবেন। Extract করা JavaScript/Electron application directory বা সমর্থিত ASAR target ব্যবহার করুন।

Windows-এর উদাহরণ:

```powershell
npx -y rea-agents@latest analyze-javascript-application "D:\apps\myapp" --json
```

### D. GitHub repository ডাউনলোড করে দেখা

```bash
git clone https://github.com/morluto/rea.git
cd rea
git status
git log -5 --oneline
```

ফাইলের তালিকা:

```bash
ls -la
find docs -maxdepth 2 -type f
```

এই কমান্ডগুলো repository-এর ফাইল ও ইতিহাস দেখতে সাহায্য করে। এগুলো চালানো মানেই REA ইনস্টল বা সেটআপ সম্পন্ন হওয়া নয়।

### E. REA আপডেট

Global installation থাকলে:

```bash
rea update
```

সেটআপের সর্বশেষ সংস্করণ দিয়ে agent registration আপডেট করতে:

```bash
npx rea-agents@latest setup
```

পরিবর্তনগুলো পর্যালোচনা করে অনুমোদন দিন এবং সংশ্লিষ্ট agent পুনরায় চালু করুন।

### 3\. Native binary reverse engineering

REA-এর native analysis-এর জন্য Ghidra, IDA অথবা Hopper-এর মতো provider প্রয়োজন হতে পারে। Kali Linux-এ Ghidra ব্যবহার করতে চাইলে প্রথমে আপনার বিদ্যমান Ghidra installation-এর অবস্থান খুঁজে নিন।

```bash
find "$HOME" /opt -maxdepth 4 -type f -name ghidraRun 2>/dev/null
```

পাওয়া installation directory অনুযায়ী পরিবেশ সেট করুন। উদাহরণস্বরূপ:

```bash
export GHIDRA_INSTALL_DIR="/opt/ghidra"
rea doctor --provider ghidra --json
rea providers --json
```

`/opt/ghidra` কেবল উদাহরণ; আপনার আসল installation path বসাতে হবে। প্রয়োজন হলে Java/JDK-ও কনফিগার করতে হবে।

Native analysis-এর জন্য এরপর আপনার ইনস্টল করা সংস্করণে উপলভ্য command ও argument দেখতে:

```bash
rea --help
rea doctor --json
```

আমি এখানে যাচাই না করে binary analysis-এর কোনো নির্দিষ্ট subcommand বা flag বানিয়ে দিচ্ছি না। REA-এর [CLI ও Evidence guide](https://github.com/morluto/rea?utm_source=chatgpt.com) থেকে সংশ্লিষ্ট নির্দেশনা মিলিয়ে নিন।

### 4\. বাস্তব উদাহরণ: নিজের অ্যাপ বিশ্লেষণ

প্রথমে readiness পরীক্ষা

```bash
rea doctor --json
```

নিজের অ্যাপের path ঠিক করুন

```bash
ls -la /home/kali/lab/myapp
```

JavaScript/Electron analysis চালান

```bash
npx -y rea-agents@latest analyze-javascript-application /home/kali/lab/myapp --json
```

ফলাফল পর্যালোচনা করুন

module, imports, Electron boundaries এবং returned evidence দেখুন। বিশ্লেষণের ফলাফল থেকে সরাসরি vulnerability নিশ্চিত হয়েছে—এমন সিদ্ধান্ত নেবেন না।

### 5\. আপনার জন্য কোন নির্দেশনাটি সবচেয়ে উপযুক্ত?

আপনি REA কোথায় ও কী কাজে ব্যবহার করবেন?

এগুলো নির্বাচন করলে আপনার পরিবেশ অনুযায়ী ইনস্টলেশন ও কমান্ড সাজিয়ে দিতে পারব।

আপনার operating system

Kali Linux

Ubuntu / Debian

Windows 10/11

Windows + WSL

আপনার প্রধান লক্ষ্য

Bug bounty ও অনুমোদিত security research

JavaScript / Electron app analysis

EXE, ELF বা native binary reverse engineering

REA-এর সব CLI command শেখা

আপনি কোন AI coding agent ব্যবহার করেন?

Codex

Claude Code

Cursor

কোনো coding agent নেই; শুধু terminal

আমার জন্য কমান্ড সাজান

অফিশিয়াল রেফারেন্স: [REA GitHub repository](https://github.com/morluto/rea?utm_source=chatgpt.com) · [Installation ও setup guide](https://github.com/morluto/rea/blob/main/docs/installation.md?utm_source=chatgpt.com) · [REA npm package](https://www.npmjs.com/package/rea-agents?utm_source=chatgpt.com)

### Kali Linux-এ REA: Bug Bounty ব্যবহারবিধি

আপনার ক্ষেত্রে সবচেয়ে গুরুত্বপূর্ণ বিষয় হলো: আপনার কাছে এখন কোনো AI coding agent নেই। তাই প্রথমে REA-এর standalone CLI ব্যবহার শিখব। পরে চাইলে MCP-সমর্থিত AI agent সংযুক্ত করবেন।

REA মূলত reverse engineering, application structure analysis, binary inspection ও evidence সংগ্রহের টুল। এটি সাধারণ bug-bounty scanner-এর বিকল্প নয়; ওয়েবসাইটে XSS, IDOR, SSRF বা SQL injection স্বয়ংক্রিয়ভাবে খুঁজে দেওয়ার নিশ্চয়তা দেয় না।

### ১. REA কী কী কাজে লাগবে?

![Gemini Became My Entire Hackathon Team — How a Solo Dev in Kenya Won His First MLH Prize Building RepoX - DEV Community](https://images.openai.com/static-rsc-4/9fooF8D7u9fcb64QKHwbwnoIhii2VaNXgDlsu25cBHsqfF6N2oZei1CDrvZAFIgGiMmccPz0jzhOof3Z4qbP5juv72E3TgmtR7q5mz3Pl_4NHMt8tcz1CSk1VKJRSnD7gIMPMGoe63g7mFMwHu_sJJyU1bLuglDbfES85QLoLeFe3W3DZzEEo6Rm_VghVXEw?purpose=fullsize)

JavaScript / Electron analysis

অ্যাপের module, imports, dependency সম্পর্ক ও Electron-এর অংশগুলো বোঝা।

![](https://images.openai.com/static-rsc-4/3uyC-S6jU1dZchM2ZkWkCtKmM16TZfptCv39PR1uCvn9a6qNfqQUs9pB5LJpYbNKyVatNM6nlgcMLE8C3OjpEQ-xKIDCz0ZKyQAf8Ka5D4R-kEJsXzAWRMeNge0Fk6ilBIUMpFcyYfJVRFqaNWLhafLvomuMqWtD-9NjXEHhVZCf-IdUhJYHV4Iv3aJTwqD0?purpose=fullsize)

Native binary analysis

Ghidra বা অন্য সমর্থিত provider ব্যবহার করে binary, function, pseudocode ও cross-reference পরীক্ষা করা।

![How to Fingerprint Websites With WhatWeb - A Practical, Hands-On Guide](https://images.openai.com/static-rsc-4/eRv1RFicvKv7BwvhkVRRi4qd_FvUnvl1twQuG8h0EUaQ-lKaM-bl06IsZosRLGwUNFhA7s_u1Lx6tVupfEAKka_yrqdJ8-HgFpLFA5ZVy-R1muSz5L7ttZeB5hDna_suQNPxHBqmEP6eP6GNcyLDBq4hoQhh57hj3x_Pu0um9olMK3YLCJTcOVTJULU4X_F1?purpose=fullsize)

CLI ও JSON evidence

টার্মিনাল থেকে বিশ্লেষণ চালানো, JSON রিপোর্ট সংরক্ষণ এবং পরবর্তী পরীক্ষার জন্য ফলাফল ব্যবহার করা।

অফিশিয়াল নির্দেশিকা: [REA GitHub](https://github.com/morluto/rea?utm_source=chatgpt.com) · [CLI ও Evidence guide](https://github.com/morluto/rea/blob/main/docs/cli.md?utm_source=chatgpt.com) · [Installation guide](https://github.com/morluto/rea/blob/main/docs/installation.md?utm_source=chatgpt.com)।

### ২. ইনস্টলেশনের আগে prerequisites

টার্মিনালে প্রথমে এগুলো চালান:

```bash
node --version
npm --version
git --version
ps --version
```

REA-এর বর্তমান Linux নির্দেশিকা অনুযায়ী, Node.js/npm এবং Linux-এ কাজ করা `ps` কমান্ড গুরুত্বপূর্ণ। `ps`-এর জন্য Kali-তে `procps` প্যাকেজ ব্যবহার করুন।

প্রয়োজনীয় প্যাকেজ ইনস্টল করুন:

```bash
sudo apt update
sudo apt install -y nodejs npm git procps curl jq
```

Node.js-এর সংস্করণ পরীক্ষা করুন:

```bash
node -p 'process.version'
npm view rea-agents dist-tags.latest
```

Node.js পুরোনো হলে REA-এর বর্তমান সমর্থিত সংস্করণের সঙ্গে মিলিয়ে আপডেট করুন। বর্তমান installation guide-এ runtime requirements দেখুন: [REA runtime requirements](https://github.com/morluto/rea/blob/main/docs/installation.md?utm_source=chatgpt.com)।

### ৫. বাস্তব Bug Bounty workflow

REA ব্যবহার করে একটি অনুমোদিত JavaScript/Electron অ্যাপের structure বিশ্লেষণের উদাহরণ নিচে দেওয়া হলো।

```bash

# 1. Lab directory তৈরি
mkdir -p ~/rea-lab/{targets,reports}
cd ~/rea-lab

# 2. নিজের বা অনুমোদিত target-এর files রাখুন
ls -la ~/rea-lab/targets/

# 3. REA-এর readiness যাচাই
rea doctor --json > reports/doctor.json

# 4. JavaScript application বিশ্লেষণ
rea analyze-javascript-application \
  "$HOME/rea-lab/targets/myapp" \
  --json > reports/app-analysis.json

# 5. রিপোর্টের কাঠামো দেখুন
jq 'keys' reports/app-analysis.json | less

# 6. রিপোর্টে কী আছে, তা পড়ুন
less reports/app-analysis.json
```

`myapp`-এর জায়গায় আপনার বাস্তব target directory ব্যবহার করবেন। এই নির্দিষ্ট workflow extracted JavaScript/Electron application directory বা সমর্থিত ASAR-এর জন্য; সাধারণ live website URL-এর জন্য নয়।

### রিপোর্ট পাওয়ার পর কী করবেন?

* Module ও imports: authentication, API client, routing এবং data handling-সম্পর্কিত code কোথায় আছে তা চিহ্নিত করুন।

* Source review: token handling, authorization checks, sensitive data exposure এবং unsafe input handling খুঁজুন।

* Manual verification: সম্ভাব্য সমস্যা সংশ্লিষ্ট অ্যাপের অনুমোদিত test environment-এ যাচাই করুন।

* Evidence সংগ্রহ: প্রভাব, পুনরুৎপাদনের ধাপ এবং প্রাসঙ্গিক request/response সংরক্ষণ করুন।

* Report: শুধুমাত্র প্রমাণিত finding জমা দিন; কোনো সন্দেহজনক code pattern-কে একাই confirmed vulnerability বলবেন না।

লাইভ ওয়েবসাইটের bug bounty-তে REA-এর পাশাপাশি HTTP traffic inspection, browser developer tools এবং scope অনুযায়ী উপযুক্ত web security testing tools প্রয়োজন হতে পারে।

### ৬. Ghidra দিয়ে native binary বিশ্লেষণ

যদি আপনার লক্ষ্য Android app-এর native library বা Linux executable হয়, Ghidra ব্যবহার করতে পারেন।

প্রথমে Kali-তে Ghidra আছে কি না পরীক্ষা করুন:

```bash
command -v ghidra
find /opt "$HOME" -maxdepth 4 -name ghidraRun 2>/dev/null
```

তারপর REA-তে Ghidra provider readiness পরীক্ষা করুন:

```bash
rea doctor --provider ghidra --json
rea providers --json
```

REA-এর official Ghidra guide অনুসরণ করে installation path ও provider configuration সম্পন্ন করুন। এরপর:

```bash
rea analyze /path/to/authorized-binary \
  --provider ghidra --json

rea search /path/to/authorized-binary "password" \
  --provider ghidra --json
```

এখানে `password` কেবল অনুসন্ধানের উদাহরণ। আপনার পরীক্ষার লক্ষ্য অনুযায়ী প্রাসঙ্গিক string বা symbol ব্যবহার করুন। Native analysis-এর জন্য প্রয়োজনীয় provider প্রস্তুত না থাকলে কমান্ড ব্যর্থ হতে পারে।

### ৭. Troubleshooting

| সমস্যা                        | করণীয়                                                                                                           |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------- |
| rea: command not found        | npm root -g, npm config get prefix ও command -v rea পরীক্ষা করুন।                                               |
| Node.js/npm version সমস্যা    | REA installation guide অনুযায়ী supported Node.js runtime ব্যবহার করুন।                                          |
| ps বা process ownership error | sudo apt install procps চালান; তারপর ps --version পরীক্ষা করুন।                                                 |
| provider_unavailable          | rea providers --json এবং rea doctor --provider ghidra --json চালান।                                             |
| capability_unavailable        | target type ও provider compatibility পরীক্ষা করুন।                                                              |
| JSON output অনেক বড়           | output ফাইলে redirect করুন, তারপর jq দিয়ে প্রয়োজনীয় অংশ দেখুন।                                                  |
| নতুন সংস্করণে সমস্যা          | global installation হলে rea update; package runner হলে npx rea-agents@latest দিয়ে সংশ্লিষ্ট command আবার চালান। |

Linux-এর `ps` prerequisite এবং provider troubleshooting REA-এর official installation documentation-এ বর্ণিত আছে।

### ৮. আপনার জন্য প্রস্তাবিত পরবর্তী পদক্ষেপ

আপনি যেহেতু Kali Linux-এ bug bounty শিখছেন, প্রথমে এই তিনটি কমান্ড চালিয়ে ফলাফল দেখুন:

```bash
node --version
npx -y rea-agents@latest --help
npx -y rea-agents@latest doctor --json
```

তারপর নিচের কোন target নিয়ে কাজ করবেন সেটি নির্বাচন করুন।

আপনার প্রথম analysis target

JavaScript / Electron app

Android APK / native library

Linux ELF binary

অনুমোদিত bug-bounty website ও JavaScript bundles

ইনস্টলেশন পছন্দ

শুধু terminal CLI (এখনই)

CLI + পরে AI coding agent সংযুক্ত করব

পরবর্তী ধাপ দেখান

মনে রাখবেন: REA-এর সব analysis feature চালাতে Ghidra বা AI agent আবশ্যক নয়। কোনটি প্রয়োজন হবে তা নির্ভর করে আপনি JavaScript application, native binary, নাকি website পরীক্ষা করছেন তার ওপর।
# Complete REA + Ghidra + Claude Code Setup on Parrot OS



Step-by-step installation guide

Reverse engineering lab



আপনার লক্ষ্য হলো এমন একটি integrated reverse-engineering environment তৈরি করা, যেখানে:

- REA বিভিন্ন file format ও artifact analyze করবে।
- Ghidra native executable-এর disassembly ও decompilation করবে।
- Claude Code AI agent হিসেবে REA-এর analysis tools ব্যবহার করবে।
- MCP REA এবং Claude Code-এর মধ্যে communication তৈরি করবে।
- Python automation এবং additional analysis-এর কাজে ব্যবহার হবে।

আমরা প্রথমে installation, তারপর integration, এরপর testing এবং শেষে বাস্তব analysis exercise করব।

Final architecture

Claude Code

AI investigation and reasoning

MCP connection

REA

Analysis tools, evidence and artifact inspection

Ghidra

Native binary analysis and decompilation

## Phase 1 — Prepare Parrot OS

### Step 1. Update the system

Open your Parrot OS terminal:

```
sudo apt update

sudo apt full-upgrade -y
```

Install dependencies:

```
sudo apt install -y \
  git curl wget unzip \
  build-essential \
  python3 python3-pip python3-venv \
  procps file jq
```

Check system architecture:

```
uname -m
```

Expected output for a typical 64-bit installation:

```
x86_64
```

### Step 2. Install Java

Ghidra requires a compatible JDK.

```
sudo apt install -y openjdk-21-jdk
```

Check:

```
java -version
javac -version
```

Expected:

```
openjdk version "21..."
```

If Parrot's repositories do not provide OpenJDK 21, use a compatible JDK distribution rather than installing an incompatible Java version.

### Step 3. Install Node.js

REA and Claude Code require Node.js-based tooling.

Check your existing version:

```
node -v
npm -v
```

For REA, use a Node.js release supported by the repository's current requirements. Node.js 22 or newer is a suitable starting point, subject to its declared engine requirement.

If Node.js is missing, install it using the official instructions:

[Node.js official downloads](https://nodejs.org/en/download?utm_source=chatgpt.com)

Then verify:

```
node --version
npm --version
```

## Phase 2 — Install Ghidra

### Step 4. Download Ghidra

[Install Ghidra on Linux | Flathub](https://images.openai.com/static-rsc-4/Eu5GrMOGCTB2eaUfJQr5Yg4dGU0F0PRPmHxjunwUAZeCpCae8IK2aH9jc33mAfQf9U29YCd80mXUrZLKGnrxavQ97X8gaVLnGsPKQN0msHZtRudrN9kdVnXxGG7WVseGcgqDpzQONw1IvNfclGA1iMouxqunqMGHLTW-RE7K4UU?purpose=inline)

Ghidra

Use the 12.1.x release supported by REA's current documentation.

[Official Ghidra releases](https://github.com/NationalSecurityAgency/ghidra/releases?utm_source=chatgpt.com)

REA's current Ghidra documentation specifies:

- Ghidra 12.1.x
- 64-bit JDK 21 or newer
- `analyzeHeadless` executable
- Native decompiler for your architecture

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub

Download and extract the official release archive. For example, if the archive is in Downloads:

```
mkdir -p ~/Applications

cd ~/Applications

unzip ~/Downloads/ghidra_12.1.4_PUBLIC_*.zip
```

Check the extracted directory:

```
ls ~/Applications/
```

Example:

```
ghidra_12.1.4_PUBLIC
```

### Step 5. Configure Ghidra environment variables

Run:

```
nano ~/.zshrc
```

Add:

```
# Java configuration
export JAVA_HOME="/usr/lib/jvm/java-21-openjdk-amd64"

# Ghidra configuration
export GHIDRA_INSTALL_DIR="$HOME/Applications/ghidra_12.1.4_PUBLIC"

# Java PATH
export PATH="$JAVA_HOME/bin:$GHIDRA_INSTALL_DIR/support:$PATH"

# Ghidra memory
export GHIDRA_HEADLESS_MAXMEM="2G"
```

Save with `CTRL + O`, Enter, and `CTRL + X`.

Reload:

```
source ~/.zshrc
```

Verify:

```
echo "$JAVA_HOME"

echo "$GHIDRA_INSTALL_DIR"

ls "$GHIDRA_INSTALL_DIR/support/analyzeHeadless"
```

Also check the native decompiler:

```
find "$GHIDRA_INSTALL_DIR/Ghidra/Features/Decompiler" \
  -type f -name decompile
```

### Step 6. Test Ghidra independently

Launch the graphical interface:

```
"$GHIDRA_INSTALL_DIR/ghidraRun"
```

Then test the headless analyzer:

```
"$GHIDRA_INSTALL_DIR/support/analyzeHeadless" \
  --help
```



Expected

&#x20;Ghidra should start without a Java version or missing-decompiler error.



## Phase 3 — Install Claude Code

### Step 7. Install Claude Code

Official installation method:

```
npm install -g @anthropic-ai/claude-code
```

Do not use `sudo npm install -g`.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://docs.anthropic.com\&sz=32)

Anthropic



Check:

```
claude --version

claude doctor
```

Start Claude Code:

```
claude
```

Follow the authentication process using your eligible Claude account or Anthropic Console.

Official documentation: [Claude Code installation and authentication](https://docs.anthropic.com/en/docs/claude-code/getting-started?utm_source=chatgpt.com).

### Step 8. Verify Claude Code

```
claude --help
```

Then:

```
claude mcp
```

This opens the MCP management interface. We will use REA's guided setup instead of manually creating an MCP configuration.

## Phase 4 — Install REA and connect Claude Code

[ShadowArcanist](https://images.openai.com/static-rsc-4/W75sXYpRjTR-r4rEpXW1AkL-5z2QJ9BLjfpnJkZ_g6x2u_yIi7l5de3PAVEOpYd0xCgvtf_tMTSpdo5Et3JZSQ69RaRV4EFvgGd5c5AzLkzhA5Qsm5F9hwPpIZPPR9XF4c4hcg0-4I679yCRHttxOV0MNLsttG3KvuLy8U9JLNU?purpose=inline)

REA – Reverse Engineer Anything

Official repository and npm distribution

[GitHub repository](https://github.com/morluto/rea?utm_source=chatgpt.com) · [npm package](https://www.npmjs.com/package/rea-agents?utm_source=chatgpt.com)

### Step 9. Install REA

I recommend a global installation because you will use REA frequently from your terminal.

```
npm install --global rea-agents
```

Verify:

```
rea --help
```

### Step 10. Verify Ghidra before connecting

Run:

```
rea doctor --json
```

For Ghidra-specific diagnostics, run:

```
rea doctor --provider ghidra --json
```

If you see missing Ghidra or Java prerequisites, correct those first.

### Step 11. Connect REA to Claude Code

Run:

```
rea setup --client claude_code
```

The setup process should:

1. Detect Claude Code.
2. Detect your Ghidra installation.
3. Inspect the proposed MCP registration.
4. Configure REA for Claude Code.
5. Install REA's matching workflow instructions.
6. Ask for your approval before applying changes.

Carefully review the displayed configuration paths and environment variables before approving.

REA's setup configures the agent; it does not install Ghidra or Java.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



### Step 12. Verify the integration

Run:

```
rea doctor --json
```

Then:

```
rea providers --json
```

And:

```
rea capabilities --json
```

Check that the diagnostics report a supported Ghidra installation and that Claude Code has an REA registration.

Restart Claude Code completely:

```
exit
```

Then:

```
claude
```

Inside Claude Code, run:

```
/mcp
```

You should see the REA server and its available tools.



Integration target

&#x20;Claude Code → REA MCP → Ghidra.



Important: REA uses lazy Ghidra startup. Seeing the MCP connection does not prove Ghidra analysis works; the first native analysis request must also succeed.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



## Phase 5 — Test REA + Ghidra from terminal

### Step 13. Create a safe test binary

Instead of testing with an unknown executable, create a small C program.

```
mkdir -p ~/REA-Lab
cd ~/REA-Lab
```

Create the source file:

```
nano test.c
```

Paste:

```
#include <stdio.h>
#include <string.h>

int calculate(int a, int b) {
    return a + b;
}

int main() {
    char username[32];

    strcpy(username, "Mamun");

    printf("Welcome %s\n", username);

    int result = calculate(10, 20);

    printf("Result: %d\n", result);

    return 0;
}
```

Compile:

```
gcc -g -O0 -fno-pie -no-pie test.c -o test
```

Check:

```
file test
```

Test binary created

### Step 14. Run REA analysis

```
rea analyze "$HOME/REA-Lab/test" \
  --provider ghidra \
  --json
```

This should initiate Ghidra-backed native analysis.

### Step 15. Search for functions

```
rea search "$HOME/REA-Lab/test" "main" \
  --provider ghidra \
  --json
```

### Step 16. Generate function dossier

```
rea function "$HOME/REA-Lab/test" main \
  --provider ghidra \
  --json
```

Look for:

- Function name
- Function address
- Assembly instructions
- Call relationships
- References
- Pseudocode evidence

### Step 17. Decompile a function

```
rea decompile "$HOME/REA-Lab/test" \
  --procedure main \
  --provider ghidra \
  --json
```

### Step 18. Cross-reference analysis

First identify an address from your analysis results, then:

```
rea xrefs "$HOME/REA-Lab/test" 0xADDRESS \
  --provider ghidra \
  --json
```

Replace `0xADDRESS` with an actual address from your binary.

### Step 19. Trace code

```
rea trace "$HOME/REA-Lab/test" "calculate" \
  --provider ghidra \
  --json
```

These commands are documented in REA's CLI guide.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub



## Phase 6 — Test AI analysis using Claude Code

### Step 20. Open your analysis directory

```
cd ~/REA-Lab

claude
```

Check the MCP connection:

```
/mcp
```

### Step 21. Run your first AI reverse-engineering investigation

Copy this prompt into Claude Code:

I am performing an authorized reverse-engineering analysis of the test binary located at:

\~/REA-Lab/test

Use the connected REA MCP server and Ghidra provider.

First:

1. Inspect the available REA MCP tools.
2. Check which analysis providers are available.
3. Open the binary using Ghidra.
4. Wait for analysis to complete.

Then perform:

1. Binary metadata analysis.
2. Function inventory.
3. Entry-point identification.
4. Function-by-function analysis.
5. Assembly and pseudocode inspection.
6. String analysis.
7. Imported function analysis.
8. Caller and callee analysis.
9. Cross-reference investigation.
10. Static security weakness investigation.

Pay particular attention to:

- strcpy()
- Buffer sizes
- Input validation
- Memory operations
- Function arguments
- Return values

For every potential weakness:

- Provide function name and address.
- Show supporting pseudocode.
- Explain the root cause.
- Distinguish confirmed issues from theoretical risks.
- Recommend remediation.

Do not execute the target binary.\
Do not modify the executable.\
Preserve the analysis evidence.

Save the final report as:

\~/REA-Lab/REA-Analysis-Report.md

### Step 22. Ask Claude to investigate individual functions

Try these prompts individually:

Function analysis

```
Use REA and Ghidra to explain every important
function in this binary.

Explain the function purpose, parameters,
return values and relationships.
```

Memory analysis

```
Investigate all memory-related operations.

Identify unsafe memory functions,
buffer sizes and possible boundary issues.

Provide evidence from Ghidra pseudocode.
```

Call graph analysis

```
Build a call graph of the important functions.

Identify:
- Entry points
- Callers
- Callees
- Shared functions
- Important execution paths

Explain the relationships.
```

Security investigation

```
Perform a static security review.

Investigate:
- Buffer overflow indicators
- Integer overflow indicators
- Format string risks
- Unsafe memory operations
- Hardcoded secrets
- Authentication weaknesses

Provide evidence and confidence levels.
Do not exploit anything.
```

## Phase 7 — Enable additional REA features

For a broader REA environment, use the following checklist.

Integration checklist

0/10 completed

Node.js and npm installed

Ghidra 12.1.x installed

JDK 21+ configured

Ghidra native decompiler verified

Claude Code authenticated

REA CLI installed

REA MCP connected to Claude Code

Ghidra provider passes doctor

Native test binary analyzed successfully

Claude generated an evidence-based report

### Other REA capabilities

Once the core integration works, investigate these additional features:

| Feature             | Example use                             |
| ------------------- | --------------------------------------- |
| JavaScript analysis | Inspect application modules and imports |
| Electron analysis   | Investigate IPC boundaries              |
| APK analysis        | Inspect Android application structure   |
| .NET analysis       | Examine assemblies and methods          |
| Firmware analysis   | Inspect firmware components             |
| ELF analysis        | Inspect Linux executable structure      |
| PE analysis         | Inspect Windows executable structure    |
| PCAP/HAR            | Investigate captured network behavior   |
| Crash analysis      | Examine crash metadata                  |
| Python bytecode     | Inspect `.pyc` files                    |

These do not all require Ghidra. Their individual dependencies and supported operations are described in the [REA documentation](https://github.com/morluto/rea/tree/main/docs?utm_source=chatgpt.com).&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub



## Phase 8 — Troubleshooting

| Problem                   | What to check                        |
| ------------------------- | ------------------------------------ |
| `rea: command not found`  | npm global PATH                      |
| Ghidra unavailable        | `GHIDRA_INSTALL_DIR`                 |
| Java version error        | JDK 21+ and `JAVA_HOME`              |
| `analyzeHeadless` missing | Ghidra extraction directory          |
| Decompiler unavailable    | Native decompiler architecture       |
| MCP server missing        | `rea setup --client claude_code`     |
| Claude cannot see tools   | Restart Claude Code and check `/mcp` |
| Ghidra startup timeout    | Increase REA startup timeout         |
| Linux process errors      | Check `procps` and `ps`              |

Useful diagnostic commands:

```
rea doctor --provider ghidra --json
```

```
rea doctor --client claude_code --json
```

```
rea doctor --skill --json
```

```
rea providers --json
```

If you have a small-memory VM, you can reduce Ghidra's Java heap:

```
export GHIDRA_HEADLESS_MAXMEM=512M
```

For large binaries, increase it according to available RAM. REA's default startup deadline is 330 seconds; large imports may need more time.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub+1



## Official reference links

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

REA GitHub

Source code and updates

[Open repository](https://github.com/morluto/rea?utm_source=chatgpt.com)

[image](https://www.google.com/s2/favicons?domain=https://ghidra-sre.org\&sz=32)

Ghidra

Official installation and releases

[Download releases](https://github.com/NationalSecurityAgency/ghidra/releases?utm_source=chatgpt.com)

[image](https://www.google.com/s2/favicons?domain=https://docs.anthropic.com\&sz=32)

Claude Code

Installation and MCP documentation

[Official documentation](https://docs.anthropic.com/en/docs/claude-code?utm_source=chatgpt.com)

[image](https://www.google.com/s2/favicons?domain=https://nodejs.org\&sz=32)

Node.js

Supported runtime

[Download Node.js](https://nodejs.org/en/download?utm_source=chatgpt.com)

One important correction to keep in mind: REA's current repository requires Node.js 22.x (22.23.4 or newer), 24.x (24.22.1 or newer), or 26.12.1+. Check this before installing; merely having Node.js 18 or 20 is not sufficient.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://github.com\&sz=32)

GitHub



## Recommended next step

I can also help you maintain this environment by checking REA releases, Ghidra compatibility and Claude Code integration changes every week.

🔍Check REA updates weeklyTap to set up

My recommendation: Complete Phases 1–4 first. Once `rea doctor --provider ghidra --json` and Claude Code's `/mcp` both pass, proceed to the test binary. This will make troubleshooting much easier.
