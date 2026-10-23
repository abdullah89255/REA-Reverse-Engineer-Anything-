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

