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

