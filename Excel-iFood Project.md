## The Tale of Data Planet Excelos and the Kingdom of Profitshire

### Introduction:
Once upon a time, in a faraway galaxy, there was a curious little planet known as Excelos. Unlike other planets, Excelos was not made of rock or gas but of data—beautiful streams of numbers, shimmering charts, and pulsating graphs. At the heart of Excelos was a mighty kingdom known as Profitshire (aka iFood), ruled by a wise and somewhat eccentric King, known to all as King Marginus (aka The CEO). 

[Be sure to follow *The Interesting Project Template* as shown in]: # [**The Data Science Project Studio**]: #[(https://www.datacareerjumpstart.com/products/the-data-science-project-studio/categories/2150357707/posts/2158441592). ]: #

### Problem:

For many years, Profitshire flourished, and its coffers overflowed with gold (commonly referred to as "customer spending” in ancient texts). But as time passed (3 years), Profitshire found itself losing gold despite numerous peace offerings (called "campaigns" in their dialect). By the time the 6th campaign ended, King Marginus realized that mere goodwill was not enough to reverse the trend and boost the kingdom's riches.

In his moment of desperation, King Marginus called for the only person who could save the day: the legendary Captain Analysis (aka Me) with my ability to transform numbers into actionable strategies that has saved many a kingdom from ruin.

### Task:
- Provide understanding of the characteristics features of responders
- Propose and describe customer segmentation based on customer behavior

### The Data, cleaning, and Transformation:
I pored over the ancient scrolls of past peace offerings, deciphering the data hidden within.
Original data has 41 columns and 2024 rows of data. Because of the small data size, I immediately put one of my trusted companion to work, Sir Spread Sheet as the analysis tool of choice. 

- Duplicates: used TEXTJOIN to identify and remove duplicates
- Insert columns: Age group column using IFS formula;
- Insert columns: extracted name of Month and Day customers joined the campaigns using TEXT formula.

### The Analysis:
"Aha!" he exclaimed one moonlit night, "I see where we have faltered!"
Using a combination of data filtering, data sorting, general aggregation, conditional formatting, fill tools, advanced formulas, graphs, regression, and pivot table I discovered that the previous campaigns had been too general, and lacking in targeted appeal.  

#### Scatter Plot:
The plot shows a positive correlation between income and the amount spent. At about 70000 annual income the total spent for most customers     was over 500. This is the case 67% of the time per the correlation coefficient.

<img src="images/Scatter Plot of Income and total spent.png?raw=true"/>

### Bar Chats:

- Age groups: Across all campaigns it is observed that the age groups between 36-65 are the most accepting and likely to participate in a         campaign. 

<img src="images/images/Acceptance across all campaigns.png?raw=true"/>

- Marital status: analysis shows that the married, married-together, and divorced in the ages between 36 and 65 are the ones most likely          to join a campaign.

<img src="images/Marital Status by Age group.png?raw=true"/>

- Kids/Teenage at home: customers in the ages 36 - 65 with at least 1 kid or a teenager at home are the most likely to join a campaign

<img src="images/Customers with kids.png?raw=true"/>

- Purchase channel: ages 36 to 65 in campaign6 made it clear that web, catalogue and deals purchases were the most preferred. 

<img src="images/Sum of purchases.png?raw=true"/>

### 2. You can add any images you'd like. 

<img src="images/dummy_thumbnail.jpg?raw=true"/>

