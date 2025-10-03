CPTS Field Manual - An Obsidian Vault for Penetration Testers
Overview
This repository contains my personal CPTS Field Manual, a structured knowledge base built within Obsidian. It was created during my preparation for the HTB Certified Penetration Testing Specialist (CPTS) exam. Use these notes for reference, but understand they are tailored to my learning process.

The structure is heavily based on the methodologies from the HTB CPTS path and inspired by the blogs by BRM.

Included in the repository is Field-Manual-Empty-File-Structure.7z. I highly recommend you use this empty file structure and build your own notes as you prepare. Actively creating your own manual is far more effective than passively reading someone else's.

⚠️ Disclaimer
This field manual is intended for educational and ethical purposes only. The information contained within should be used to enhance cybersecurity skills and knowledge in a lawful manner. The author is not responsible for any misuse or damage caused by the information and tools mentioned. Always obtain explicit, written permission before conducting any security testing on a system or network.

A best effort has been made to remove any proprietary information from this vault. If you find any content that you believe is proprietary, please raise an issue or contact me directly so it can be promptly reviewed and removed.

A Note on "Imperfect" Notes
This vault is being shared as-is. These notes are personal, messy, and reflect my thought process at the time; some parts may not make sense to you, and that's okay. The goal is to show a realistic example of a functional knowledge base, not a polished, perfect guide.

AI-Generated Content Disclaimer: Towards the end of my studies, I experienced significant burnout. As a result, much of the content in the Active Directory and later Windows sections was generated with the assistance of AI. Please keep this in mind and always verify the information before relying on it.

Key Features
Methodology-Driven: The structure follows the complete penetration testing lifecycle, from Information Gathering to Lateral Movement.

Granular & Actionable: Complex topics are broken down into specific, digestible notes containing commands, checklists, and key concepts.

Visually Organized: An emoji-based system (🟢, 🟣, 🔵, 🟠) helps to quickly identify the category and depth of each topic.

Built for Speed: Designed for rapid information retrieval during a time-sensitive exam or engagement.

Based on Proven Success: The content and structure are heavily inspired by BRM's successful CPTS methodology.

Linkable Knowledge: Leverages Obsidian's powerful backlinking feature to connect related concepts, tools, and vulnerabilities.

Getting Started
Prerequisites
Obsidian: You must have Obsidian installed. It is available for free on Windows, macOS, and Linux.

Installation
You can get the vault in one of two ways:

Option A: Git Clone (Recommended)

This method allows you to easily pull future updates.

git clone https://github.com/yashfren/CPTS-Field-Manual.git

Option B: Download ZIP

Click the green < > Code button on the GitHub repository page.

Select Download ZIP.

Extract the downloaded ZIP file to a location of your choice.

Opening the Vault
Open Obsidian.

In the main vault selection window, click Open folder as vault.

Navigate to and select the cloned or extracted folder.

Obsidian will detect that the vault contains community plugins and ask if you want to enable them. Click "Yes" or "Enable Plugins" to ensure the vault functions as intended.

The vault is now ready to use!

Vault Structure
The manual is organized into six core phases, each representing a key stage of a penetration test:

🟢 1. Information Gathering: Enumeration of services, web applications, and Active Directory.

🟢 2. Pre-Exploitation: Shellcraft, wordlist generation, and mastering core tools.

🟢 3. Exploitation: Gaining an initial foothold via service, web, application, and AD attacks.

🟢 4. Post-Exploitation: Privilege escalation, pillaging, and file transfers on both Linux and Windows.

🟢 5. Lateral Movement: Pivoting, port forwarding, and moving through the network.

🟢 6. NetExec: A dedicated playbook for the powerful NetExec (formerly CME) tool.

Start by exploring the 🟢 0. Progress.md file to get a high-level overview.

Acknowledgements & Inspiration
This project would not have been possible without the incredible blog posts and methodology shared by BRM. His detailed approach to the CPTS was the primary inspiration for the structure and content of this vault.

BRM's Blog: https://www.brunorochamoura.com/posts/field-manual-beta/

License
This project is licensed under the MIT License - see the LICENSE file for details.
