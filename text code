
# # 911 calls data analysis
 

#Importing numpy and pandas
import numpy as np
import pandas as pd





#Importing visualization libraries and set %matplotlib inline. 
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('whitegrid')
get_ipython().run_line_magic('matplotlib', 'inline')




# Reading in the csv file as a dataframe called df 
df=pd.read_csv('911.csv')





#Check the info() of the df 
df.info()





# Check the head of df 
df.head()




# top 5 zipcodes for 911 calls
df['zip'].value_counts().head(5)





#top 5 townships (twp) for 911 calls
df['twp'].value_counts().head(5)





#unique title codes
df['title'].nunique()


# In the titles column there are "Reasons/Departments" specified before the title code. These are EMS, Fire, and Traffic. Using .apply() with a custom lambda expression to create a new column called "Reason" that contains this string value.
# 
# *For example, if the title column value is EMS: BACK PAINS/INJURY , the Reason column value would be EMS. *




df['reason']=df['title'].apply(lambda x:x.split(':')[0])




# the most common Reason for a 911 call based off of this new column
df['reason'].value_counts()




#using seaborn to create a countplot of 911 calls by Reason. 
sns.countplot(x='reason',data=df,palette='viridis')





#the data type of the objects in the timeStamp column
type(df['timeStamp'].iloc[0])




#Using pd.to_datetime to convert the column from strings to DateTime objects
df['timeStamp']=pd.to_datetime(df['timeStamp'])


# We can now grab specific attributes from a Datetime object by calling them. For example:
# 
#     time = df['timeStamp'].iloc[0]
#     time.hour




# using .apply() to create 3 new columns called Hour, Month, and Day of Week
df['Hour'] = df['timeStamp'].apply(lambda time: time.hour)
df['Month'] = df['timeStamp'].apply(lambda time: time.month)
df['Day of Week'] = df['timeStamp'].apply(lambda time: time.dayofweek)




#Using the .map() with this dictionary to map the actual string names to the day of the week: 
dmap = {0:'Mon',1:'Tue',2:'Wed',3:'Thu',4:'Fri',5:'Sat',6:'Sun'}





df['Day of Week']=df['Day of Week'].map(dmap)




#using seaborn to create a countplot of the Day of Week column with the hue based off of the Reason column
sns.countplot(x='Day of Week',data=df,hue='reason',palette='viridis')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)




#using seaborn to create a countplot of the Month column with the hue based off of the Reason column
sns.countplot(x='Month',data=df,hue='reason',palette='viridis')
plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)


# creating a gropuby object called byMonth, where we group the DataFrame by the month column and using the count() method for aggregation. Using the head() method on this returned DataFrame




byMonth=df.groupby('Month').count()
byMonth.head()





#creating a simple plot off of the dataframe indicating the count of calls per month
byMonth['twp'].plot()





# using seaborn's lmplot() to create a linear fit on the number of calls per month. Keep in mind we may need to reset the index to a column.
sns.lmplot(x='Month',y='twp',data=byMonth.reset_index())




#Creating a new column called 'Date' that contains the date from the timeStamp column.
df['Date']=df['timeStamp'].apply(lambda t: t.date())





#groupby this Date column with the count() aggregate and create a plot of counts of 911 calls
df.groupby('Date').count()['twp'].plot()
plt.tight_layout()





#recreating this plot but creating 3 separate plots with each plot representing a Reason for the 911 call
#the traffic plot
df[df['reason']=='Traffic'].groupby('Date').count()['twp'].plot()
plt.title('Traffic')
plt.tight_layout()



#the fire plot
df[df['reason']=='Fire'].groupby('Date').count()['twp'].plot()
plt.title('Fire')
plt.tight_layout()





#the EMS plot
df[df['reason']=='EMS'].groupby('Date').count()['twp'].plot()
plt.title('EMS')
plt.tight_layout()


#  creating heatmaps with seaborn and our data. We'll first need to restructure the dataframe so that the columns become the Hours and the Index becomes the Day of the Week. 




dayHour=df.groupby(by=['Day of Week','Hour']).count()['reason'].unstack()
dayHour.head()




#Creating a HeatMap using this new DataFrame. 
plt.figure(figsize=(12,5))
sns.heatmap(dayHour,cmap='viridis')





#creating a clustermap using this DataFrame
sns.clustermap(dayHour,cmap='viridis')





#Repeating these same plots and operations, for a DataFrame that shows the Month as the column. 
dayMonth=df.groupby(by=['Day of Week','Month']).count()['reason'].unstack()
dayMonth.head()




#Creating a HeatMap using this new DataFrame. 
plt.figure(figsize=(12,5))
sns.heatmap(dayMonth,cmap='viridis')




#creating a clustermap using this DataFrame
sns.clustermap(dayMonth,cmap='viridis')






