# MODEL TO PREDICT HOUSE PRICES IN IRELAND

## 0. Introduction
The house price prediction model is a well established problem, with plenty of data available, studies and literature around it. And yet it can be subject to errors, particularly for mid and long term projections, due to strong links to external factors such as economic performance (local and globally), interest rates, credit availability, policies from government and local authorities (property tax, tax relief, development plans), external investors, migration and demographic changes, etc.
This is relevant for both investors and owners/occupiers because the amount they are willing to pay for a property is affected by its estimated or perceived future value.

The Data Scientist's job is to make sense of the data available to the organisation and produce value from it through a series of steps:
* Ask the right questions (those that address the business objectives).
* Analise and prepare the data (perform exploratory data analysis and describe, verify its quality, select, clean, enhance, aggregate and merge the data relevant to the question, do we need more or different data, does it reveal other important questions...).
* Formulate a hypothesis and create a suitable model (is it plausible, and look for the model that best fits both the data and the questions).
* Validate the model (once tested and outputs generated, is the model a good fit and does it answers the initial questions?).
* Report the results (share the insights in a compelling way that triggers action).
* Deploy, monitor and maintain.
This is an iterative process that not always follows the exact sequence above, and also might have feedback loops at various points.

## 1. How would you interface with product, design and tech teams?
It is important both for the data scientist and these teams to have a close relationship, providing feedback often and participating in the development process, rather than work in isolation.

Collaboration with product and design teams is more relevant in the early phases of the modeling process, in order to ask the right questions and have a better understanding of the business objectives.
Another crucial moment is communicating the results (after validating the model), offering clear insights and showing the benefits of deploying the model.

Working alongside the tech team during the analysis and data preparation phase is essential to understand how the data is generated, improve data quality, and design processes that incorporate new data sources if required. 
This continues at the reporting phase when presenting the results, and later during deployment/monitoring/maintenance to help build the infrastructure that supports the solution.


## 2. What data sets would be required?
Assumption: Excluding from here sites (land), renting market and commercial property.

House prices are affected by many factors (some studies mention 400+ predictors), but as in other modeling problems a smaller subset might have the most impact.

I would collect data from several areas (each with their own attributes), such as:

- Property type: 
	House (semi-detached, detached, terraced)
	Apartment
	Duplex
	Studio

- Property features:
	Size (square meters)
	Layout and orientation
	Number of bedrooms (and size)
	Number of bathrooms (and size)
	Facilities (Parking, central heating -gas, oil-, lift, internet connection, balcony/terrace/garden)
	Less quantifiable (light, views, noise)
	
- Property condition:
	New or second hand (build year)
	Untouched / Redecorated / Renovated / Repairs required
	Floor / Kitchen / Bathroom(s) 
	Materials / construction quality / finishing
	Windows isolation level
	Official assessments (Inspection report, property valuation, energy ratings, radon risk map)

- Expected usage:
	Investor / Owner-occupier
	
- Location:
	County > Town > Area
	Coast / Interior
	Land zoning and restrictions, local building activity, local regulationss
	Air quality
	Traffic volume
	
- Access to services:
	Public transport (train, luas, bus, main road)
	Road quality
	School
	Hospital
	Shopping centre
	Amenities (cinema, pub, ...)
	Proximity to local employment opportunities
	Sports facilities (gym, swimming pool, football pitch)
	Green areas, Parks
	
- Taxes and fees:
	Management / maintenance fee
	Property tax, current taxes paid, tax exemptions

- Historical:
	Past sell prices for the property, mortgage records attached to those, taxes
	Value of houses in the vicinity, and in the broader area
	
- Demographics:
	Cost of living
	Crime levels
	Work prospects
	Household income
	Employment status
	Education levels
	Occupation
	Social status
	Age
	Gender
	Ethnic profile
	Family status (single, couple, children, retired)
	Death rate
	Birth rate
	
- External factors:
	Economic indicators (local and globally), trade market, Consumer Price Index, Salary growth, Unemployment rate, GDP
	Central bank measures (interest rates, credit availability, banking deposit rate)
	Government policies (property tax, tax relief, general and infrastructure development plans)
	Migration and demographic changes


## 3. What data cleansing steps would be needed?
Many models (multiple linear regression, gradient boosting) do not accept missing values, so this must be tackled. 
Removing full records could cause bias in the analysis, if there were many of them for example, or if there was a reason behind missing values or outliers from a set of records (clerical error or application error, etc).
Also, imputation based on neighbor values or some other criteria could cause bias too.
So any changes like that must be open to revisit later on.

Raw inspection or using histograms, scatterplots etc will help identify the contentious values.

Some examples:
- Outlier Correction
	Sqm (square meters) related features: Imputed by mean within sub area
	Distance-related features: Imputed by mean within sub area
	Remove record if price per sqm > X
	
- Missing values
	Build Year: Imputed by most common build year in sub area
	Number of Rooms: Imputed by average number of rooms in similarly sized apartments within the sub area
	State: Imputed using the build year and sub area 

- Poor quality on historical data: discard altogether, or cluster older records into one group.

- Cluster areas/neighborhoods into smaller groups, based in some criteria such as avg sales/month or avg price/sqm.

- Correct older house prices to account for accumulated inflation since then, so we can compare prices like to like:
If	CPI = Consumer Price Index, then the value of 2005 Euros vs 2017 would be:
	2017 EUR value = (2017 CPI / 2005 CPI) * 2005 EUR value
, this gives the "Purchasing Power" in 2005 relative to 2017


Then there is the topic of data enhancement, by creating, combining, splitting variables to improve prediction accuracy:

- Create new variables:
	price per sqm
	bathrooms/bedrooms ratio
	Total rooms/sqm ratio

- Combine variables for model simplicity:
	Combine distance to school, hospital, bus... into "distance to services"
	Limit top 25 areas and group the rest in "Other"

- Split variables for model simplicity
	
The enhanced variables might or not replace the original ones.



## 4. What software could be leveraged?
- Database 
For this particular problem, most of the data could be stored in a traditional RDBMS like PostgreSQL, Microsoft SQL Server, MySQL, Oracle, DB2 etc, either on premises or on the Cloud (hosted by AWS, Azure, Oracle cloud, IBM cloud etc).
Most of these databases also handle unstructured / No-SQL data sources, or could move to a No-SQL database (MongoDB, ElasticSearch, HBase etc).

- Coding language
For exploratory analysis, statistical modeling and machine learning, R or Python (there is also SAS and Matlab). There are plenty of ML packages available for them (Scikit, NumPy, TensorFlow for Python for example, Caret and others for R).

- Prototyping and sharing the model
Jupyter Notebook.

- Visualisation / Reporting
The languages above have those tools (for example ggplot2 for R, matplotlib for Python), also can incorporate some BI dashboard like in Tableau.

- Code repository
GitHub would be the choice.

- Deployment, Monitoring, Maintenance
When deploying to production, this can be done with Jenkins (CI), Docker (containers) and Kubernetes (management).



## 5. What code would need to be built and tested?




## 6. How would you validate the accuracy of the model?

In supervised predictive modeling, typically the available data (predictors and target values) is split (randomly) in two sets:
1) Training (70-90% of the data), to train your model by known sets of inputs and outputs.
2) Test (10-30% of the data), to apply once the model is built, and check its accuracy (i.e. predicted vs actual target values).

But quite often the data is split in three sets:
1) Training (50-80%), as before
2) Validation (10-25%), evaluates the model fit while tuning its parameters
3) Test (10-25%) - evaluate the final model fit and estimate the accuracy.

Also, cross-validation might be used. This technique partitions the training dataset into a number K of subsets (K-folds). 
One fold is set aside for validation, while the rest of the data (K-1 folds) is used as the training set for the model. During model testing with that fold, accuracy statistics are gathered.
The process repeats for each of the folds. 
By comparing the accuracy statistics from the various folds, you can assess the quality of the data set and the reliability of the predictions. 

Cross-validation might be used instead of the single Training-Validation (or Training-Validation-Test) approach, 


## 7. How would you deploy to production?


