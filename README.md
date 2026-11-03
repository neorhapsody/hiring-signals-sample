# Hiring Signals Weekly: free 250-row sample

A free sample of **Hiring Signals Weekly**: companies that opened new roles in the past week, one row per company and role group, with first-hire flags. Company-level data only, with no personal contacts.

This sample has **250 rows** from the edition **2026-W44**, which covers **26 Oct–1 Nov 2026** (Malaysia time). It has 25 rows for each of the 10 role groups. The rows are chosen by a fixed hash, not by hand, and the 10 highest-scored companies are left out.

The full 2026-W44 file has 3,995 rows for 2,384 companies, with 7,022 new roles (after removing 5,330 re-posts) and 1,001 first-hire rows. It comes from the public job boards of about 7,977 companies on Greenhouse, Lever, Ashby, Workable and Recruitee, read every day.

The feed is new: 2026-W44 is its first full weekly edition, so there is no long history yet.

**Full weekly feed:** [Hiring Signals Weekly membership on Gumroad](https://fikrimarhan.gumroad.com/l/twnazx), $15/month or $129/year

The full feed is a Gumroad membership: a new file every Monday (Malaysia time) for the previous Monday-to-Sunday week.

## Files

- `hiring-signals-2026-W44-sample.csv`: the sample. Opens in Excel, Google Sheets, Numbers or Airtable.
- `LICENSE`: CC BY 4.0.

## How the data is collected

1. Every day, we read the public job board of each company we track (7,975 boards read successfully during this window): every board in the ATS Company Directory (Greenhouse, Lever, Ashby, Workable and Recruitee), plus boards that left it because they have no open roles right now.
   A board that has no open roles stays tracked. A board is dropped only after about 30 days in a row of "not found". When a company's board disappears, we check its website for a board on another ATS and follow it.
   We use only the public, unauthenticated job-board APIs. We follow robots.txt, space out requests, and back off on errors.
2. We store each job id with the date we first saw it (`first_seen`) and the date we last saw it (`last_seen`).
   If a board cannot be read on a given day, its jobs keep their previous `last_seen`, so a temporary error never looks like a closed role.
3. A role is **new** in a week when all of these are true:
   - It was first seen during the week.
   - Its company was already tracked before that day, so a board added to the directory does not show all of its roles as new.
   - It was still open at the company's latest successful snapshot.
   - It is not a re-post. A re-post is a new job id with the same title and location as a role at the same company that was open in the previous 30 days.
4. Each role is assigned to at most one role group, using its title.

## Role groups

| role_group | What it covers |
|---|---|
| sales | SDR, BDR, account executives, account managers (including key, strategic, enterprise, channel and agency account managers), sales managers and leaders, sales engineers, solutions engineers, pre-sales and solutions architects. Not sales operations or retail sales, and not insurance-servicing, collections, support or operations "account managers". |
| security | Security engineering, AppSec, cloud and product security, SOC, GRC, incident response, CISO. Not security guards or officers, and not marketing, support or recruiting roles that only mention a security product. |
| data | Data engineering, analytics engineering, data and BI analysts, data platform. Not data entry or data centre roles. Data scientists are in ml_ai. |
| devops | DevOps, SRE, platform, infrastructure and cloud engineering, release engineering. ML/AI platform and infrastructure engineers are in ml_ai. |
| ml_ai | Machine learning and AI engineers, scientists and researchers (the title says AI/ML right before the role, as in "AI Engineer" or "ML Platform Engineer", or names machine learning), data scientists, applied scientists, MLOps, prompt engineers, heads of AI. Not AI-training gig work (raters, tutors, annotators) and not roles that only mention an AI product ("Software Engineer, AI" is software_engineering). |
| software_engineering | Software engineers and developers not in the groups above: backend, frontend, full-stack, mobile, embedded and firmware, QA automation and SDET, engineering managers, forward-deployed engineers, members of technical staff. Not hardware, mechanical or electrical engineering, and not business or property developers. |
| marketing | Marketing (product, growth, performance, field, lifecycle, content, brand), demand generation, SEO/SEM and paid media, social media, communications and PR, developer relations. Not telemarketing or brand ambassadors. |
| hr_recruiting | Recruiters, sourcers and talent acquisition, HR and people teams (HRBP, people operations), compensation and total rewards, learning and development. Not "benefits specialist" call-centre roles, workers' compensation, or talent pools. |
| customer_success_support | Customer success managers, technical account managers, customer support and support engineers, customer onboarding and implementation specialists. Not internal IT or desktop support, and not call-centre, hotel or travel reservation, or insurance enrolment roles. |
| finance_accounting | Accountants, controllers, FP&A and financial analysts, finance leaders, tax, treasury, accounts payable and receivable, payroll, billing and internal audit. Not financial advisers, loans or insurance. |

Each role is in at most one group, checked in the order of this table, using the title only. Technical account managers and customer success managers are in customer_success_support, not sales. "Account director" titles are in no group.

## Work arrangement

`remote_role_count`, `hybrid_role_count` and `onsite_role_count` come from each ATS's own field where it has one (Lever and Ashby `workplaceType`, Workable `telecommuting`, Recruitee `remote` / `hybrid` / `on_site`), and from the location text otherwise (Greenhouse, or when the field is unspecified). When a role allows several, remote wins over hybrid, and hybrid over on-site. Roles with no stated arrangement are in none of the three counts. These fields are recorded from 30 Oct 2026; roles last read before then have no stated arrangement.

## Hiring velocity score (0 to 100)

The score is the same on every row of a company. It combines:

- 50% volume: new roles at the company this week, any department. It reaches the maximum at 10 new roles.
- 30% growth: new roles divided by open roles the day before the week, counting at least 5 open roles. It reaches the maximum at 50% growth.
- 20% first hire: the company has a first hire in at least one tracked group.

## Data dictionary

| Column | Type | Meaning |
|---|---|---|
| `edition` | string | File edition, e.g. "2026-W44" for a weekly file or "PREVIEW-2026-10-25" for the preview. |
| `week_start` | date | First day of the signal window (MYT calendar date). |
| `week_end` | date | Last day of the signal window (MYT); the date of the latest snapshot used. |
| `company_name` | string | Company name as shown on its job board. |
| `domain` | domain or empty | Company website domain where confirmed by the ATS Company Directory; empty when unknown. |
| `ats` | enum:Greenhouse or Lever or Ashby or Workable or Recruitee | Applicant tracking system that hosts the board. |
| `board_slug` | string | Board identifier on that ATS. |
| `careers_url` | url | Public careers board. |
| `role_group` | enum:sales or security or data or devops or ml_ai or software_engineering or marketing or hr_recruiting or customer_success_support or finance_accounting | sales = SDR/BDR/AE, account managers, sales leaders, sales and solutions engineers, solutions architects; security = security engineering and operations; data = data engineering and analytics (not data science); devops = DevOps/SRE/platform/infrastructure; ml_ai = machine learning and AI engineers and scientists, data scientists, MLOps; software_engineering = other software engineers and developers; marketing = marketing, communications, PR and developer relations; hr_recruiting = recruiting and HR/people teams; customer_success_support = customer success, technical account managers and customer support; finance_accounting = finance, accounting, tax, payroll and audit. One group per role, decided from the title in that order. |
| `new_role_count` | int>=1 | Roles in this group first seen in the window and still open at the latest snapshot (re-posts excluded). In the PREVIEW: open roles whose ATS publish date falls in the window. |
| `open_in_group_before` | int>=0 or empty | Open roles in this group on the day before the window. Empty when the company was not tracked yet, and in the PREVIEW. |
| `first_hire_in_group` | enum:yes or no or empty | "yes" when the company had 0 open roles in this group the day before the window and now has at least 1 new one. Empty in the PREVIEW and for companies tracked for less than a week. |
| `sample_titles` | string | Up to 5 distinct titles of the new roles, separated by " \| ". |
| `sample_urls` | urls | Public job-posting links for the sample titles, in the same order, separated by " \| ". |
| `locations` | string or empty | Up to 5 most common locations of the new roles, separated by " \| ". |
| `remote_role_count` | int>=0 | How many of the new roles are remote: the ATS work-arrangement field says remote (Lever/Ashby workplaceType, Workable telecommuting, Recruitee remote), or, where the ATS has no such field (Greenhouse) or leaves it unspecified, the location says "Remote", "Anywhere" or "Worldwide". A role that may be done remotely or in a hybrid way counts as remote. |
| `hybrid_role_count` | int>=0 | How many of the new roles are hybrid (ATS work-arrangement field, or "Hybrid" in the location). Roles counted as remote are not counted here. |
| `onsite_role_count` | int>=0 | How many of the new roles are on-site (ATS work-arrangement field, or "On-site"/"In office" in the location). Roles with no stated arrangement are in none of the three counts, so remote + hybrid + on-site can be less than new_role_count. |
| `remote_regions` | string or empty | Up to 3 most common regions where the remote roles can be done, separated by " \| ": "anywhere" for Anywhere/Worldwide/Global, otherwise a country or region named in the location (for example "United States", "EMEA", "LATAM"). Empty when there are no remote roles or their locations name no region. |
| `salary_min` | num or empty | Lowest published pay among the new roles in this group, in salary_currency per salary_interval. Empty when no role publishes pay. |
| `salary_max` | num or empty | Highest published pay, same currency and interval. |
| `salary_currency` | currency or empty | ISO currency of salary_min/salary_max (the most common currency+interval among the roles that publish pay). |
| `salary_interval` | enum:YEAR or MONTH or WEEK or DAY or HOUR or empty | Pay interval of salary_min/salary_max. |
| `roles_with_salary` | int>=0 | How many of the new roles in this group publish a structured pay range. |
| `company_new_roles_total` | int>=1 | All new roles at the company in the window, any department. |
| `company_open_roles_now` | int>=1 | All open roles at the company at the latest snapshot. |
| `velocity_score` | int0-100 | Company hiring velocity, 0-100 (same on every row of a company). See README for the formula. |
| `is_new_board` | enum:yes or no or empty | "yes" when the oldest open role at the company that has an ATS publish date was published less than 30 days before week_end, so the board looks newly opened (often a first-time hirer); "no" when an older open role exists; empty when the ATS gives no publish dates. |

Lists inside a cell use " | " as the separator. Cells that start with =, +, - or @ are prefixed with an apostrophe so that spreadsheets do not run them as formulas.

## What this file is not

- It has **no personal data**: no recruiter or hiring-manager names, emails or phone numbers.
- It has no copied job descriptions. It lists titles, public links and company-level facts.
- Salary appears only where the employer publishes a structured pay range on its ATS.
- Coverage is limited to the boards we track (see above). SmartRecruiters is not included because its robots.txt disallows generic crawlers.

## Sources and attribution

The list of boards comes from the ATS Company Directory, which is built from these openly licensed sources. Every board is checked live by us; the job data comes from each ATS's public job-board API.

- LastRound AI, "ATS Company Directory" (2026-08), licensed CC BY 4.0. https://github.com/fyrosofttech/lastroundai-hiring-data. Licence: https://creativecommons.org/licenses/by/4.0/
- Latmay, "ATS Career Page URLs" (Hugging Face dataset latmay/ats-career-page-urls), licensed CC BY 4.0. https://huggingface.co/datasets/latmay/ats-career-page-urls. Licence: https://creativecommons.org/licenses/by/4.0/
- Outscal OpenJobs, MIT licence, Copyright (c) 2026 Santiago Fernández de Valderrama and Outscal. https://github.com/outscal/OpenJobs
- kalil0321/ats-scrapers, MIT licence, Copyright (c) 2026 Kalil Bouzigues. https://github.com/kalil0321/ats-scrapers
- Feashliaa/job-board-aggregator, MIT licence, Copyright (c) 2026 Riley Dorrington. https://github.com/Feashliaa/job-board-aggregator
- ConorsCode/open-jobs-data, MIT licence, Copyright (c) 2026 open-jobs-data contributors. https://github.com/ConorsCode/open-jobs-data
- Board URLs discovered via the Common Crawl URL index (https://commoncrawl.org).
- Greenhouse, Lever, Ashby, Workable and Recruitee public job-board APIs. These companies are not affiliated with this product and do not endorse it.

This sample is shared under CC BY 4.0.
