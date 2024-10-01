## Healthcare: Analyzing Readmission Rates Among Diabetic Patients" - An SQL Project! <img src="images/LinkedIn Article (3).png?raw=true"/>

### Introduction:

The healthcare industry is under immense pressure to improve patient outcomes while controlling costs. Patient readmissions are a key metric used by policymakers and insurance companies to assess the quality of care provided by hospitals.  
In today's healthcare landscape, managing chronic conditions like diabetes is a major challenge for healthcare providers. Diabetic patients require ongoing monitoring and personalized treatment plans to manage their condition effectively. However, healthcare systems often struggle with optimizing their resources, and patient readmissions.

These readmissions, often within a short period after discharge, indicate potential gaps in care and directly impact hospital performance and patient well-being. This analysis sets out to help the hospital management answer some key business questions.

**Key Business Questions:** 

[Be sure to follow *The Interesting Project Template* as shown in]: # [**The Data Science Project Studio**]: #[(https://www.datacareerjumpstart.com/products/the-data-science-project-studio/categories/2150357707/posts/2158441592). ]: #

1. What is the distribution of patients stay in the hospital? Hospital goal is 7 days or less: goal met; Most patients were able to be discharged after 3 days of hospital stay.
2. Provide a list of the medical specialties that have an average number of procedure count above 2.5 with the total procedure count above 50: The analysis shows 3 different surgery specialties, radiologist, and Cardiologist.
3. Is the hospital treating patients of different races differently, specifically with the number of lab procedures done? At an average of ~43 procedures no differences were observed in the average number of lab procedures done between races.
4. How do payer types affect patient outcomes?
    * Is there a correlation between insurance types (payer codes) and readmission rates? Most payer have a readmission rate of almost 50%.
5. What specialties have the lowest readmission rates?
    * Are there certain medical specialties that see more frequent readmissions? Can those specialties adjust their care protocols to reduce these rates? The endocrinologist has one of the lowest rates of readmission as expected. Other specialist should write for a consult with an endocrinologist before their diabetic patients gets discharged.

### Why This Project?

In many countries, including the U.S., hospitals face financial penalties for high readmission rates. Therefore, understanding the patterns of readmissions can offer a goldmine of insights to improve patient care strategies, reduce operational costs, and avoid penalties.

Through data analysis of 130 US hospitals dataset that represents ten years of clinical care I aimed to dig deep into the factors contributing to diabetic patient readmissions, and the level of care provided.

#### Dataset Overview:

The database have 2 separate tables named: ‘health’ and ‘demographics’. The tables hold detailed information about patient demographics and encounters. Key columns include:
- Race, gender, age, patient_num, etc
- Patient ID: Identifier for each patient.
- Time in Hospital: The number of days a patient stayed during the visit.
- Number of Medications and Procedures: Clinical actions taken during the patient’s stay.
- Readmission Status: Whether the patient was readmitted within 30 days, beyond 30 days, or not at all.


#### Analysis Conducted:

The analysis involved several stages to identify key insights:
1. Cleaning and Transforming the Data: We standardized the entries, and converted categorical values such as number of procedures and lab procedures, and number of medications into numeric formats for better aggregation. Using the `Alter Table and Modify columns’ query.
2. A general overview of the time spent in the hospital when admitted.

   `--Used RPAD function to generate an Histogram.
   USE patient;
   SELECT ROUND(time_in_hospital, 1) AS bucket,
   COUNT(*) AS count,
   RPAD('', COUNT(*)/100, '!') AS bar
   FROM health
   GROUP BY bucket
   ORDER BY bucket;`

The analysis shows that most patients spends between 1 to 4 days in the hospital. Overall the majority of the patients spends between 1 to 7 days in the hospital. 

3. Segmentation of Readmitted Patients: I categorized patients by their readmission status (No, <30, >30) and examined how each patient group differed in terms of the number of medications, and procedures. The average number of medications and number of procedures are similar across the 3 readmission status.


   --View created Table
   SELECT readmitted, AVG(num_medications), AVG(num_procedures)
   FROM health
   GROUP BY readmitted;


[<img src="images/--CREATE A TENP TABLE FOR CLEANING AND TRANSFORMATION.png?raw=true"/>]: #
            
4. Analyzing Payer and medical specialty Influence: I grouped readmission rates by payer_code and medical_specialty to see if certain insurance types or medical specialty correlated with higher or lower rates of readmission.
`
   SELECT 
       payer_code,
       COUNT(*) AS total_patients,
       SUM(CASE 
               WHEN readmitted IN ('<30', '>30') THEN 1 
               ELSE 0 
           END) AS total_readmissions,
       ROUND(
           (SUM(CASE 
                   WHEN readmitted IN ('<30', '>30') THEN 1 
                   ELSE 0 
               END) * 100.0) / COUNT(*), 2
       ) AS readmission_rate_percentage
   FROM 
       health
   GROUP BY 
       payer_code
   ORDER BY 
       readmission_rate_percentage DESC;
`
With the most payers having about 50% of readmission rate digging deeper into the hospital processes is recommended.
This led to similar analysis of the data grouped by the medical specialty.

A Quick Look through the table revealed some inconsistencies in country and region naming nomenclature.  

Vietnam was entered as **‘Vietnam’, and ‘Viet Nam’**; **‘North Macedonia as Macedonia', 'Macedonia, former Yugoslav Republic of', 'Macedonia, former Yugoslav Republic’.**
I conducted a little research on Macedonia and found out the current name is ‘North Macedonia’. The country entries was standardized using CASE..WHEN query to correct the entries (Only Vietnam / Viet Nam shown).

    --Identifying incorrectly spelt Viet Nam
    
    SELECT COUNT (country), country AS country_Spelling
    FROM world_bank_loan
    WHERE country IN ('Vietnam','Viet Nam')
    Group BY country;

<img src="images/Viet Nam.png?raw=true"/>

    --Update 'Viet Nam' to 'Vietnam'
    
    UPDATE world_bank_loan
    SET country = CASE country
    	WHEN 'Viet Nam' THEN 'Vietnam'
    	ELSE country
    END; 


<img src="images/country_spelling a.png?raw=true"/>
                                                                    

**Regions:** I noticed that Eastern and Southern Africa, and Western and Central Africa regions were entered both as Uppercase and Lowercase. Since most of the entry is in uppercase letters I updated the lower case regions to uppercase.
    --Region column with mix of lower and uppercase
    
    SELECT region
    FROM world_bank_loan
    GROUP BY world_bank_loan.region;

 <img src="images/SOUTH ASIA.png?raw=true"/>

    --Update region column to Uppercase
    
    UPDATE world_bank_loan
    SET region = UPPER(region);

 <img src="images/AFRICA EAST.png?raw=true"/>
 

**TRIM:** Checked for leading or trailing that could affect my search results spaces but there are none so Trimming is not needed

    --Check for leading/trailing spaces for Trimming
    
    SELECT country, 
           LENGTH(country) AS original_length,
           LENGTH(TRIM(country)) AS trimmed_length
    FROM world_bank_loan
    WHERE LENGTH(country) <> LENGTH(TRIM(country));
    
 <img src="images/integer.png?raw=true"/>

**NULLS:** 
There are Null values in the data set but none was removed or updated because the null values does not impact my analysis and some of the rows have other useful data points that I decided to retain.

### The Analysis:
I transformed raw numbers into understandable insights, revealing the maximum and average disbursements made to different countries. By visualizing the data, I traced the paths of the largest sums of money, identifying which nations had the strongest connections to the World Bank's treasure chest.

**Here are my discoveries:**

1. _What does the World bank International Development Association (IDA) loan program from 1961 to 2024 looks like: descriptive analysis was conducted using aggregate functions (Sum and Averages), Count, and Distinct functions._

The Total and Average loan $ amount disbursed from 1961 to date (1st half of 2024): a **total of $18 trillion and an average of $34 million disbursed.**

Total number of Country with recorded disbursement(s) from 1961 to date (1st half of 2024): **135 countries** has benefitted from the program.

The Total and average outstanding loan to date as of the 1st half of 2024: a **total of $24 trillion dollars at an average of $18 million is due back** to the World bank.
  
        --Historical Overview from 1961 to 2023
        --Num. of countries with loans, total Num of loans issued
        --total and average loan amount owed to IDA in US$ 
        --total and average loan amount disbursed
        
        SELECT 
        COUNT (DISTINCT country) total_country, 
        COUNT (DISTINCT credit_number) total_transactions,
        SUM(ROUND(disbursed_amount_us$,2)) total_disbursed$, 
        ROUND(AVG(disbursed_amount_us$),2) avg_disbursed$, 
        SUM(ROUND(due_to_ida_us$,2)) total_due$, 
        ROUND(AVG(due_to_ida_us$),2) avg_due$
        FROM world_bank_loan;

<img src="images/Total and average laons.png?raw=true"/>

Unique projects: a total of **8202 unique projects** has been financed to date (1st half of 2024):

    --How many unique projects has been financed
    
    SELECT COUNT (DISTINCT project_name)
    FROM world_bank_loan
    WHERE project_name !=' ';
                
<img src="images/Projects count.png?raw=true"/>


2. _Historical Y-O-Y loan $ amount trends:_ 

The amount disbursed was used in this query because the original principal amount was not always disbursed. To be able to group by the board approval year, I extracted the year from the timestamp data type, used the sum function rounded to 2 decimal places where the disbursed amount is not zero dollars.
Overall the loan amount disbursed has seen an upward trend Y-O-Y with 2009 seeing the most loan disbursed at **1.9 trillion US$.**

    --Historical Y-O-Y $ amount of loans disbursed

    SELECT EXTRACT(YEAR FROM board_approval_date) board_approval_year, 
    SUM(ROUND(disbursed_amount_us$,2)) total_disbursed$
    FROM world_bank_loan
    WHERE disbursed_amount_us$ !=0
    GROUP BY board_approval_year
    ORDER BY board_approval_year;


<img src="images/Y-O-Y loan trend.png?raw=true"/>

<img src="images/y-o-y loan trend2.png?raw=true"/>

[<img src="images/Av Income and Age.png?raw=true"/>]:#
[<img src="images/Av Income and Age.png?raw=true"/>]:#


When ordered by the total disbursed the **Year 2009 saw the highest total loan disbursed** in any year at a whooping **1.9 Trillion US dollars.** Followed closely by **year 2007 at $1.84 Trillion** and in third place is the **year 2010 at $1.77 Trillion.**

    --Historical MAX $ amount of loan disbursed and Year
    
    SELECT EXTRACT(YEAR FROM board_approval_date) board_approval_year, 
    SUM(ROUND(disbursed_amount_us$,2)) total_disbursed$
    FROM world_bank_loan
    WHERE disbursed_amount_us$ !=0
    GROUP BY board_approval_year
    ORDER BY SUM(ROUND(disbursed_amount_us$,2)) DESC;

<img src="images/2009 largest loan.png?raw=true"/>

3. _The country, project name and year with the highest principal loan amount:_

Using the aggregate max function in subquery the highest principal loan amount went to **Bangladesh in 2011 for the Padma Bridge project with a principal loan amount of $1.2 Trillion.**

    ---largest loan made out; which country and project
    --used subquery to bypass the 'group by' fx of an aggregate
    
    
    SELECT board_approval_date::date, 
    borrower, 
    project_name,country, 
    credit_number,
    original_principal_amount_us$ 
    FROM world_bank_loan
    WHERE original_principal_amount_us$ = 
    	(SELECT MAX(original_principal_amount_us$)
    	FROM world_bank_loan
    ) LIMIT 1;


<img src="images/Largest loan.png?raw=true"/>

The completed Padma Bridge (beautiful)<img src="images/Bangladesh bridge.png?raw=true"/>


4. _The Top 3 countries and regions with the most cumulative disbursements:_

Using aggregate function ‘sum’, order by, and group by to query the data I found the historical top 3 countries and regions with the most cumulative disbursed amount (US$): **_India, Bangladesh, and Pakistan in South Asia_** has the most disbursed $ amount since 1961 at **_6.5 trillion US$, about 3 trillion US$, and 2.8 trillion US$ respectively._**

    --Top 3 countries with the most cumulative $ disbursed
    
    SELECT country, region, SUM(disbursed_amount_us$) cumulative_disbursed_US$ 
    FROM world_bank_loan
    GROUP BY country, region
    ORDER BY SUM(disbursed_amount_us$) DESC LIMIT 3;

<img src="images/top 3 coutries.png?raw=true"/>


### Conclusion:

As I close the book on this tale, the key findings stand out like jewels in a crown. The World Bank's loans are not just financial transactions—they’re strategic moves that shape the prosperity of nations. By analyzing this data, I’ve uncovered which countries have benefited the most, the amount of projects that has been sponsored, and the trends in financial aid. Hope this was insightful?

Whether you're a data enthusiast, a financial wizard, or just curious about the analysis I’d like to hear your thoughts. Let's connect.



[**Reference original dataset here**](https://finances.worldbank.org/Loans-and-Credits/IDA-Statement-Of-Credits-and-Grants-Historical-Dat/tdwh-3krx)  
