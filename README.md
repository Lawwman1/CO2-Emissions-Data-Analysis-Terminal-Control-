# CO₂ Emissions Data Quality Investigation

# Project Overview

How reliable is a country-level carbon emissions dataset when it is being used to inform climate decisions?

This project investigates a 2022 snapshot of global CO₂ emissions, population, and GDP data. The goal was to understand the structure of an unfamiliar dataset, identify missing or potentially misleading values, and determine what should be verified before using the data to guide real-world decisions.

# Scenario & Questions

I set out to answer five questions:

What does each column represent, and what is its appropriate data type?
Is the dataset structured or unstructured?
How many records have missing ISO country codes or GDP values?
Could missing values or regional aggregates lead to misleading conclusions?
What specific question should be answered by the data provider before the dataset is trusted for decision-making?
I also explored a separate question: Why do the reported CO₂ values for Africa and Africa (GCP) differ?


# Data Quality Question
Question for the data provider:

How are missing GDP values represented and explained in thedataset, particularly for records that represent regional aggregates rather than individual countries, and is there documentation that distinguishes unavailable GDP data from values that are not applicable?

I chose this question because 89 rows have missing GDP values, and the dataset includes both individual entities and aggregate groupings. Without knowing why values are blank, an analyst could incorrectly treat missing data as zero, exclude records that should be included, or make unfair comparisons between countries and regions.

The 36 blank ISO codes also reinforce the need to distinguish aggregate records from individual countries. A missing identifier should be investigated in context rather than automatically classified as an error.

# Conclusion
A dataset can be neatly organized and still require careful investigation before it is suitable for decision-making. This snapshot provides useful information about global CO₂ emissions, but missing GDP values, blank identifiers, and aggregate records all require context.

Before using it to prioritize climate-support efforts, I would seek clarification from the data provider about missing-value conventions and the definitions of aggregate records. Understanding those limitations is an essential first step toward responsible data analysis.

# Source and license

Source:
Our World in Data. CO₂ and Greenhouse Gas Emissions Dataset. https://github.com/owid/co2-data (accessed October 16, 2026)