---
name: leadyol
description: Required guide for every Leadyol task. Load this before calling any Leadyol MCP tool, because it holds the order of the steps, the export filters and the do-not-contact rules. Finds business leads by category and city or country with Leadyol, then gets website contacts and e-mails, removes duplicates, checks e-mails and exports to Excel or CSV. Use when the person wants a list of businesses (for example dentists in Lyon), their phones, e-mails or social profiles, or an Excel export.
---

# Leadyol

Leadyol is a program that runs on the person's own computer. It finds businesses by industry and place in open, licensed data (Overture Maps), then visits each business's own website to find e-mails, phones and social profiles. This skill tells you how to use its MCP tools.

## When to use

Use this skill when the person wants to:
- build a list of businesses by category and place ("dentists in Lyon", "bakeries in Casablanca");
- find phones, websites, e-mails and social profiles;
- find businesses with **no website** (for example to offer them one);
- remove duplicates, check e-mails, and export to Excel or CSV;
- compare how many businesses of a type exist in several cities or countries.

Leadyol does not send messages. Do not use it for that.

## Requirement

1. Leadyol is installed and **running** on the computer (it opens at `localhost:4848`).
2. Leadyol's MCP server is connected to this assistant (see README: in Leadyol, "AI assistants" page, Connect; or the `claude mcp add` command).

**First step in every session:** call `get_overview` (totals, do-not-contact count, whether the search engine is installed, running tasks). If the tool does not exist or answers "Leadyol is not running", stop and tell the person to start Leadyol and connect the MCP server. If the search engine is not installed, tell the person to click "Install search engine" once in Leadyol.

## Tools

- `find_categories`: category ids for words like "dentist" or "real estate" (1,800+ categories). Parameters: `query`, `limit`.
- `find_place`: a city, region or country in any language. Search the place name alone ("Sfax", not "Sfax Tunisia"). Returns ready-to-use `area` values for `search_businesses`. Parameters: `query`, `limit`.
- `search_businesses`: starts a search and creates a list. It runs in the background. Parameters: `area` (from `find_place`; city with radius `km`, region, country, or several countries), `categories` (ids from `find_categories`), `keyword` (words in the business name), `name` (list name), `max`, `need` (keep only businesses that have phone / website / email).
- `list_lists`: lists already made, with how many businesses have a phone, e-mail, website or social profile.
- `get_list`: results of one list, with progress while a task runs. Parameters: `list_id`, `filter` (all, email, phone, website, noweb, social, dnc), `text`, `limit` (max 200, default 25), `offset`.
- `remove_duplicates`: merge the same place found twice, or the same phone or website. Parameter: `list_id`.
- `find_contacts`: visits the websites of the list to find e-mails and social profiles. Runs in the background; follow it with `get_list`. Parameters: `list_id`, `all` (also re-visit businesses already checked). On a trial licence this counts as website visits.
- `check_emails`: checks format, whether the domain receives mail, and disposable addresses. Sends no e-mail. Parameter: `list_id`.
- `export_list`: saves an Excel (xlsx) or CSV file on the computer and returns the file path. Parameters: `list_id`, `format`, `need` (none, any, email, phone), `no_personal_emails`, `no_website`. `no_website: true` keeps ONLY the businesses without a website: use it only when the person asks for that, never to mean "skip the website step". The do-not-contact list is applied and source credits are included.
- `get_overview`: totals across lists, do-not-contact count, search engine status, running tasks.

## Workflows

### Build a list of dentists in Lyon and export it
1. `get_overview` to check Leadyol is ready.
2. `find_categories` with "dentist". Show the best ids to the person and pick one or two (for example general dentistry and dental clinic).
3. `find_place` with "Lyon". Choose the city result (default radius 15 km; you may change `km` in `area`).
4. `search_businesses` with `area`, `categories` and a clear `name`. Note the `list_id`.
5. Call `get_list` until the task is finished (a city search takes about 20 to 60 seconds; a whole country takes several minutes). Do not start the next step or another search before it ends.
6. `remove_duplicates`.
7. `find_contacts`, then `get_list` until done.
8. `check_emails`, then `get_list` until done.
9. `export_list` (for example `need: "email"`, `no_personal_emails: true`). Tell the person the file path and the exact number of rows `export_list` reports.

### Businesses with no website
Run steps 1 to 6, then `get_list` with `filter: "noweb"`, or `export_list` with `no_website: true`.

### Compare cities or countries
Run `search_businesses` for each place with the same category and compare the counts in `list_lists`. Say that open-data coverage differs by country and industry.

### Few results
Try related categories, a wider radius, or fewer `need` filters. Small towns in some countries are less covered.

## Safety rules (follow them always)

- Leadyol lists **public business information**. Do not use it to send unsolicited messages or spam.
- Respect the **do-not-contact list**. Never remove or work around it. Rows marked `dnc` stay out of exports.
- Respect the law of each country (for example GDPR, CAN-SPAM, KVKK). In the EU, UK and Turkey, use `no_personal_emails: true` when the person wants to leave out named people's addresses. How the data is used is the person's responsibility: remind them to say where they found the details and to honour every opt-out.
- Do not sell the lists as a database. Keep the source credits when sharing an export.
- Never bulk-message businesses on WhatsApp. For contacts who agreed to be messaged, that is a job for Workyol, not Leadyol.
- The data is not 100% complete or up to date: some websites no longer exist. Say so when you report numbers.
- Do not promise results ("unlimited leads", "no ban"). Report what the tools returned.

## Example prompts

- "Find dentists in Lyon, remove duplicates, find their e-mails, check them and export to Excel."
- "Find dental clinics in Izmir without a website."
- "How many restaurants are there in Dubai, Riyadh and Doha? Compare."
- "Build a list of real estate agencies in Casablanca with phone and e-mail only."
- "Show me the first 20 businesses of my last list that have an e-mail."
- "Export my list 'Bakeries Paris' to CSV, only rows with an e-mail, without personal addresses."
- "Find importers of olive oil in Germany and list their websites."
- "Clean the duplicates in my list and tell me how many were merged."
