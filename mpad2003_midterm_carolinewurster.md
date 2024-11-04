**October 16, 2024**<br>
**MPAD 2003 Data Storytelling**<br>
**Caroline Wurster**<br>
**Presented to Jean-Sébastien Marier**<br>

# Midterm Project: Exploratory Data Analysis (EDA)

## Foreword

For this assignment, you must extract data from a dataset provided by the instructor. You must then clean and analyze the data, create exploratory charts/visualizations, and find a potential story idea. Your assignment must clearly detail your process. You are expected to write about 1500-2000 words, and to include several screen captures showing the different steps you went through. Your assignment must be written with the Markdown format and submitted on GitHub Classroom.

**Here are some useful resources for this assignment:**

* [The template repository for this assignment in case you delete something by mistake](https://github.com/jsmarier/jou4100_jou4500_mpad2003_project2_template)

## 1. Introduction

My name is Caroline Wurster and I will be analyzing a City of Ottawa dataset about XYZ
* Offer a succinct description of the dataset you are analyzing (at least three or four short observations: how the data was collected, what it includes, etc.).

The City of Ottawa collects service request data from various channels, including the 311 Contact Centre, Client Service Centres, 311 Email, and the Web-based Self-Service portal. This data is then presented on the Open Ottawa website, where it can be viewed by ward and includes information about the responsible department and a description of each request. The dataset is updated monthly. For our analysis, we will focus on the month of August 2024.

[Here is the link to the original dataset on Open Ottawa.](https://www.arcgis.com/home/item.html?id=65fe42e2502d442b8a774fd3d954cac5)

[Here is the link to the CSV version on the GitHub portal.](https://raw.githubusercontent.com/jsmarier/course-datasets/refs/heads/main/ottawa-311-service-requests-august-2024.csv)

## 2. Getting Data

I imported the data into Google Sheets by first opening the CSV file link from the Github repository. 
I then right clicked and hit “Save As” to save the file into my downloads
Then, I created a new Google Sheets file and clicked on “File → Import → Upload”.
A screen popped up where I put “Replace current sheet” as the import location and “Comma” as the separator type.
I then clicked “Import Data” and after loading for a second, the file was successfully imported.


![](file-after-importation.png)<br>
*Figure 1: The file right after importing it into Google Sheets*

[Here is the link to my Google Sheets spreadsheet](https://docs.google.com/spreadsheets/d/1t-jOmurv8BvXxvz8qQL1A-_T3aBUDeR_cZUblj-8FQE/edit?usp=sharing)

* Some general observations regarding the dataset:
* There is 11 columns and 28 539 rows

For the most part the data looked clean, however there are a few things that could be fixed: 
* I used the Clean-Up Suggestions tool and it told me there was over 750 columns with whitespace that could be removed
* The data is supposed to only cover all of August, however, the very last row has a September date
* A lot of the addresses are missing

* Make specific observations about at least three columns: 
What types of variables are we dealing with? Be specific. For example: "Column A features nominal variables with the names of all participants in the study. Column B includes the age of each participant as discrete variables."

Column B contains the status of each service request as a nominal variable. The status describes each request as either resolved, active, or cancelled. If the request is resolved, it means the problem was fixed. If the request is active, it is still being worked on. If the request is cancelled, it means the service is no longer needed.

Columns H and I contain the latitude and longitude of each service request as continuous variables. However, many rows have ‘/N’ instead of a location, likely because certain service requests could be linked to specific households. Revealing the exact location might compromise privacy. Instead, the dataset includes the ward for each service request. This way, the precise address is not disclosed, but the general area (ward) is still provided.

Column A contains the service request ID of each request made, represented as discrete variables. Each ID provides a unique numeric identifier that allows for easy tracking and reference within the dataset. This identifier ensures that each request can be precisely located in the records without the need to reference other details, such as location, which helps maintain confidentiality. By using unique IDs, it is possible to manage, update, and analyze individual requests efficiently, even if other details of the request (like location) are withheld for privacy.


* Is there something missing, out of the ordinary, surprising?
* One notable finding in the dataset is recorded in row 10, which details a traffic light outage that took an unusually long time to resolve—specifically, from August 1st to October 3rd, a span of over two months. This is particularly surprising because traffic light outages are critical issues that typically require immediate attention for the safety and convenience of motorists and pedestrians.

Hypothesis: Wards with higher population (such as Rideau-Vanier, Somerset, Kitchissippi, Orléans South-Navan) have more service requests because of greater demands on infrastructure


**Here are examples of functions and lines of code put in grey boxes:**

1. If you name a function, put it between "angled" quotation marks like this: `IMPORTHTML`.
1. If you want to include the entire line of code, do the same thing, albeit with your entire code: `=IMPORTHTML("https://en.wikipedia.org/wiki/China"; "table", 5)`.
1. Alternatively, you can put your code in an independent box using the template below:

``` r
=IMPORTHTML("https://en.wikipedia.org/wiki/China"; "table", 5)
```
This also shows how to create an ordered list. Simply put `1.` before each item.

## 3. Understanding Data (700-1000 words)

### 3.1. VIMO Analysis (162 words)

For my VIMO analysis, I used the “Review Column Stats” tool under Data → Data Cleanup → Cleanup Suggestions to help me visualize each column using a bar graph.

![](review-column-stats.png)<br>
*Figure 2: The Review Column Stats tool, illustrating the most frequently used channels for submitting service requests.*

__Valid:__ The status column serves as good example of a valid value set, containing only the defined categories: 'Active,' 'Resolved,' and 'Cancelled.' Each entry is accounted for, with no blank or missing values present and they all fall within the expected range.

__Invalid:__ In the final row of data, there is a September opening date, which is technically considered invalid since this dataset is intended to reflect only service requests made throughout August.

__Missing:__ The addresses, longitude, and latitude columns contain numerous entries marked as '/N,' indicating that the expected data is absent or missing.

__Outliers:__ In the channel column, the outlier is 'counter' (as illustrated in Figure 2), with only 2 people submitting service requests in this manner. However, this may also be classified as walk-in service. We will address this inconsistency during the data cleaning process.


Support your claims by citing relevant sources. Please follow [APA guidelines for in-text citations](https://apastyle.apa.org/style-grammar-guidelines/citations).

**For example:**

As Cairo (2016) argues, a data visualization should be truthful...

### 3.2. Cleaning Data

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

To clean the data, I used the SPLIT function for the “Description” column because it had an English and French description. However, for this dataset, everything is only in English so we didn’t need the French translation.

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
* I then double-clicked the small blue square in the lower-right corner of the cell with the formula so it would do it to the rest of the cells
* Since there was a lot of \N, \N's, I used the find and replace tool to replace them with just a single \N
* Right click to “Paste Special” → “Values Only” so you can edit the cells 
* Delete the original columns, H and I
* Change the heading to "Latitude, Longitude"



### 3.3. Exploratory Data Analysis (EDA)

Insert text here.

**This section should include a screen capture of your pivot table, like so:**

![](pivot-table-screen-capture.png)<br>
*Figure 2: This pivot table shows...*

**This section should also include a screen capture of your exploratory chart, like so:**

![](chart-screen-capture.png)<br>
*Figure 3: This exploratory chart shows...*

## 4. Potential Story

Insert text here.

## 5. Conclusion

Insert text here.

## 6. References

Include a list of your references here. Please follow [APA guidelines for references](https://apastyle.apa.org/style-grammar-guidelines/references). Hanging paragraphs aren't required though.

**Here's an example:**

Bounegru, L., & Gray, J. (Eds.). (2021). *The Data Journalism Handbook 2: Towards A Critical Data Practice*. Amsterdam University Press. [https://ocul-crl.primo.exlibrisgroup.com/permalink/01OCUL_CRL/hgdufh/alma991022890087305153](https://ocul-crl.primo.exlibrisgroup.com/permalink/01OCUL_CRL/hgdufh/alma991022890087305153)
