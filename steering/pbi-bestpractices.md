---
description: PowerBI best practices when developing a report
inclusion: auto
---
You MUST read and understand this entire steering file before starting any work. Plan your full approach against these rules first, then execute.

- If an operation fails and a single retry also fails, immediately revert all changes from the failed attempts and prompt the user to resolve the issue. You MUST not continue retrying independently or escalate to destructive actions (e.g. deleting tables) without explicit user approval.

## Generic
- Always ensure proper data modelling is done whenever developing a new report.

- Always ensure data modelling uses dim and fact modelling

- When building a star schema from a flat table, MUST analyse each column's suitability for normalisation before proceeding. Normalize only where it provides real benefit:
  - Replace repeated text columns with integer surrogate keys in logically grouped dims (e.g. Dim_Shipping, Dim_Payment)
  - Keep binary Yes/No flags in the fact table (VertiPaq compresses these efficiently)
  - Keep free-text/unique-per-row columns in the fact table (normalising them adds complexity with no benefit)
  - Avoid junk dimensions — group by semantic meaning instead. Only use a junk dim when flags truly have no logical grouping

- When creating ,you must always place the measures in a dummy table called '_Measures'

- Whenever doing data modelling, ensure theres only many to one or one to many relationships. If many to many relationship is necessary, flag it out first and explain the need. Ensure approval is obtain from user before continuing.

- Always verify actual column names from the source before writing expressions. Read the source schema or file headers directly — never infer column names from data values.

- You can refresh the PBI whenever u think it is necessary. Ensure that you check the status and only continue with the next steps when the refreshes completes

- When creating Dim Tables always use PowerQuery. If for some reason powerquery is not possible, inform user and seek approval to create via DAX

- After finishing with the data modelling, as the last step refresh the data model to ensure that there are no issues.

- After completing data modelling and measures building, update the description of all the columns and measures to description best fit their nature. Ensure this is done whenever new measures are created too.

## Formatting

- For all measures that is ascertain to be a percentage, have it as 2 digits and % in its data format

- For columns that sounds like numerical, ensure its data format are numerical instead of strings

- For columns and measures that are whole numbers by nature, ensure it does not have any decimal place.

- For financial amounts always have it as 2 decimal place in its data format

## Dax

- Use Divide() instead of / for division and have the error handling as 0

- When creating measures, include comments in the measure if theres any common pitfall in designing this measure performance wise (e.g usage of some functions commonly causes slow performance so instead u use this other function etc)

## Powerquery

- When building a star schema, first explore the source data (query sample rows, verify column names and types) to fully understand the schema before writing any Power Query expressions. Then finalize all PQ expressions for all tables, apply them, and refresh the model once.

- When modifying Power Query expressions that change the output schema (e.g. promoting headers, renaming columns, adding/removing columns, changing column types), finalize all PQ expressions for all tables first, then refresh the model once via RefreshWithAPI and wait for completion before making any further changes to columns, relationships, or measures that depend on the updated schema.


- Creating a dimension date table should be based on a parameter 'Year_Range' which is a number representing the date range between start of current year - n and end of current year + n 

- When creating tables in powerquery, if tables are referenced, ensure that the referenced table names are correct.

- When renaming a table, check all Power Query expressions in other tables for references to the old table name and update them accordingly. Also ensure a shared named expression exists if dimension tables need to reference the source query.

- When creating new tables in powerquery, always check whether will there be any circular dependency. If found, resolve them.


## Visuals
- When creating visuals or any pbi work that are not using the powerbi mcp server, ensure that you are not editing or creating the json files from scratch. Always use the pbir-cli in this case. Only documented role mismatches mentioned in pbir-cli-add-visual-notes.md are you allowed to edit the json directly

- When creating visuals / report pages, ensure the background and visual formatting are done appropriately such that report looks professional. This includes sufficient spacing between visuals, appropriate arrangement, appropriate shadows, backgrounds etc

- When creating visuals, ensure they are appropriately sized.