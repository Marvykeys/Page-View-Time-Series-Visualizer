# (FreeCodeCamp) Page-View-Time-Series-Visualizer :alarm_clock:

As one of the final tasks at the end of the FREECODECAMP Python For Data Analysis Course,

I was required to visualize a time series data using a line chart, bar chart, and box plots. 
I used Pandas, Matplotlib, and Seaborn to visualize a dataset containing the number of page views each day on the freeCodeCamp.org forum from 2016-05-09 to 2019-12-03. 
The data visualizations aids understanding the patterns in visits and identify yearly and monthly growth.

I used the data to complete the following tasks

1) Imported the data from "fcc-forum-pageviews.csv" using pandas. 

2) Set the index to the "date" column.

3) Clean the data by filtering out days when the page views were in the top 2.5% of the dataset or bottom 2.5% of the dataset.

4) Created a draw_line_plot function that uses Matplotlib to draw a line chart.

5) I made "Daily freeCodeCamp Forum Page Views 5/2016-12/2019" the title, as required.

6) I labeled the x axis as "Date" and the y axis as "Page Views".

7) I created a draw_bar_plot function that draws a bar chart showing average daily page views for each month grouped by year, with the legend should show month labels and have a title of "Months", the label on the x axis should be "Years" and the label on the y axis should be "Average Page Views".

8) I created a draw_box_plot function that uses Searborn to draw two adjacent box plots showing how the values are distributed within a given year or month and how it compares over time. 

9) As required, I titled the first chart "Year-wise Box Plot (Trend)" and titled the second chart "Month-wise Box Plot (Seasonality)". 

```Python
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()

# Import data (Make sure to parse dates. Consider setting index column to 'date'.)
df = pd.read_csv("fcc-forum-pageviews.csv", parse_dates = ["date"], index_col = "date")

# Clean data
df = df[
  (df["value"] >= df["value"].quantile(0.025)) &
  (df["value"] <= df["value"].quantile(0.975))]


def draw_line_plot():
    # Draw line plot
    fig, ax = plt.subplots(figsize= (10, 5))
    ax.plot(df.index, df["value"], "r", linewidth = 1)

    ax.set_title("Daily freeCodeCamp Forum Page Views 5/2016-12/2019")
    ax.set_xlabel('Date')
    ax.set_ylabel('Page Views')


    # Save image and return fig (don't change this part)
    fig.savefig('line_plot.png')
    return fig

def draw_bar_plot():
    # Copy and modify data for monthly bar plot
    df["month"] = df.index.month
    df["year"] = df.index.year
    df_bar = df.groupby(["year", "month"]) ["value"].mean()
    df_bar = df_bar.unstack()
    # Draw bar plot
    fig = df_bar.plot.bar(legend=True, figsize= (10,5), ylabel="Average Page Views", xlabel="Years").figure
    plt.legend(["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"])

    plt.xticks(fontsize = 10)
    plt.yticks(fontsize = 20)

    # Save image and return fig (don't change this part)
    fig.savefig('bar_plot.png')
    return fig

def draw_box_plot():
    # Prepare data for box plots (this part is done!)
    df_box = df.copy()
    df_box.reset_index(inplace=True)
    df_box['year'] = [d.year for d in df_box.date]
    df_box['month'] = [d.strftime('%b') for d in df_box.date]

    # Draw box plots (using Seaborn)
    df_box["month_num"] = df_box["date"].dt.month
    df_box = df_box.sort_values("month_num")

    fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(10,5))
    axes[0] = sns.boxplot(x=df_box["year"], y=df_box['value'], ax= axes[0])
    axes[1] = sns.boxplot(x=df_box["month"], y=df_box['value'], ax= axes[1])

    axes[0].set_title('Year-wise Box Plot (Trend)')
    axes[0].set_xlabel('Year')
    axes[0].set_ylabel('Page Views')

    axes[1].set_title('Month-wise Box Plot (Seasonality)')
    axes[1].set_xlabel('Month')
    axes[1].set_ylabel('Page Views')
    # Save image and return fig (don't change this part)
    fig.savefig('box_plot.png')
    return fig
```
