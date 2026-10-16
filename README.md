# CO₂ Emissions Data Quality Investigation (2022)
Project Overview
How reliable is a country-level carbon emissions dataset when it is being used to inform climate decisions?

This project investigates a 2022 snapshot of global CO₂ emissions, population, and GDP data. The goal was to understand the structure of an unfamiliar dataset, identify missing or potentially misleading values, and determine what should be verified before using the data to guide real-world decisions.

Scenario & Questions
Imagine a climate-and-energy nonprofit deciding where to direct community-protection resources and climate-support efforts. Before using emissions data to inform those decisions, the team needs to know what the dataset actually contains and where its limitations lie.

I set out to answer five questions:

What does each column represent, and what is its appropriate data type?
Is the dataset structured or unstructured?
How many records have missing ISO country codes or GDP values?
Could missing values or regional aggregates lead to misleading conclusions?
What specific question should be answered by the data provider before the dataset is trusted for decision-making?
I also explored a separate question: Why do the reported CO₂ values for Africa and Africa (GCP) differ?

Dataset Overview
Dataset: CO₂ emissions and related indicators for 2022
File: data/co2_emissions_2022.csv
Source: Our World in Data — CO₂ and Greenhouse Gas Emissions
Original data: owid-co2-data.csv
License: Creative Commons Attribution 4.0 International (CC BY 4.0)
Source documentation: data/SOURCE.md
The file is a curated snapshot of the larger Our World in Data dataset. It retains six columns and all 254 reported entities for 2022, including countries, territories, regional aggregates, and other groupings. No values were deliberately filled in or corrected when the snapshot was created.

The original dataset draws on the Global Carbon Project and other sources. See the source repository for additional attribution and methodology.

What I Found
1. Column types
The CSV is structured data: records are organized into rows, with consistent named columns.


Column	Appropriate type	Reason
country	String / text	Contains country, territory, and aggregate names.
iso_code	String / text	Contains identifiers such as AFG and BRA; these are labels, not quantities.
year	Integer	Contains the year 2022.
population	Integer or nullable integer	Represents counts of people; some entries are blank.
gdp	Numeric, typically integer-valued in this file	Represents an economic quantity, with missing values in some records.
co2	Decimal / floating-point number	Contains emissions values such as 10.170 and 480.058.
An important distinction is that a CSV stores values as text, so these are the types inferred from the meaning and appearance of the values rather than a guarantee of how a particular software tool will parse them. Missing values also need to be handled explicitly during analysis.

2. Missing values
My terminal inspection identified two important gaps:

36 rows have a blank iso_code.
89 rows have a blank gdp.
These counts show that missing information is a recurring feature of the dataset, not just an isolated error.

However, a blank ISO code does not automatically mean that a record is incorrect. Some entries represent continents, income groups, or other aggregates that do not have individual country codes. Similarly, a blank GDP value does not establish whether the value was unavailable, inapplicable, or omitted for another reason.

3. Regional aggregates need careful interpretation
The first few CSV rows already reveal why understanding the data matters:

Afghanistan has an ISO code (AFG), a population of 40,578,847, a GDP value, and a CO₂ value of 10.170.
Africa has a population of 1,445,862,695 and a CO₂ value of 1459.114, but its ISO code and GDP fields are blank.
Africa (GCP) has a CO₂ value of 1459.103, while its ISO code, population, and GDP fields are blank.
The two Africa-related entries have slightly different reported CO₂ values. Their labels indicate that they represent different groupings or methodologies, so they should not automatically be treated as interchangeable.

Brazil provides another concrete example: its 2022 row reports a population of 210,306,411, GDP of 3,187,412,907,763, and CO₂ of 480.058.

These examples illustrate why a data analyst must examine what each row represents before comparing values or drawing conclusions.

Data Quality Question
Question for the data provider:

How are missing GDP values represented and explained in the 2022 dataset, particularly for records that represent regional aggregates rather than individual countries, and is there documentation that distinguishes unavailable GDP data from values that are not applicable?

I chose this question because 89 rows have missing GDP values, and the dataset includes both individual entities and aggregate groupings. Without knowing why values are blank, an analyst could incorrectly treat missing data as zero, exclude records that should be included, or make unfair comparisons between countries and regions.

The 36 blank ISO codes also reinforce the need to distinguish aggregate records from individual countries. A missing identifier should be investigated in context rather than automatically classified as an error.

Reflection: AI Literacy
This investigation reinforced that AI can help explain unfamiliar concepts and suggest ways to investigate data, but its output still needs to be checked against evidence.

Two risks stood out to me:

Outdated or inaccurate information: An AI assistant might provide incorrect assumptions about a dataset, its source, or its columns.
Commands without understanding: An AI-generated terminal command might produce an output that I cannot explain or verify, leaving me dependent on a result I do not fully understand.
My personal rule is to verify AI-provided facts against the actual dataset, documentation, or terminal output before relying on them professionally. I should also understand what a command does before running it and be able to explain what its result means.

The main lesson is that a plausible-looking result is not the same as a verified fact. Good analysis requires checking the source, understanding the structure, and asking questions about what the data does not tell us.

Conclusion
A dataset can be neatly organized and still require careful investigation before it is suitable for decision-making. This snapshot provides useful information about global CO₂ emissions, but missing GDP values, blank identifiers, and aggregate records all require context.

Before using it to prioritize climate-support efforts, I would seek clarification from the data provider about missing-value conventions and the definitions of aggregate records. Understanding those limitations is an essential first step toward responsible data analysis.