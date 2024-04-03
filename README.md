# New Zealand Citizenship
![flag-2395523_1280](https://github.com/ksadangrit/nz_citizenship/assets/156267785/5b01f52f-d803-4a2e-8788-795fe742fda3)


_Image by <a href="https://pixabay.com/users/gonta65-5092573/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2395523">gonta65</a> from <a href="https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2395523">Pixabay</a>_

## Introduction
This project represents a small-scale analysis driven by my personal curiosity as an immigrant and the desire to understand migration dynamics.The dataset, spanning from 1949 to 2022 (74 years), provides a summary of the number of citizenships granted over time. 

Although it lacks detailed demographic information, such as sexes and ages of individuals, my goal is to identify trends, particularly the top and least represented countries of origin for immigrants to New Zealand. By doing so, I aim to gain insights into migration patterns and their broader implications for the population dynamics of New Zealand.

**Tools: Google Sheets** for analysis and **Looker Studio** for visualizations and dashboard creation.

#### Questions
1. Which countries contribute most to the number of new citizens?
2. Has the number of citizenships granted increased or decreased over the past decades?
3. Have there been any shifts in the countries from which immigrants successfully obtain citizenship?

#### Main Objectives
* Analyze generic trends over decades.
* Examine percentage differences across decades.
* Identify the top 10 countries of all time.
* Investigate specific countries.

### Analytical workflow

## Importing data
Download the CSV file from this [website](https://catalogue.data.govt.nz/dataset/country-of-birth-for-people-granted-new-zealand-citizenship/resource/5a2d19fa-b73a-4275-b32b-1335beade1a7). This data is provided by the Department of Internal Affairs

The picture below provides a snapshot of the dataset.
![Screen Shot 2024-04-03 at 1 27 29 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/67c4e3de-d46e-4212-afca-096fc57f9d07)

#### Columns in the table
* `_id`: The identification assigned to each country, ordered alphabetically.
* `Country of Birth`: The country of birth of the immigrants granted citizenship.
* `Total`: Total number of new citizens granted citizenship.
* `%`: The percentage of new citizens from each country out of all new citizens.
* `1949` - `2022`: The span of years in which citizenships were granted.


## Cleaning the data 
* Align all header positions to the center.
* Make the country of birth cell bigger and change the % column to ‘Percentage’ for readability. 
* Check for any duplicate using the Data cleanup → Remove duplicates
* Trim any whitespaces using Trim whitespace under the Data cleanup
* Change the column name from `_id` to `ID`

**Replace blank cells with 0**

The presence of empty cells in the table implies that no immigrants from certain countries were granted citizenship in certain years. To verify this, we will use the MIN function (`=MIN(E2:BZ331`) ) to check for the minimum value, ensuring it's either 1 or 0. 

Our analysis confirms that the minimum number in this table is 1. To prevent confusion and errors in calculations or visualizations reliant on numerical data, we will replace all empty cells with 0 by using `IF` statements with `ISBLANK` and `ARRAYFORMULA` for data manipulation.

- Copy columns `C1:BZ1` (Total, %, 1949 - 2022) to `CD` column. Also, copy IDs and country names (A1:B331) to `CC` column.
- Enter `=ARRAYFORMULA(IF(ISBLANK(E2:BZ331), 0, E2:BZ331))` in `CF2` to duplicate numerical data, replacing blanks with 0.
- Calculate total using `SUM` and percentage with `=IFERROR(CD2/SUM($CD$2:CD$331)*100, ""),` ensuring two decimal points for the new percentage column (CE).
- Verify all calculated data matches the original, then replace the original table with the updated dataset.

## Analysis 
### 1. Max, Min and Average
#### 1.1 Finding max and min values 
We’ll utilize conditional formatting to highlight the maximum and minimum values across all years.
- Bold and freeze the header.
- Go to Format → Conditional formatting.
- Select the cell range and choose 'Custom formula is'.
- For max value, use `=E332=MAX($E$332:$BZ$332)` and red color.
- For min value, use `=E332=MIN($E$332:$BZ$332)` and green color.

**Finding**: The highest number of citizenships granted occurred in **2022**, with **40,355** individuals, while the lowest number was in **1950**, totaling **247** individuals. 

#### 1.2 Finding the average
We'll examine this across all years using the formula `=AVERAGE(E332:BZ332)`. 

**Finding**: Around **13,103** immigrants have been granted citizenship every year.

### 2. Finding Top 10 and Last 10 countries
#### 2.1 Top 10 birth countries
- Select all sales data.
- Go to Data → Create a filter → Click the sort button next to Total → Sort Z to A (Descending order).
- Save as a new sheet named "Top 10 countries."

#### 2.2 Last 10 birth countries
- Repeat the steps in 2.1, but change the sorting order to ascending.

**Finding**: Approximately 30 countries have only ever been granted a single New Zealand citizenship. These countries are often small islands with relatively low populations. Given the limited significance of this finding, we will not delve further into it.

Note: We won't be checking for the average or median number of citizenships granted from all countries. This is because the data is not evenly distributed, with only a few birth countries having significantly high numbers of new citizens.

### 3. Decades
#### 3.1 Group years by decades 
We'll now analyse trends in immigration and citizenship granting across the 74 years.
3.1.1 Create Decade Columns:
- Add new columns for each decade, spanning from the 1950s to the 2020s. Ensure that the year 1949 is excluded from the 1950s decade, and the 2020s only includes the years 2020 to 2022.

3.1.2 Calculate Total Number for Each Decade:
- In each decade column, use the SUM function to calculate the total number for each country and overall during that decade.
  
For example, to calculate the total number for the 1950s:
- In the column representing the 1950s, use the formula `=SUM($F2:$O2)` to sum the values from the corresponding range for each country.
- Extend this formula downwards to calculate the total for each country.
- At the bottom of the column, use the formula `=SUM(F2:O2)` to calculate the overall total for the 1950s decade.

Copy the new table with the decade columns and paste it into a new sheet for later visualisations. 

#### 3.2 Rank top 5 for each decades
- In the new sheet that we've just created for the decade columns, apply filter to the entire table and hide the total row. 
- Sort the 1950s column in descending order. 
- Copy and paste only the top 5 countries and the total into a new sheet. 
- Add a Rank column and assign colors using the Alternating colors option. 
- Repeat these steps for other decades, sorting in descending order and hiding irrelevant columns, then copy and paste into the existing sheet.

## Visualisations and Findings
In this section, I use both Google Sheets and Looker Studio to visualize the findings. I opted for Google Sheets to visualize certain findings due to its convenient built-in charting function. However, for creating a dashboard, Looker Studio provides user-friendly options that are highly effective and easy to use.

For Google Sheets, I select the cells or table, click 'Insert,' then 'Chart,' and choose the chart type. In Looker Studio, I connect Google Sheets, select the data sheet, and add a chart that best conveys the results. I won't delve into the step-by-step process of creating each chart as it's relatively straightforward. I also will refrain from revisiting specific findings previously discussed in the preceding section such as max and min values.














