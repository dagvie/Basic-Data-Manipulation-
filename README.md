# Assignment 1: Car Listings Data

## Overview
This is the first required programming assignment for SEIS-631: Data Preparation and Analysis. You will work with a dataset of car listings drawn from Craigslist postings in the Midwest. The data contains information about the make, model, year, price, odometer, fuel type, transmission, and other details.  

Your goals are to learn how to inspect, clean, and transform a real dataset using Pandas, and to begin generating simple exploratory statistics and visualizations.  

This assignment is designed to take about 2–3 hours.  

---

## Learning Objectives
By the end of this assignment you should be able to:

- Load CSV data into a Pandas DataFrame.
- Inspect the structure of the dataset and identify missing values.
- Clean and transform variables into more useful forms.
- Create new derived variables.
- Perform basic filtering, grouping, and aggregation.
- Generate simple visualizations with Matplotlib or Pandas.
- Write a brief reflection on the data cleaning process.

---

## Data
The dataset is provided in a compressed zip file on Canvas. Download that data and extract the CSV file to your repository directory. The dataset is named `car_listings.csv`. Remember: we do not want to track large data files in Git. I've set up the `.gitignore` file to exclude CSV files, so you should be safe, but make sure you don't accidentally commit the data file.

The file contains approximately 266K rows of car listings from the following cities:

- Chicago  
- Minneapolis  
- Milwaukee  
- Kansas City  
- St. Louis  
- Madison  
- Grand Rapids  
- Omaha  
- Des Moines  
- Duluth  
- Appleton  
- Fargo  

These data were scraped from Craigslist starting in mid-2023. The dataset contains the following columns:

| Column            | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| `url`             | Full URL of the original Craigslist listing                                 |
| `location`        | City where the listing was posted (e.g., `chicago`, `minneapolis`)          |
| `post_id`         | Unique Craigslist identifier for the listing                                |
| `time_posted`     | Timestamp when the listing was posted                                       |
| `name`            | Name of the vehicle from the listing                                        |
| `make`            | Vehicle manufacturer (e.g., Ford, Toyota)                                   |
| `model`           | Vehicle model (e.g., Camry, F-150)                                          |
| `year`            | Vehicle model year                                                          |
| `odometer`        | Reported odometer reading (miles)                                           |
| `title`           | Title status (e.g., clean, salvage, rebuilt)                                |
| `paint`           | Exterior paint color                                                        |
| `drive`           | Drivetrain type (e.g., FWD, RWD, 4WD)                                       |
| `cylinders`       | Engine cylinder count (text as posted, e.g., `4 cylinders`)                 |
| `fuel`            | Fuel type (e.g., gas, diesel, hybrid, electric)                             |
| `type`            | Vehicle body type (e.g., sedan, SUV, truck)                                 |
| `transmission`    | Transmission type (e.g., automatic, manual)                                 |
| `condition`       | Reported condition of the vehicle (e.g., excellent, good, fair)             |
| `vin`             | Vehicle Identification Number (if provided)                                 |
| `price`           | Asking price in U.S. dollars                                                |
| `posting_body_text` | Free-text body of the Craigslist post                                     |
| `title_text`      | Text of the Craigslist listing title                                        |
| `num_images`      | Number of images included in the listing                                    |
| `latitude`        | Latitude coordinate of the listing                                          |
| `longitude`       | Longitude coordinate of the listing                                         |


---

## Instructions
Complete all work in the provided Jupyter Notebook (`Explore Car Listings.ipynb`).  Some of the 
steps indicated have been taken care of for you to help you get started.

### Part 0: Setup
- Import Pandas and Numpy.  
- Load the dataset into a DataFrame.  
- Inspect the first 10 rows.  

### Part 1: Inspect the Data
- Print dataset shape, data types, summary statistics, and missing values.  
- Question 1: Which columns are most incomplete?  

### Part 2: Cleaning
- Drop duplicate rows (`post_id` is unique).  
- Standardize string columns to lowercase.  
- Create new variables:
  - `car_age = 2025 - year`  
  - `high_mileage = odometer > 150000`  
  - `price_per_mile = price / odometer`  
- Question 2: Which make has the highest median `price_per_mile`?  

### Part 3: Exploration

For this next section, cut down the data to just Ford F-150s. Then:
- Fill missing values:
  - Numeric (`odometer`, `year`, `price`) with median.  
  - Categorical (`fuel`, `drive`, `transmission`, `paint`) with the most common value (the "mode").  
- Filter listings priced between $5,000 and $50,000.  
- Compute average price by location.  
- Count monthly listings using `time_posted`.  

### Part 4: Visualization

I've built some visualizations for you and asked you questions about them. 

### Part 5: Reflection
- Write a short reflection in Markdown:
  - What was most challenging about cleaning this dataset?  
  - Did anything surprise you?  

---

## Deliverables

When you have completed your work, commit and push your code file, `Exploring Car Listings.ipynb`.

---

## AI Policy
You may use AI to *ask questions* if you are stuck or need clarification. However, do not use AI code-completion or copy/paste code from AI. The purpose of this assignment is for you to gain fluency with Pandas by writing code yourself.

---

## Evaluation
Assignments will be evaluated on completeness and clarity. Work must run without errors, be written to a professional standard (clean, readable code), and include all required answers and reflection.  

## Feedback 

Rather than doing
```
listings.groupby('make')['price_per_mile'].median().sort_values().idxmax()
```

it's more common to leave off the `idmax` so that you can see the values too. This is a great way to return *exactly* the make, but it's often nice to see the top N and the values that go with them. 

You write 
>  'lamborghini' it is not meaningfull because it represents an outlier.

I'm not sure what this means. There are probably some lamborghini owners who would disagree with the "not meaningful" characterization. In class, maybe ask about this "outlier" point. Statisticians don't tend to use it in the way you're using it here. Moreover, the _statistic_ that's being calculated is the `price_per_mile` values. The make with the highest value of that statistic is lamborghini. So my question isn't whether or not you think this car is meaningful, it's about the metric we're calculating.

When you do `ford_150s = listings.loc[listings['model']=='f150']`, throw a `.copy()` on the end to avoid those warnings. 

I ask
> Check number of rows remain filtering to prices between $5,000 and $50,000

But you don't actually do the filtering, since `ford_150s[(ford_150s['price']>=5000)&(ford_150s['price']<=50000)]` doesn't change `listings`. You're just printing the results out to the screen. This causes you to give the wrong answer to the size after filtering. You should have noticed that something was weird at that point.

Very cool month to season function. It still runs into the issue that July/Aug have an extra year of data, but it's a nice idea. 

Overall, you did a very nice job. We can call this complete with one additional piece of non-coding work. Send me a message on email or Teams that answers these questions: 

* Do you think `price_per_mile` is a meaningful statistic?
* Why or why not?
