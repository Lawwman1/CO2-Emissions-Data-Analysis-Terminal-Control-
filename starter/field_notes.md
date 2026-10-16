# Data Literacy Field Notes & Terminal Log

**The scenario:** A climate-and-energy nonprofit wants to use this
country-level energy and emissions dataset to help decide where to direct
community-protection and climate-support efforts — but the file just
landed with zero documentation. Before anyone can trust a decision built
on top of it, you need to figure out what's actually in it — what each
column really holds, and anything that looks off. That's what this whole
file walks through: the real, everyday first pass any analyst runs on an
unfamiliar file, and the real answers you need before this data is safe
to act on.

Dataset: `../data/co2_emissions_2022.csv` (see `../data/SOURCE.md`). Each
section below describes what you're trying to find out — **figuring out
which command (and which flags) actually gets you there is part of what
this project tests**, so the exact command isn't given. Where a command
belongs, you'll see a `# TODO:` comment describing the goal instead of a
ready-to-run command; replace the comment with the real command you used.
Paste the **real output** you got underneath, then write your answer based
on what you actually saw, not what you'd expect. Work through this in
order; open the raw file before anything else.

Run all commands from the repo root — the top-level folder you land in
after cloning *your own* copy of this repo (`git clone ...` then `cd
your-repo-name`, see `README.md` Step 3; not this template) — or `cd` into
`data/` first and drop the `data/` prefix — your choice, just be
consistent.

**This one file is your whole record** — the commands you ran, their real
output, and your written answers, all together in the order you actually
did the work. There's no separate log file to keep in sync with this one;
whenever you write a command down anywhere in this file, use a fenced code
block tagged `bash`, like this:

````
```bash
git status
```
````

Three backticks, the word `bash`, your command, three backticks to close —
it renders with syntax highlighting and makes the command copy-pasteable.

## Setup log

Before jumping into the dataset, log the commands you actually ran to set
this repo up (see `README.md`) — `git init`/clone, creating your
`.gitignore`, your first commit. Add to this as you go, don't
reconstruct it from memory at the end.

```bash
mkdir Mod0Project
cd Mod0project
git clone https://github.com/Lawwman1CO2-Emissions-Data-Analysis-Terminal-Control-.git
cd CO2-Emissions-Data-Analysis-Terminal-Control-
touch .gitignore
git add .
git commit -m "Added Git Ignore"
git push
cd data/

```

## 1. First look at the raw file

Before trusting anything about this file's structure, you want a quick,
safe peek at it — not the whole 254-row file, just the **first 5 lines**,
enough to see the shape of it without scrolling past everything.

```bash
head -n 5 co2_emissions_2022.csv 
```

**Output:**

```bash
country,iso_code,year,population,gdp,co2
Afghanistan,AFG,2022,40578847,53303471617,10.170
Africa,,2022,1445862695,,1459.114
Africa (GCP),,2022,,,1459.103
Albania,ALB,2022,2827614,36171005594,4.498
```
**What I notice:**

```bash
I noticed that there are certain data points that are missing from the spreadsheet (The ISO code, population, and GDP and missing from Africa)
```



## 2. Column names

Next you want to see just the column names by themselves — one per line is
a lot easier to scan than one long comma-separated row.

```bash
head -n 1 co2_emissions_2022.csv  
```

**Output:
```bash
country,iso_code,year,population,gdp,co2
```

## 3. Column-by-column data type table

For each column, you want to preview real values — **the first 6 values**
of just that one column — without scrolling through every field on every
row. Column numbers for reference: `country`=1, `iso_code`=2, `year`=3,
`population`=4, `gdp`=5, `co2`=6.

```bash
cut -d ',' -f 1 co2_emissions_2022.csv | head -n 7
```

Fill in the table from what you actually saw — not a guess from the column
name alone:

| Column | Inferred type | How I can tell |
|---|---|---|
| _Country_ | String | The countries are all words that do not have any numeric values |
| _ISO Code_ | Int | All these are numbers without decimal poimts that hold numeric values |
| _Year_ | Int | Like the ISO code, years and whole numbers that actually have numeric value |
| _Population_ | Int | Population has to be descibed by a number, and you can't have 0.5 of a person |
| _GDP_ | Float | GDP is a numeric value. However, unlike ISO codes, the GDP has a decimal point, making it a float|
| _Co2_ | float | Co2 levels have a numeric decimal point value, making them a float |

**In your own words:** what's the actual difference between a list/array
and a table? (Not a definition you looked up — explain it the way you'd
explain it to a friend who's never coded. No command needed for this one,
just think about the table you just filled in above.)

```bash
A list is more linear than an array; a list is more 1 dimensional, whle an array can be both horizontal and vertical
```

## 4. Structured or unstructured?

State whether this dataset is structured or unstructured, and why.

```bash
This data is semi-structured, as the column that the data seems to be sorted by, countries, is not fully structured. there are titles in the countries category that are not actually countries. Some are continents, and others are unspecified.
```
## 5. Before you trust this data

Your first look (see section 1) turned up a row with a suspiciously
**blank** `iso_code`. Before you build anything on this data, you want to
know exactly how many of the 254 rows share that blank — is it a one-off,
or a real pattern worth flagging before anyone draws conclusions from it?
You may need more than one command for this — piping (`|`) feeds one
command's output into the next as input.

```bash
A command (or combination) that counts how many rows have a BLANK iso_code

cut -d ',' -f 2 co2_emissions_2022.csv | grep -c '^$'
```

**Output:**
_36_

```bash
The same idea, but for the gdp column

cut -d ',' -f 5 co2_emissions_2022.csv | grep -c '^$'
```

**Output:**
_89_

List **at least 2** specific things you'd want to know about how this
dataset was collected, before trusting any conclusion drawn from it — ground
at least one in the counts you just found.

**My 2 concerns:**
1. I would want to know why so many countries ISO code is missing. Every country has an ISO code, so the fact that there are 36 entries missing it is concerning
2. Should it be inferred that there was no data to report for the GDP for some countries, or that ther entries are not countries, therefore they have no GDP?

## 6. Data-quality question for the source

Take the **most specific and checkable** of the two concerns above, and
phrase it as a real question you'd send to the data's source/maintainer. A
good test: could they actually answer it with a concrete yes/no or fact —
not "is this data good?"

```bash
Does the GDP missing have something to do with the miscatergorization of certain values?
```
## 7. Something you're actually curious about

Different question from #6 — that one was about checking the data's
trustworthiness. This one's about you: what's something you're genuinely
curious to explore using this data? Pick two specific countries you're
curious to compare, and look up their full rows.

```bash
A command that finds a specific country's row by name

grep -i "brazil" co2_emissions_2022.csv
grep -i "africa" co2_emissions_2022.csv

```

**Output:**
Brazil,BRA,2022,210306411,3187412907763,480.058

Africa,,2022,1445862695,,1459.114
Africa (GCP),,2022,,,1459.103
Central African Republic,CAF,2022,5098038,3752056621,0.377
South Africa,ZAF,2022,62378417,723559323969,428.782

**My question:** Why is there a difference between Africa and Africa (GCP)?

## 8. AI-literacy reflection

Name **2 concrete risks** of trusting an AI assistant's output about this
dataset (or any data question) without checking it yourself. Then state
**one specific personal rule** for when you'll double-check an AI-provided
fact before relying on it professionally — specific enough that you could
actually follow it, not "I'll be careful."

**Risks:**
1. AI could give outdated information
2. AI does not always explain the answer to you, so if I needed to understand why the code it gave me worked I had to enter extra prompts

**My rule:** Don't enter an output that you cannot personally verify after. I need to be able to check the AI's work after it produces an output

## 9. Something that went wrong (and how you figured it out)

Somewhere in this project you're going to run a command that either throws
an error or gives you output that isn't what you expected — everyone hits
this at least once. Paste the actual command and what actually happened
(not a paraphrase), then explain what that told you and how it led to the
fix — not "I googled it and it worked."

```bash
# TODO: a command that errored, or gave you unexpected results

git add
```

**What happened:**
Nothing specified, nothing added

**What it told me and how I fixed it:** I did an incorrect syntax. I needed to add a period after git add (git add .)
