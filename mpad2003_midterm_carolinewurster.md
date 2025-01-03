**October 16, 2024**<br>
**MPAD 2003 Data Storytelling**<br>
**Caroline Wurster**<br>
**Presented to Jean-Sébastien Marier**<br>

# Midterm Project: Exploratory Data Analysis (EDA)

## 1. Introduction

My name is Caroline Wurster and I will be analyzing a City of Ottawa dataset about the service requests made in all of August 2024.

To begin, I made a few short observations about the dataset we'll be looking at:

The City of Ottawa collected the service request data from a variety of channels, including: 311 Contact Centre, Client Service Centres, 311 Email, and Web-based Self-Service portal. The data is then presented on the Open Ottawa website where it is updated monthly. For this dataset, we will be looking at service requests from August 1, 2024 to August 31, 2024. With 11 columns total, the dataset contains both categorical variables (e.g., status, type) and numeric variables (e.g., latitude, longitude), which will require different handling in the analysis.

[Link to the original dataset on Open Ottawa.](https://www.arcgis.com/home/item.html?id=65fe42e2502d442b8a774fd3d954cac5)

[Link to the CSV version on the GitHub portal. This is the dataset we'll be looking at.](https://raw.githubusercontent.com/jsmarier/course-datasets/refs/heads/main/ottawa-311-service-requests-august-2024.csv)

As we work through this analysis, we will be looking at:
* __Getting Data:__ How to import the data into Google Sheets and making initial observations about it
* __Understanding Data:__ What to look for when writing a VIMO analysis, how to clean the dataset, and how to create an exploratory data analysis using pivot tables and charts
* __Potential Story:__ Finding a potential story that we could create with the data that we looked at
* __Conclusion:__  Conluding with what was challenging, rewarding, and what could've been done differently

## 2. Getting Data

To begin, I imported the data into Google Sheets by first opening the CSV file link from the Github repository. I then right clicked and hit “Save As” to save the file into my downloads. Then, I created a new Google Sheets file and clicked on “File → Import → Upload”. A screen popped up where I put “Replace current sheet” as the import location and “Comma” as the separator type. I then clicked “Import Data”, and after loading for a second, the file was successfully imported.


![](file-after-importation.png)<br>
*Figure 1: The file right after importing it into Google Sheets.*
[Link to my Google Sheets spreadsheet.](https://docs.google.com/spreadsheets/d/1t-jOmurv8BvXxvz8qQL1A-_T3aBUDeR_cZUblj-8FQE/edit?usp=sharing)

__General Observations Regarding The Dataset:__
* There is 11 columns and 28, 539 rows
* As for the cleanliness of the data, there are a few things that could be fixed: 
* Using the Clean-Up Suggestions tool, it told me there was over 750 columns with whitespace that could be removed
* Upon further investigation, I also found that the data is supposed to only cover all of August, however, the very last row has a September date
* I also found that a lot of the addresses are missing because they have "/N" next to them

__Specific Observations About Three Columns:__ 

__Column A__

Column A contains the service request ID of each request made, represented as discrete variables. Each ID provides a unique numeric identifier that allows for easy tracking and reference within the dataset. This identifier ensures that each request can be precisely located in the records without the need to reference other details, such as location, which helps maintain confidentiality. By using unique IDs, it is possible to manage, update, and analyze individual requests efficiently, even if other details of the request (like location) are withheld for privacy.

__Column B__

Column B contains the status of each service request as a nominal variable. The status describes each request as either resolved, active, or cancelled. If the request is resolved, it means the problem was fixed. If the request is active, it is still being worked on. If the request is cancelled, it means the service is no longer needed.

__Column H / I__

Columns H and I contain the latitude and longitude of each service request as continuous variables. However, many rows have ‘/N’ instead of a location, likely because certain service requests could be linked to specific households. Revealing the exact location might compromise privacy. Instead, the dataset includes the ward for each service request, as well. This way, the precise address is not disclosed, but the general area (ward) is still provided.

__Is There Something Missing, Out Of The Ordinary, Or Surprising About The Data?__
* One notable finding in the dataset is recorded in row 10, which details a traffic light outage that took an unusually long time to resolve—specifically, from August 1st to October 3rd, a span of over two months. This is particularly surprising because traffic light outages are critical issues that typically require immediate attention for the safety and convenience of everyone on the road.

__Hypothesis:__ Wards with higher population (such as Rideau-Vanier, Somerset, Kitchissippi, Orléans South-Navan) take longer to resolve service requests because of the amount of service requests they receive.

## 3. Understanding Data

### 3.1. VIMO Analysis

For my VIMO analysis, I used the “Review Column Stats” tool under Data → Data Cleanup → Cleanup Suggestions to help me visualize each column using a bar graph. "Using data visualization is a great way to spot anomalies in data before you get started, think about what level of incorrectness you can tolerate in the data" (Statistics Canada, 2020).

![](review-column-stats.png)<br>
*Figure 2: The Review Column Stats tool, illustrating the most frequently used channels for submitting service requests.*

__Valid:__ The status column serves as a good example of a valid value set, containing only the defined categories: 'Active,' 'Resolved,' and 'Cancelled.' Each entry is accounted for, with no blank or missing values present and they all fall within the expected range.

__Invalid:__ In the final row of data, there is a September opening date, which is technically considered invalid since this dataset is intended to reflect only service requests made throughout August.

__Missing:__ The addresses, longitude, and latitude columns contain numerous entries marked as '/N,' indicating that the expected data is absent or missing.

__Outliers:__ In the channel column, the outlier is 'counter' (as illustrated in Figure 2), with only 2 people submitting service requests in this manner. However, this may also be classified as walk-in service. We will address this inconsistency during the data cleaning process.

### 3.2. Cleaning Data (441 words)

To clean the data, I used a few different methods including: 

__1. The Google Sheets Data-Cleaning Tools__

![](data-cleanup.png)<br>
*Figure 3: Data Cleanup Tools such as "Cleanup Suggestions", "Remove Duplicates", and "Trim Whitespace"*

* To trim the whitespace in the dataset, I simply clicked "Trim Whitespace" in the Data Cleanup Tools, and then it just automatically did it for me
* To remove duplicates, I clicked the "Remove Duplicates" button, however it told me that no duplicate rows were found
* As I mentioned earlier, I used the find and replace tool (Control + F) to find the word "Counter" and replace it with "Walk-In"

![](find-and-replace.png)<br>
*Figure 4: Using the find and replace tool*

__2. Used The Split Function__


The SPLIT function divides text around a specified character or string, and puts each fragment into a separate cell in the row.

To clean the data, I used the SPLIT function for the “Description” column because it had an English and French description. However, for this dataset, everything is only in English, so we didn’t need the French translation.

* To start, I added 2 new columns 
* In one of the columns, I wrote the SPLIT function as so:

`=SPLIT(D2, "|")`
* I used "|" as the delimiter because this is where the English and French are separated so they'll go into different columns

![](SPLIT-function.png)<br>
*Figure 5: Use the SPLIT function to separate the values into 2 separate columns*

* I then double-clicked the small blue square in the lower-right corner of the cell with the formula so it would do it to the rest of the cells
* Right click to “Paste Special” → “Values Only” so you can edit the cells 
* Delete the French column (column F) and delete the original column (column D)
* Change the heading to "Description"

__3. Used The Concatenate Function__

The CONCATENATE function appends strings to one another.

To further clean the data, I used the CONCATENATE function to put the latitude and longitude into one collective column.

* To start, I added a new column
* In the column, I wrote the CONCATENATE function as so:

`=CONCATENATE(H2, ", ", I2)`
* I wrote it this way so that between the latitude and longitude, there would be a comma with a space beside it to easily differentiate between the two

![](CONCATENATE-function.png)<br>
*Figure 6: Use the CONCATENATE function to join 2 separate values into 1 column.*

* I then double-clicked the small blue square in the lower-right corner of the cell with the formula so it would do it to the rest of the cells
* Since there was a lot of \N, \N's, I used the find and replace tool to replace them with just a single \N
* Right click to “Paste Special” → “Values Only” so you can edit the cells 
* Delete the original columns, H and I
* Change the heading to "Latitude, Longitude"

__After The Cleaning Process__

After these methods, I manually went in and changed all of the titles so they were just English, expanded the columns so you could see all of the data, and deleted the last column that had a September date.

![](after-cleaning-process.png)<br>
*Figure 7: Final dataset after the cleaning process.*

### 3.3. Exploratory Data Analysis (EDA)

Now that the data is clean, I made a pivot table and bar chart to conduct an exploratory data analysis. 

The variables I chose to examine were "Type" and "Status". I chose these variables because I wanted to see what services are most in demand and how effectively the city is addressing those demands. 

* The "Type" variable categorizes service requests. This helps in understanding which services residents use the most, and which issues or services require the city’s attention.
* The "Status" variable (Active, Resolved, Cancelled) shows the progress of each request. This variable is essential for evaluating the effectiveness and efficiency of the city’s response to service requests.

![](pivot-table.png)<br>
*Figure 8: This pivot table shows the volume and resolution status of service requests by type in a table format.*

![](exploratory-chart.png)<br>
*Figure 9: This exploratory bar chart made in Datawrapper demonstrates a graphical representation of the data from the pivot table, making it easier to visually understand what the data is telling us*

After examining the pivot table and chart, there were a couple statistics that stood out to me:

* One specific statistic that stood out to me is "Garbage and Recycling". I noticed that this category has the highest total requests (10,257), with the majority resolved (10,033). 
* Another statistic that stood out to me was the amount of active requests regarding Water and the Environment. There are 1,089 active requests out of a total of 3,313. This is a large proportion of unresolved cases.

From this data and these observations, there were a few things I learned:

* Some service types, like Garbage and Recycling, have an exceptionally high volume of requests, most of which are resolved. This could imply either strong efficiency in resolving these requests or that they are straightforward to address.
* Other high-demand categories, such as Bylaw Services and Roads and Transportation, also show large volumes of requests, but with a significant number still active, indicating possible delays or backlogs.
* Water and the Environment and Roads and Transportation have a high number of unresolved, active requests. This suggests that these services might need additional resources or more efficient workflows to meet demand.

To create a potential story from this, we need to be able to catch the reader's attention. "To draw your readers in you have to be able to hit them with a headline figure that makes them sit up and take notice" (Bounegru et al., n.d.). An example for this dataset could be:

* __“Top Issues Residents Report and How Well They’re Addressed”__
* This could be a general story that explores the most common types of service requests and the city’s responsiveness in each area. 

Some variables and numbers that could warrant further investigation include:
* Understanding the time taken to resolve different types of requests. This would reveal efficiency in service delivery and help us understand where delays occur.
* To warrant further investigation, I could calculate the average time for each type of request to be resolved. I could then compare this across different types and statuses to see which requests take the longest to resolve. Longer resolution times for certain categories might indicate a need for additional resources or process improvements.

## 4. Potential Story

 ### "Top Issues Residents Report and How Well They’re Addressed"

This story could look at how well city services meet community needs by using resident feedback and service request data. By analyzing the most common requests—like garbage collection, bylaw enforcement, and road repairs—we can see which services are most in demand and how quickly the city responds. Data on response times and open cases would highlight where the city is efficient and where delays occur. Resident interviews could add personal perspectives on service satisfaction, while input from city service managers could explain challenges like limited resources. This story would give a clear picture of both the successes and struggles of city departments in serving the residents of Ottawa.

One example that could help prove my point for this story is this article on [Ottawa residents' concerns over rat infestations](https://ottawa.ctvnews.ca/this-ottawa-ward-has-the-most-complaints-about-rats-so-far-in-2024-1.6879854). This article highlights a frequent service request and the city’s response through Bylaw and Regulatory Services, making it highly relevant to the story. The article illustrates location-based differences (such as higher complaints in Rideau-Vanier) and highlights residents' frustrations with limited city intervention, emphasizing the need to explore gaps in service delivery.

## 5. Conclusion

Throughout this assignment, I went through some challenging and rewarding aspects.

One example of a challenging aspect for me was the VIMO Analysis. I think I struggled with this part of the assignment because working with such a large dataset made it difficult to find missing or invalid data, especially compared to the smaller datasets we’ve previously handled.

The most rewarding aspect of this assignment for me was cleaning the data. I think this was the most rewarding because I feel as though I have a pretty solid understanding of the different functions and tools on Google Sheets that we learned in the modules and in class. I found it interesting to use what I've learned and being able to apply it to this assignment.

I have identified some gaps in my own knowledge, such as learning more about efficient ways to clean and prepare large datasets, like handling incomplete entries.

Looking back, creating multiple visulization charts instead of just one, could've made it easier to identify different patterns and outliers than just the ones I found.

To sum up, this assignment has been a valuable learning experience, with both challenging and rewarding moments. Overall, it gave me a clearer understanding of both my strengths and areas for growth, and I’m looking forward to building on these insights in future projects.

## 6. References

Bounegru, L., Chambers, L., & Gray, J. W.Y. (2021). *The Data Journalism Handbook 1: How Journalists Can Use Data to Improve the News*. Amsterdam University Press. [https://datajournalism.com/read/handbook/one/understanding-data/start-with-the-data-finish-with-a-story](https://datajournalism.com/read/handbook/one/understanding-data/start-with-the-data-finish-with-a-story)

Pringle, J. (2024, May 9). *This Ottawa ward has the most complaints about rats so far in 2024*. CTV News Ottawa. [https://ottawa.ctvnews.ca/this-ottawa-ward-has-the-most-complaints-about-rats-so-far-in-2024-1.6879854](https://ottawa.ctvnews.ca/this-ottawa-ward-has-the-most-complaints-about-rats-so-far-in-2024-1.6879854)

Statistics Canada. (2020, September 23). *Data Accuracy and Validation: Methods to ensure the quality of data*. [https://www.statcan.gc.ca/en/wtc/data-literacy/catalogue/892000062020008](https://www.statcan.gc.ca/en/wtc/data-literacy/catalogue/892000062020008)

__Other References:__

[Module 5: Cleaning Data in Google Sheets Video](https://brightspace.carleton.ca/d2l/le/content/290806/viewContent/3855132/View)

[Module 6: Using Pivot Tables in Google Sheets Video](https://brightspace.carleton.ca/d2l/le/content/290806/viewContent/3855146/View)

