# Leadyol skill for Claude

Find business leads from Claude: by category and city, with website contacts and e-mails, duplicates removed, and an Excel export.

![Leadyol](docs/screenshot.png)

## Open data. Public business information.

Leadyol finds businesses in open, licensed data (Overture Maps), then visits each business's own website for e-mails and social profiles. It is not a Google Maps scraper: it does not open or automate a map website. It sends no messages. It keeps a do-not-contact list and leaves it out of every export. You are responsible for how you use the data (GDPR, CAN-SPAM, KVKK).

## What you need

- The **Leadyol app**, installed and running on your computer (Windows, Mac or Linux). Free 7-day trial: https://leadyol.tr/?ref=skill-github
- An AI assistant that supports skills and MCP servers (Claude Code, Claude Desktop, claude.ai).
- Leadyol's MCP server connected (below).

This skill is free and MIT-licensed. The app has its own licence.

## Install the skill

**Claude Code**

Copy this folder to `~/.claude/skills/leadyol/`, or clone it there:

```
git clone https://github.com/afrikyol/leadyol-skill.git ~/.claude/skills/leadyol
```

**Claude Desktop / claude.ai**

Zip the folder (the zip must contain `SKILL.md`). Go to Settings, Capabilities, Skills, and upload the zip.

## Connect the MCP server

In Leadyol: open the **AI assistants** page and click **Connect** next to Claude Desktop or Claude Code, then restart that app once.

By hand, for Claude Code (change the path to your Leadyol folder):

```
claude mcp add leadyol --scope user -e LEADYOL_URL=http://127.0.0.1:4848 -- node "C:\path\to\Leadyol\mcp\leadyol-mcp.js"
```

Optional settings: `LEADYOL_PASSWORD` (if the dashboard has a password) and `LEADYOL_EXPORT_DIR` (default: Downloads/Leadyol). Leadyol must be running.

Then ask Claude: "Show me my Leadyol overview."

## Example prompts

- Find dentists in Lyon, remove duplicates, find their e-mails, check them and export to Excel.
- Find dental clinics in Izmir without a website.
- How many restaurants are in Dubai, Riyadh and Doha? Compare.
- Build a list of real estate agencies in Casablanca with phone and e-mail only.
- Export my list as CSV, only rows with an e-mail, without personal addresses.

## Safety

- Leadyol lists public business information. Do not use it for spam or unsolicited messages.
- The do-not-contact list is always respected and left out of exports.
- Follow the law of your country (GDPR, CAN-SPAM, KVKK).
- The data is not 100% complete or up to date.

## Licence

MIT, Copyright (c) 2026 Afrikyol, LLC.

---

## Français

Leadyol trouve des entreprises par catégorie et ville depuis Claude, puis récupère les contacts de leurs sites web (e-mails, réseaux sociaux), supprime les doublons et exporte vers Excel. Données ouvertes et informations professionnelles publiques ; la liste d'opposition est toujours respectée. Respectez le RGPD.
Application et essai gratuit : https://leadyol.tr/?ref=skill-github

## Türkçe

Leadyol, Claude üzerinden sektöre ve şehre göre işletmeleri bulur, web sitelerinden iletişim bilgilerini (e-posta, sosyal medya) toplar, mükerrer kayıtları birleştirir ve Excel'e aktarır. Açık veri ve herkese açık işletme bilgileri; iletişim kurulmayacaklar listesine her zaman uyulur. KVKK'ya uyun.
Uygulama ve ücretsiz deneme: https://leadyol.tr/?ref=skill-github

## العربية

يجد Leadyol الشركات حسب الفئة والمدينة من Claude، ثم يجمع بيانات الاتصال من مواقعها (البريد الإلكتروني وحسابات التواصل)، ويحذف التكرار ويصدّر إلى Excel. بيانات مفتوحة ومعلومات تجارية عامة، وتُحترم دائمًا قائمة عدم الاتصال. التزم بالقوانين المحلية (GDPR وCAN-SPAM وKVKK).
التطبيق وتجربة مجانية: https://leadyol.tr/?ref=skill-github
