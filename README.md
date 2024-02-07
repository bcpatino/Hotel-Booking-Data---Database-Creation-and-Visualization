# Hotel-Booking-Data---Database-Creation-and-Visualization

## Table of Contents

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Database Development Cleaning and Preparation](#database-development-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Creating a table for Visualization in Power BI](#creating-a-table-for-visualization-in-Power-BI)
- [Results/Findings](#results/findings)
- [Recommendation](#recommendation)
- [Limitations](#limitations)
- [References](#references)
  
### Project Overview

This project aims to create a database from csv files and build a visual data story in ower BI to present to stakeholders.
![Dashboard](https://github.com/bcpatino/Hotel-Booking-Data---Database-Creation-and-Visualization/assets/159120303/025d623d-5ab3-4f33-a876-b28dd0a85842)


### Data Sources

[Youtube](https://www.youtube.com/@absentdata)

### Tools

- Database Creation, Cleaning and Initial Analysis: PostgreSQL
- Visualization: Microsoft Power BI

### Database development, Cleaning and Preparation

1. Data Loading and Review - The csv file of the dataset was obtained from Youtube/absentdata. After the content was reviewed, planning of the analytical approach followed.
2. Database Creation - the database 'work' was developed to contain 5 tables:
        - year_2018_full
        - year_2019_full
        - year_2020_full
        - meal_cost
        - market_segment
4. Data Cleaning - a column was renamed from year_2018_full, year_2019_full and year_2020_full

### Exploratory Data Analysis 

- Is our hotel revenue growing by year? -- analyzed in PostgreSQL
```PostgreSQL
with hotel_data AS 
(SELECT * FROM year_2018_full
	 UNION SELECT * FROM year_2019_full
	 UNION SELECT * FROM year_2020_full)

SELECT hotel_data.arrival_date_year, hotel_data.hotel,
SUM((hotel_data.stays_in_weekend_nights + hotel_data.stays_in_week_nights) 
	* hotel_data.adr)  as revenue 
	FROM hotel_data
	GROUP BY hotel_data.arrival_date_year, hotel_data.hotel;
```

- Should we increase our parking lot space? -- analyzed in Power BI
- What trends can we see in the data? -- analyzed in Power BI

### Creating a table for visualization in Power BI
```PostgreSQL
 ALTER TABLE meal_cost
RENAME COLUMN meal TO meal1;

ALTER TABLE market_segment
RENAME COLUMN market_segment TO market_segment1;

WITH hotel_data AS  
(SELECT * FROM year_2018_full
	 UNION SELECT * FROM year_2019_full
	 UNION SELECT * FROM year_2020_full)

SELECT * FROM hotel_data
	LEFT JOIN public.market_segment
	on hotel_data.market_segment = market_segment.market_segment1
	LEFT JOIN public.meal_cost
	on meal_cost.meal1 = hotel_data.meal
```

### Results/Findings

1. Revenue
   - Revenue spiked from 2018 to 2019 but decreased slightly on 2020. The situation is true for both city and resort hotels.
2. Parking Space
   - Parking occupancy is only at 3%.
3. Trends
   - Average weeknight stay is higher in count that average weekend stay for both hotels.
   - Hotels in europe gained the highest revenue
   - Cancellation rate was lowest on 2018 and increased and became constant between 2019 and 2020.
   - 94% of guests are new guests while 74% came for transient stay only.
   - trend on meal availment is similar to the trend on revenue.

### Recommendation

1. Revenue
       - due to the global economic impact of covid and the restrictions it unveiled, businesses, especially in hospitality, absorbed great effect due to the decline in revenue. As the pandemic continues, expect a continuous negative impact on the business. Alternative supplementary business options may be considered fast to salvage the situation.
2. Parking – an increase in parking space is not necessary at the moment.
3. Trends – Ensure that all health and safety policies are in place especially for guests booking in for business purposes as they are the ones that will keep the hotel business afloat during the pandemic period. To salvage the constant level of cancellation, a rebooking promo may be implemented to revert the sale as a re-scheduled booking rather than complete cancellation. As Europe is heavily hit by covid infections, focus promotions to other countries that have lesser or manageable cases or in countries with reliable health protocols. Always keep in touch with the local tourism office for any updates and opportunities that will open.


### Limitations

- the project does not present a final report for stakeholders. The focus is on database and dashboard creation.

### References

1. [Absent Data](https://www.youtube.com/@absentdata)
