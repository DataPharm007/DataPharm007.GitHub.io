## The Tale of Data Planet Excelos and the Kingdom of Profitshire

### Introduction:
Once upon a time, in a faraway galaxy, there was a curious little planet known as Excelos. Unlike other planets, Excelos was not made of rock or gas but of data—beautiful streams of numbers, shimmering charts, and pulsating graphs. At the heart of Excelos was a mighty kingdom known as **Profitshire (aka iFood - a Brazilian based online food delivery service)**, ruled by a wise and somewhat eccentric King, known to all as King Marginus (aka The CEO). 

[Be sure to follow *The Interesting Project Template* as shown in]: # [**The Data Science Project Studio**]: #[(https://www.datacareerjumpstart.com/products/the-data-science-project-studio/categories/2150357707/posts/2158441592). ]: #

### The Problem:

For many years, Profitshire flourished, and its coffers overflowed with gold (commonly referred to as **"customer spending”** in ancient texts). But as time passed (3 years), Profitshire found itself losing gold despite numerous peace offerings (called **"campaigns"** in their dialect). By the time the 6th campaign ended, King Marginus realized that mere goodwill was not enough to reverse the trend and boost the kingdom's riches.

In his moment of desperation, King Marginus called for the only person who could save the day: the legendary Captain Analysis (aka Me) with my ability to transform numbers into actionable strategies that has saved many a kingdom from ruin.

### The Task:
- Provide understanding of the characteristics features of responders
- Propose and describe customer segmentation based on customer behavior

### The Data, cleaning, and Transformation:
I pored over the ancient scrolls of past peace offerings, deciphering the data hidden within.
Original data has 41 columns and 2024 rows of data. Because of the small data size, I immediately put one of my trusted companion to work, Sir Spread Sheet as the analysis tool of choice. 

- Duplicates: used **TEXTJOIN** to identify and remove duplicates
- Insert columns: Age group column using **IFS formula**;
- Insert columns: extracted name of Month and Day customers joined the campaigns using TEXT formula.

### The Discovery:
"Aha!" he exclaimed one moonlit night, "I see where we have faltered!"
By using a combination of *data filtering, data sorting, general aggregation, conditional formatting, fill tools, advanced formulas, graphs, regression, and pivot table* I discovered that the previous campaigns had been too general, and lacking in targeted appeal. Here are my discoveries: 

**Income By Age Groups:**
The data shows that the 66+ years old have the highest average income followed closely by 51-65 years old.

<img src="images/Av Income and Age.png?raw=true"/>


**Income and Total Spent**
The plot of annual income vs total spent shows a positive correlation between income and the amount spent. At about 60000 annual income the total spent for most customers was over 500. This is the case 67% of the time per the correlation coefficient.

<img src="images/Scatter Plot of Income and total spent.png?raw=true"/>


**Recency Among All Ages:**
There is no significant difference in the recency between all ages at an average recency of 49 days.

<img src="images/Average Recency by Age Group.png?raw=true"/>


**Campaign Responders Across Age groups:** 
Across all campaigns the data showed that the age groups between 36-65 are the most accepting and likely to participate in a campaign. 

<img src="images/Acceptance across all campaigns.png?raw=true"/>


**Marital status and Campaign Responders:** 
analysis shows that the married, married-together, and divorced in the ages between 36 and 65 are the ones most likely to join a campaign.

<img src="images/Marital Status by Age group.png?raw=true"/>


**Responders and Family size:** 
customers in the ages 36 - 65 with at least 1 kid or a teenager at home are the most likely to join a campaign.

<img src="images/Customers with kid.png?raw=true"/>


**Responders and Purchasing channel:** 
the ages 36 to 65 in campaign6 made it clear that the web, catalogue, and deal purchases were the most preferred. 

<img src="images/Sum of Purchases.png?raw=true"/>

### Proposals:
With the newfound insight, here are my proposed new peace offerings (campaign targets). 

**- Demographic segmentation:** *Targeted customers should preferably be within the ages of 36 and 65, and married with at least 1 kid or a teenager at home.* 

**- Economic segmentation:** *campaign targets the ages between 36 and 65 with an average annual income of at least 60000.*

**- Customer behavior:** *campaigns should be tailored with a call-to-action to make a web, catalogue or a deal purchase.*

**- (Data not shown)** *Analysis shows that most responders opted in to campaigns during the month of January and preferably on a Saturday followed closely by Thursday. So, campaigns should be tailored to reach recipients by Thursdays or Saturdays.*


### In Conclusion:
King Marginus (The CEO), overjoyed, declared a grand feast celebrating turning their data into a story of triumph.
And that, dear reader, is how a kingdom on the data planet Excelos turned its fortunes around and thrived once more.

Thank you for discovering with me.
Do you have any questions I can help you answer? Don't hesitate to reach out. 👍

[**Reference original dataset here**](https://github.com/nailson/ifood-data-business-analyst-test/blob/master/ifood_df.csv)  
