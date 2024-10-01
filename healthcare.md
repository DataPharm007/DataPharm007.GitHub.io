## Healthcare: Analyzing Readmission Rates Among Diabetic Patients" - An SQL Project! <img src="images/LinkedIn Article (3).png?raw=true"/>

### Introduction:

The healthcare industry is under immense pressure to improve patient outcomes while controlling costs. Patient readmissions are a key metric used by policymakers and insurance companies to assess the quality of care provided by hospitals.  
In today's healthcare landscape, managing chronic conditions like diabetes is a major challenge for healthcare providers. Diabetic patients require ongoing monitoring and personalized treatment plans to manage their condition effectively. However, healthcare systems often struggle with optimizing their resources, and patient readmissions.

These readmissions, often within a short period after discharge, indicate potential gaps in care and directly impact hospital performance and patient well-being. This analysis sets out to help the hospital management answer some key business questions.

**Key Business Questions:** 

[Be sure to follow *The Interesting Project Template* as shown in]: # [**The Data Science Project Studio**]: #[(https://www.datacareerjumpstart.com/products/the-data-science-project-studio/categories/2150357707/posts/2158441592). ]: #

1. What is the distribution of patients stay in the hospital? Hospital goal is 7 days or less: goal met; _Most patients were able to be discharged after 3 days of hospital stay._
2. Provide a list of the medical specialties that have an average number of procedure count above 2.5 with the total procedure count above 50: _The analysis shows 3 different surgery specialties, radiologist, and Cardiologist._
3. Is the hospital treating patients of different races differently, specifically with the number of lab procedures done? _At an average of ~43 procedures no differences were observed in the average number of lab procedures done between races._
4. How do payer types affect patient outcomes?
    * Is there a correlation between insurance types (payer codes) and readmission rates? _Most payer have a readmission rate of almost 50%._
5. What specialties have the lowest readmission rates?
    * Are there certain medical specialties that see more frequent readmissions? Can those specialties adjust their care protocols to reduce these rates? _The endocrinologist has one of the lowest rates of readmission as expected. Other specialist should write for a consult with an endocrinologist before their diabetic patients gets discharged._

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
- Cleaning and Transforming the Data: We standardized the entries, and converted categorical values such as number of procedures and lab procedures, and number of medications into numeric formats for better aggregation. Using the `Alter Table and Modify columns’ query.
- A general overview of the time spent in the hospital when admitted.

```SQL
   --Used RPAD function to generate an Histogram

   USE patient;
   SELECT ROUND(time_in_hospital, 1) AS bucket,
   COUNT(*) AS count,
   RPAD('', COUNT(*)/100, '!') AS bar
   FROM health
   GROUP BY bucket
   ORDER BY bucket;
   ```

The analysis shows that most patients spends between 1 to 4 days in the hospital. Overall the majority of the patients spends between 1 to 7 days in the hospital. 

- Segmentation of Readmitted Patients: I categorized patients by their readmission status (No, <30, >30) and examined how each patient group differed in terms of the number of medications, and procedures. The average number of medications and number of procedures are similar across the 3 readmission status.

``` SQL
   --Average number of medications and procedures by readmission status
   
   SELECT readmitted, AVG(num_medications), AVG(num_procedures)
   FROM health
   GROUP BY readmitted;
```

[<img src="images/--CREATE A TENP TABLE FOR CLEANING AND TRANSFORMATION.png?raw=true"/>]: #
            
- Analyzing Payer and medical specialty Influence: I grouped readmission rates by payer_code and medical_specialty to see if certain insurance types or medical specialty correlated with higher or lower rates of readmission.

```SQL
   --Readmission rates by payer_code
   
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
```

With the most payers having about 50% of readmission rate digging deeper into the hospital processes is recommended.
This led to similar analysis of the data grouped by the medical specialty.

- The hospital director needs a list of the medical specialties that have an average number of procedure count above 2.5 with the total procedure count above 49.

```SQL
   SELECT distinct(medical_specialty), 
   ROUND(AVG(num_procedures),1) average_proc, 
   COUNT(medical_specialty) count
   FROM health
   GROUP BY medical_specialty
   HAVING count > 49 AND average_proc > 2.5
   ORDER BY average_proc DESC;
```

<img src="images/Viet Nam.png?raw=true"/>

- The Chief of Nursing wants to know if the hospital seems to be treating patients of different races differently, specifically with the number of lab procedures done.

```SQL
   SELECT ROUND(AVG(num_lab_procedures),2) lab, race FROM health
   JOIN demographics ON health.patient_nbr=demographics.patient_nbr
   WHERE race !='?'
   GROUP BY race
   ORDER BY lab DESC;
```

<img src="images/country_spelling a.png?raw=true"/>
                                                                    
The analysis indicated there are no differences in the way patients are treated based on their race and the number of lab procedures done. 

**Other requests:**
- The Research department needs a list of all patient_numbers who are African-American or have a "Up" to metformin:

```SQL
   SELECT DISTINCT patient_nbr 
   FROM demographics 
   WHERE race = "African-American"
   UNION
   SELECT patient_nbr 
   FROM health 
   WHERE metformin = "Up"
   ORDER BY patient_nbr;
```

 <img src="images/SOUTH ASIA.png?raw=true"/>

- The management wants a report in the following format:

"Patient [patient_nbr] was [RACE] and was [READMISSION STATUS]. They had [num_ medications and [num_lab_procedures.]”

I used a combination of CASE WHEN, CONCAT, and JOINS to be able to accomplish this.

```SQL
   --Used CONCAT function

   SELECT CONCAT('Patient ', demographics.patient_nbr, ' was ',  race, ' and ', 
   (CASE 
   	WHEN readmitted !='NO' THEN ' was readmitted. They had '
   	ELSE ' was not readmitted. They had '
       END), num_medications, ' medications ',' and ', num_lab_procedures, ' lab procedures.') AS summary
   FROM health
   JOIN demographics ON health.patient_nbr=demographics.patient_nbr
   WHERE num_medications>= 50 AND race != 'Other' AND race !='?'
   ORDER BY num_medications, num_lab_procedures DESC;
```

 <img src="images/AFRICA EAST.png?raw=true"/>


**Key takeaways from this analysis:**

- Most patients stay in the hospital is less than 7 days.
  
- The number of medications and procedures are similar across all patients.
  
- The endocrinologist have the one of the lowest rates of admission; the hospital care plan for diabetic patients admitted for other conditions should include consultation with an endocrinologist before dis charge.

**The Impact:**

By applying these insights, hospitals can redesign care plans for diabetic patients, to include an endocrinologist in the treatment plan before discharge regardless of the primary admission diagnosis. They can reduce readmission rates, improve patient outcomes, and ultimately reduce costs associated with long-term diabetic care.
      
### Conclusion:

This analysis offers healthcare providers a roadmap to improving the care of diabetic patients and provides actionable data that can lead to better clinical decisions, lower costs, and healthier outcomes for patients managing chronic diabetes.



[**Reference original dataset here**](https://www.kaggle.com/datasets/brandao/diabetes?resource=download)  
