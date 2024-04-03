# New Zealand Citizenship
![flag-2395523_1280](https://github.com/ksadangrit/nz_citizenship/assets/156267785/5b01f52f-d803-4a2e-8788-795fe742fda3)


_Image by <a href="https://pixabay.com/users/gonta65-5092573/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2395523">gonta65</a> from <a href="https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2395523">Pixabay</a>_

## Introduction
This project represents a small-scale analysis driven by my personal curiosity as an immigrant and the desire to understand migration dynamics.The dataset, spanning from 1949 to 2022 (74 years), provides a summary of the number of citizenships granted over time. 

Although it lacks detailed demographic information, such as sexes and ages of individuals, my goal is to identify trends, particularly the top and least represented countries of origin for immigrants to New Zealand. By doing so, I aim to gain insights into migration patterns and their broader implications for the population dynamics of New Zealand.

**Tools: Google Sheets** for analysis and **Looker Studio** for visualizations and dashboard creation. I have chosen Google Sheets for cleaning and processing the data, considering the relatively small size of the dataset with just over 300 rows and less than 80 columns. Additionally, Looker Studio is a convenient tool for creating dashboards, offering seamless connectivity with other data sources.

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
* As no empty rows or NA values are present, we won't eliminate any rows

**Replace blank cells with 0**

The presence of empty cells in the table implies that no immigrants from certain countries were granted citizenship in certain years. To verify this, we will use the MIN function (`=MIN(E2:BZ331`) ) to check for the minimum value, ensuring it's either 1 or 0. 

Our analysis confirms that the minimum number in this table is 1. To prevent confusion and errors in calculations or visualizations reliant on numerical data, we will replace all empty cells with 0 by using `IF` statements with `ISBLANK` and `ARRAYFORMULA` for data manipulation.

- Copy columns `C1:BZ1` (Total, %, 1949 - 2022) to `CD` column. Also, copy IDs and country names (A1:B331) to `CC` column.
- Enter `=ARRAYFORMULA(IF(ISBLANK(E2:BZ331), 0, E2:BZ331))` in `CF2` to duplicate numerical data, replacing blanks with 0.
- Calculate total using `SUM` and percentage with `=IFERROR(CD2/SUM($CD$2:CD$331)*100, ""),` ensuring two decimal points for the new percentage column (CE).
- Verify all calculated data matches the original, then replace the original table with the updated dataset.

## Analysis 
In this section, I will primarily discuss the steps I employ to sort, filter, or calculate the data for analysis. Findings will be discussed as relevant. However, the majority of the findings will be visualized and discussed in the next part of this project.

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
- In each decade column, use the `SUM` function to calculate the total number for each country and overall during that decade.
  
For example, to calculate the total number for the 1950s:
- In the column representing the 1950s, use the formula `=SUM($F2:$O2)` to sum the values from the corresponding range for each country.
- Extend this formula downwards to calculate the total for each country.
- At the bottom of the column, use the formula `=SUM(F2:O2)` to calculate the overall total for the 1950s decade.

Copy the new table with the decade columns and paste it into a new sheet named 'decades' for later visualizations.

#### 3.2 Rank top 5 for each decade
- In the new sheet that we've just created for the decade columns, apply filter to the entire table and hide the total row. 
- Sort the 1950s column in descending order. 
- Copy and paste only the top 5 countries and the total into a new sheet. 
- Add a Rank column and assign colors using the Alternating colors option. 
- Repeat these steps for other decades, sorting in descending order and hiding irrelevant columns, then copy and paste into the existing sheet.

### 4. Forecasting and Percentage Change
#### 4.1 Yearly
- Copy the year and total number into new columns.
- Highlight all the cells containing years and total numbers.
- Drag the fill handle down until the year 2040 to see the forecasted numbers.
- Create another column for the percentage change from the previous year using the formula `=(B3-B2)/B2*100`, where B3 represents the year before and B2 represents the second year for which we want to calculate the change.

#### 4.2 Decades
- Copy the decades and total columns and paste them into a new sheet.
- Add another column for the percentage change, applying the formula from step 4.2 but adjusting the columns from B to N.
- Apply conditional formatting using a color scale from green to red to visually represent the range of percentage changes for each decade, with green indicating lower values and red indicating higher values.
  

## Visualisations and Findings
In this section, I use both Google Sheets and Looker Studio to visualize the findings. I opted for Google Sheets to visualize certain findings due to its convenient built-in charting function. However, for creating a dashboard, Looker Studio provides user-friendly options that are highly effective and easy to use.

For Google Sheets, I select the cells or table, click 'Insert,' then 'Chart,' and choose the chart type. In Looker Studio, I connect Google Sheets, select the data sheet, and add a chart that best conveys the results. I won't delve into the step-by-step process of creating each chart as it's relatively straightforward.

### Citizenships granted from 1949 to 2022
![Screen Shot 2024-04-03 at 11 06 05 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/83c127e7-3306-4914-a3b5-573f33e395e8)

**Findings**
- Citizenships granted saw a sharp decline from 1949 to 1950, but have steadily risen since then until 1978.
- Overall, there's been an increasing trend in citizenships granted over the past 74 years.
- Significant drops occurred in 1999, 2010, and 2021 compared to the preceding years.
- In 2020, the number of citizenships granted reached a peak at 40,355, more than doubling the count from the previous year.

### Citizenships granted across decades
![Screen Shot 2024-04-03 at 11 14 47 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/2022142f-07d0-44a5-9d5a-b84a6db13c5f)

**Findings**
- Citizenships granted have increased consistently in every decade.
- A notable surge occurred between the 1990s and 2000s.
- The 2010s witnessed the highest number of citizenships granted, while the 1950s had the lowest count.
- It's important to note that while the graph suggests a significant decrease from the 2010s to the 2020s, this conclusion cannot be drawn conclusively as the dataset only extends to the year 2022.

### Top 5 birth countries for each decades 
![Screen Shot 2024-04-03 at 3 11 41 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/9ce59d0e-09ad-4849-8870-2b509a40daf9)

**Findings**
- The United Kingdom consistently ranks in the top 2 countries across all decades, holding the first position in 1949 and during the 1970s to 1990s.
- The UK has been surpassed only twice: by the Netherlands in the 1950s and 1960s, and by India in the 2020s.
- From 1949 to the 1980s, the majority of the top 5 birth countries were from Western regions, including the UK, Netherlands, and Australia. However, this trend began to shift in the 1990s. 
- Since the 1970s, Samoa has consistently been among the top 5 countries every decade except the 2000s.
- The majority of new citizens from China were granted citizenships in the 1990s and 2000s.


### Top 10 birth countries where new citizens come from
![Screen Shot 2024-04-03 at 11 07 56 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/4d0b63b8-c2d1-4e8f-8719-fdcd148c9e66)

**Findings**
- Since 1949, the UK has contributed the most to the number of new citizens, with over 233,000 individuals originating from there, surpassing the second-ranked Samoa by more than 2.8 times.
- Notably, there is a marginal difference of less than 10,000 individuals between countries ranked from second to seventh.
- Half of the top 10 countries are from Asia, with India ranking third, China fifth, and the Philippines, South Korea, and Taiwan occupying the seventh to ninth positions, respectively.
  
### Top 5 birth countries (of all time) across decades
![Screen Shot 2024-04-03 at 11 14 15 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/3328e00f-da16-4690-851d-b1e142e089d5)

**Findings**
- The UK saw a surge in citizenships granted from the 1960s to the 1980s, peaking in the 1980s with over 60,000 immigrants gaining citizenship. Samoa's highest peak was in the 2010s, with a secondary peak in the 1980s.
- The 1980s marked a significant increase in citizenships granted across all countries.
- Trend lines for the UK and Samoa are similar, with Samoa's increase beginning in the 1970s. India, China, and South Africa saw notable increases from the 1980s onwards.
- India's peak was in the 2000s, while China and South Africa peaked in the 2010s.
- The apparent decrease in citizenships granted in the 2020s may not be conclusive, as data only extends to 2022.
  
### Forecasting future trends for the coming years
![Screen Shot 2024-04-03 at 11 39 48 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/5f568b42-17a9-45ac-89a9-fbaaf13f2fca)

### Analyzing the percentage change for each year
![Screen Shot 2024-04-03 at 10 41 39 PM](https://github.com/ksadangrit/nz_citizenship/assets/156267785/8a6fe5d2-5104-4407-bde0-86ea40c969f9)

### Key Findings

Click [here](https://lookerstudio.google.com/s/hT1nYgxOJcU) to access the dashboard on the Looker Studio.

![NZ_Citizenship](https://github.com/ksadangrit/nz_citizenship/assets/156267785/10e63677-9967-49b9-9dbf-6c162b3a37e5)



















