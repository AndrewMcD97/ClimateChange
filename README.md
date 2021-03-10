| BIG DATA ANALYTICS | 1 |
| --- | --- |
|
 |
 |

![](RackMultipart20210310-4-1y71sua_html_3bd4f5cadc2dede1.gif)
# European City Temperature Analysis

Andrew Mc Daid

![](RackMultipart20210310-4-1y71sua_html_b8c7456bc16dd1b2.gif)

**Abstract—**  **Global Warming or Climate Change is one of the main issues faced by the current generation. This project aims to use analysing techniques to figure out the extent that the earth has warmed up from the year 1750-2015, which parts of the world that has risen by the most as well as the reasons why this is the case. The intended outcomes are to determine when Global Warming truly began as well as its effect on the future of European Cities. Another outcome will be the effect the oceans have on the temperature by measuring if a city being on a coastline has a different effect that cities not on a Coastline. Included in this report are descriptions of steps taken in sorting out the useful data and the methods used for analytics and visualisations are included in this report.**

**Index Terms—**  **Big Data Analytics, Spark, Climate Change**

![](RackMultipart20210310-4-1y71sua_html_b8c7456bc16dd1b2.gif)

1. INTRODUCTION

Global Warming is an issue that is extremely prevalent in the world today and is an issue that we must take the utmost urgency in fixing it. It is essentially the long-term heating of the earth and has been happening since the industrial age as more and more manufacturing of items switched to machine based and fossil fuel burning. On average each decade, the global temperature is now increasing at 0.2 degrees Celsius. The goal of this project is to use techniques on temperature data from Kaggle in order to aid in answer the following questions relating to Global Warming:

1. What will the temperatures in the European cities be in the future? What will the year on year increase/decrease in temperature be? Will this cause problems?
2. In what parts of the world is the temperature rising/falling by the most every year?

1. Does a city being on a coastline impact its temperature in relation to other cities of a similar latitude/longitude that do not have a coastline?

1. What can we state about the global temperature based on the historical outlook from the year 1750 onwards?

A rundown of the source data as it appears on GitHub is featured below. Later in this report the data itself is explained in detail. The GitHub link can be found in the notes section at the end of this report alongside other useful links:

This will all be achieved using datasets found on Kaggle as well as LYIT Blackboard at:

**https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data**

The analytics performed on these datasets will be achieved using Spark, PySpark as well as through using a Databricks cluster and several other Python packages such as Plotly and Seaborn for graphical representations. For this project, all of my processes are run using a Spark Cluster on Databricks. This cluster has the following details: Runtime 8.0 (Scala 2.12, Spark 3.1.1) with 15.3GB RAM and 2 Cores and 1DBU. Together this offers enough processing power for the amount of data being looked at.

1. DATA DESCRIPTION

The data being used is in the form of 6 CSV files, 5 from Kaggle and the other from LYIT Blackboard (CitiesExt.csv).

Each CSV files contains historical temperature date dating from the year 1750-2015. Included within this data is the city in which the temperature is being recorded, the country the city is in, the date this temperature is recorded (typically on the First of every month of every year) These figures combine 1.6 billion temperature reports from 16 pre-existing archives. The CSV files are divided up into interesting subsets (City, Country, Global) all the raw data comes from Berkeley Earth data page: [http://berkeleyearth.org/data/](http://berkeleyearth.org/data/)

2BIG DATA ANALYTICS

![](RackMultipart20210310-4-1y71sua_html_f7979a273d58101f.gif)

The other CSV Files known as CitiesExt.csv comes from LYIT Blackboard and includes the average temperatures of 213 Cities across Europe as well as their co-ordinates and whether or not they are on a coastline.

1. DATA PRE-PROCESSING

Prior to trying to answer the questions that was proposed, pre-processing must be done on the data in order to make it more malleable. This was done using the very useful **pandas** Python library.

A. Separation of CSV Files

Upon creation of the python file, the required files were uploaded in a file known as **archive.zip** that contained the 5 files from Kaggle that was needed. Upon uploading these files were unpacked into separate CSV files. These CSV files were then turned into pandas Data frames using the pd.read\_csv command within python.

![](RackMultipart20210310-4-1y71sua_html_9f061caf875098.png)

![](RackMultipart20210310-4-1y71sua_html_2560c32bee6189ab.png)

C. Separation of Datetime into Year, Month

The default data frame comes with a YYYY-DD-MM format, but for the purposes of this project, the year value itself is what is needed for performing analytics. To separate this out, a lambda function is preformed on the data frame in order to only take into account the first 4 characters of each row of this particular column.

![](RackMultipart20210310-4-1y71sua_html_ee41c0150d1a6b63.png)

Fig. 4. Extracting Year out of the datetime format.

D. Splitting The 12 Values for Year into One Value

After completing the previous step there will be 12 separate values for each year corresponding to the average temperature of each month in the year. The mean temperature of each year has to be calculated and to do this the .mean() and .reset\_index() commands can be used in order to get the average and put it into one singular value. This will later help to calculate yearly average temperatures and see what the temperature will be like in future.

Fig. 2. Separating CSV files into usable Pandas Dataframes

Fig. 2. Turning CSV files into dataframes

B. Dropping of Rows Without Values

In order to further clean the new data frames that have been created, the rows without useful data should be dropped or deleted so that they do not affect our analytics later on. To do this the .dropna() command was used on the data frames.

The inplace=true label was selected as this means that the original data frame will permanently get rid of the empty rows rather than only creating a temporary view of the data frame

with the rows removed and then placed back in after viewing.

![](RackMultipart20210310-4-1y71sua_html_c79d8f5759238a2f.png)

Fig. 3. Dropping empty rows from the GlobalTempsYearly df

![](RackMultipart20210310-4-1y71sua_html_f85c117965f3858f.png)

Fig. 5. Getting the mean value per year for temperature

| CANCER DEATH ANALYTICS | 3 |
| --- | --- |
|
 |
 |

IV. ANALYTICS

This section features implementation and results of the 4 questions posed at the start of the report.

A. What can we state about the global temperature based on the historical outlook from the year 1750 onwards?

This analysis will be able to show us clear information on a global level about the prevalence of Climate Change as well as when it sped up and what the current rate of progression is.

In order to do this a pandas dataframe of the CSV file GlobalTemperatures.csv was created in order to get a look at the data. This particular data contains an average of the everywhere in the world&#39;s temperature statistics on the first day of every month of every year since 1750. The earliest years show a bit of dirty data as the methods of collecting temperature were not as good back then as there is today. However, upon inspecting the dataset it seems that the temperatures become more consistent as time goes on.

The dataframe that was created had its empty rows dropped and then the Year taken out of the datetime column within it. Upon cleaning the data, the _plotly_package was used in order to create a visualization of the data within the dataframe.

The plot pits each year against the average temperature for that year taking into account every month of the year.

![](RackMultipart20210310-4-1y71sua_html_4645d2a55c45e442.png)

Fig. 6. Code for a line plot of Year vs Temp

The visualization in question is a line plot connecting the average global temperature of each year from 1750 to the next year in order to see the trend between years and a greater idea of the extent of Global Warming. As we all know, Global Warming is in part caused by the increase of industrialization and the burning of fossil fuels within our atmosphere, this is in turn leading to a melting of the polar ice caps due to higher temperatures within our atmosphere.

![](RackMultipart20210310-4-1y71sua_html_47debddf625d0d46.png)

Fig. 7. The line plot of Year vs Avg Temp in Celsius

From the graph above, the early years are perhaps not so accurate until around the 1800&#39;s due to technology at the time in counting the temperature. But from the 1800&#39;s onwards there is a clear increase in temperature globally due to the rise of industrialization in the world, especially in the richer countries at the time like Britain, The USA, France, Germany and Spain.

Another observation is that since 1975, the rate of increase has gone up severely. Some of the reasons for this is due to the far higher global population, producing more waste as the factories have got more sophisticated and are able to produce far more units/items in the same time span as years earlier. At this point plastic was also being manufactured at a far higher rate than before and recycling had not yet become commonplace.

4BIG DATA ANALYTICS

![](RackMultipart20210310-4-1y71sua_html_f7979a273d58101f.gif)

B. In what parts of the world is the temperature rising/falling by the most every year?

In order to get a better understanding of Global Warming, I wanted to know where it was affecting the most and when the highest yearly changes were, particularly from the year 1975 when there was a clear sign of increasing temperatures at a rate never seen before. In order to calculate this, the GlobalLandTemperaturesByCity.csv file was used in order to gauge what cities suffered most with this affliction.

To do this the relevant columns were taken from this dataframe and then the year the calculations took place from was set to greater than 1975. Following this, A simple machine learning model was built that would preform a linear regression on the countries and cities in the world in order to train it and calculate the change in temperature per year for each unique city in the world.

To find out the cities with the greatest change per year, this slope that showed the change year on year was then placed into a sorted list from highest change per year to lowest and then plotted into a seaborn bar plot with the greatest at the top and decreasing down the bar plot. In the end this showed the Increment of temperature per year per city in the world.

![](RackMultipart20210310-4-1y71sua_html_511a1585e836fcbc.png)

![](RackMultipart20210310-4-1y71sua_html_1d151075b9793b03.png)

Fig. 8. The increase in temperature per year per city

It was found that Belgorod of Russia and Pavlohrad of Ukraine had the biggest change year on year with an average change of about 0.06 degree Celsius. It seems that countries nearer to the north and south pole are experiencing global warming to a greater extent that the countries and cities nearer the equator due to the ice caps melting.

| CANCER DEATH ANALYTICS | 5 |
| --- | --- |
|
 |
 |

C. Does a city being on a coastline impact its temperature in relation to other cities of a similar latitude/longitude that do not have a coastline?

I was curious to see whether or not European cities with a coastline had a higher temperature than cities that did not have a coastline, so I decided to plot them against one another by separating out the dataframe using the CitiesExt.csv file. I used the column &#39;coastline&#39; within this file to separate the dataframe into cities with a coastline and cities without a coastline.

![](RackMultipart20210310-4-1y71sua_html_2e674fdde982abd.gif) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png)

![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png) ![](RackMultipart20210310-4-1y71sua_html_253969f19100091f.png)

Fig. 9. Separating data frames by having a coastline or not

for a map of Europe beneath these co-ordinates but

the cities are on the right points and you can make out

the shape of the continent.

![](RackMultipart20210310-4-1y71sua_html_555a7f548613ac03.png)

Following this two scatterplots were created based on the

longitude and latitude of European cities. They were then

coded with a legend which would indicate whether or not the

city had a coastline, **blue = coastline, red = no coast line**.

![](RackMultipart20210310-4-1y71sua_html_a07e7039a0f44cea.png)

Fig. 10. Creating a plot based on co-ordinates and coastlines

From this a plot with the cities was able to be created,

Unfortunately, I was not able to get BaseMap working

1. CONCLUSION

This project offered a great chance for gaining new information on an issue that will not only affect myself but also generations to come. It helped to highlight how we need to be focusing on it as individuals and bring in new reforms in order to curb the increase in temperatures that we find ourselves facing year on year. Whether this be threw eco-friendly fuels being developed with a similar efficiency to fossil fuels or through other means, every avenue needs to be explored to in order to come up with viable ways of dealing with this issue in todays industrial world. Thankfully in recent years, famous people and budding young activists have been placing a greater importance on these issues that have been reflected in their frequency of display through traditional and mainstream media channels.

As well as learning information on the subject matter, I also gained useful information on python packages that had previously been unknown to myself. It helped to increase my personally-lacking skills in pulling useful information and insights out of data – something I am hoping to get better at down the line and keep improving on.

I also learnt that pandas proved to be an efficient way of cleaning data for usage in analytics. Spark is an interesting way of using data when the data you are dealing with is of vast quantities. It&#39;s capabilities are something im eager to learn more of and be able to rely on more than pandas. Pandas is great but can struggle with larger CSV files. The visualizations although sometimes difficult to create and sometimes unable to look the way I wanted were a very nice visual way of interpreting data and making it easy for myself to understand.
