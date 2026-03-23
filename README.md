# Canadian Tax Prep Skill

A Claude skill that turns your financial documents into a production-ready, CPA-ready Excel workbook for your Canadian T1 tax return. Works for any province and any combination of employment, self-employment, investments, home office, and family deductions.

---

## Installation

1. Download `canadian-tax-prep.skill` from this repo
2. Open Claude desktop app (Cowork mode)
3. Click **"Copy to your skills"** on the `.skill` file card
4. Done — the skill is now active in all your Cowork sessions

To update the skill after making changes to the source files, re-package it:

```bash
cd /path/to/skill-creator
python -m scripts.package_skill /path/to/canadian-tax-prep /path/to/output/
```

Then reinstall via the new `.skill` file.

---

## What It Does

Given a folder of your tax documents, the skill will:

1. **Interview you** — asks a short set of questions about your tax situation (province, income types, accounts, family) so it builds only the sheets you actually need
2. **Inventory your documents** — scans the folder, lists what it found, and tells you what's missing
3. **Auto-scan receipts** — reads every file in any `Expenses/` or `receipts/` folder, categorizes each one, and flags items requiring CPA review
4. **Build the Excel workbook** — creates a formatted, formula-driven `.xlsx` file ready for your accountant
5. **Generate a CPA question list** — surfaces gaps, flags, and decisions your CPA needs to make
6. **Update your project notes** — writes a session summary back to `CLAUDE.md` so next year picks up where you left off

---

## Supported Tax Profiles

| Profile | Who it's for | Sheets created |
|---|---|---|
| `simple` / `t4` | Salaried employee, no investments, no business | Source Docs, Income, Summary, Estimates |
| `t4+invest` | Salaried + RRSP/TFSA/trading | + Investments sheet |
| `retired` | Pension/CPP/OAS + investments | + Investments sheet |
| `self-employed` | Business/freelance only | + Expenses, Expense Detail |
| `both` | T4 + business (most common complex case) | All sheets including Home Office |

The skill selects the right profile automatically based on your answers — you don't need to specify it.

---

## Recommended Folder Structure

```
2025-Taxes/
├── Documents/        ← T4s, T5s, bank statements, investment slips
├── Expenses/         ← expense receipts (PDFs, images, scans)
└── Output/           ← workbook saved here
```

Point Claude at the `2025-Taxes/` folder and it takes it from there. The folder structure is a recommendation — you can use whatever layout you already have and Claude will adapt.

---

## Output Workbook Sheets

| # | Sheet | Contents |
|---|---|---|
| 1 | Source Documents | Every file found, with extraction status |
| 2 | Income Summary | T4, self-employment, investment, other income with CRA line numbers |
| 3 | Home Office | Simplified vs. detailed method comparison (if applicable) |
| 4 | Investments | RRSP, TFSA, RESP balances; capital gains/losses; dividends & interest |
| 5 | Business Expenses | T2125 categories with gross and deductible amounts |
| 5b | Expense Detail | Line-by-line receipt register from the Expenses folder |
| 6 | Tax Summary | One-page dashboard linking all sheets, CPA questions |
| 7 | Tax Estimates | Rough federal + provincial estimate (labelled ESTIMATE ONLY) |

---

## Expense Categories Recognized

The skill automatically categorizes receipts into CRA-aligned categories:

- Software & Subscriptions
- Professional Fees
- Office Supplies
- Meals & Entertainment *(50% rule applied)*
- Air/Ground Travel
- Accommodation
- Vehicle Fuel, Maintenance, Accessories
- Telecommunications
- Insurance
- Gift Cards *(flagged — likely not deductible)*
- Banking & FX Fees
- And more (see `references/deduction_rules.md`)

Items that need CPA review are flagged automatically — gift cards, vehicle expenses (need mileage log), large single purchases over $500, and mixed-use items like home internet.

---

## Privacy Rules

The skill follows these data handling rules:

**Acceptable in the workbook:** client names, institution names, payee names — needed for CPA review context

**Never stored:** SINs, credit/debit card numbers, bank account numbers, full addresses, passwords

Amounts, dates, and categories are extracted from receipts. Source filenames are used as references.

---

## CRA Reference Material

The skill ships with three reference files used during workbook generation:

- `references/cra_lines.md` — CRA line numbers for all income, deduction, and credit fields; RRSP/TFSA/RESP rules; provincial tax brackets
- `references/deduction_rules.md` — Home office (simplified vs. detailed), vehicle expenses, meals 50% rule, gift card limits, currency conversion rules, common audit red flags
- `references/document_checklist.md` — Document checklist by profile type (T4, self-employment, investments, home office, family)

---

## How to Trigger the Skill

In Cowork, open your tax documents folder and say anything like:

> "I'm preparing my 2025 Canadian taxes."

> "Help me get ready for my accountant."

> "Build a tax workbook from the files in this folder."

> "Organize my T4 and self-employment income for CRA."

The skill activates automatically on any of these and walks you through the rest.

---

## Disclaimer

This workbook is a preparation and organization tool for discussion with a CPA — not final tax advice. Your accountant has final say on methods, amounts, elections, and interpretations. Keep all source documents for at least 7 years per CRA requirements.
