# Purpose
This Python script is designed to visualize the progression of training and validation fitness over a series of generations, specifically for an AI stock screener application. The script reads data from a JSON file, which contains information about different generations and their corresponding fitness values. It then processes this data by filtering out generations beyond a specified number (45 in this case) and normalizes the fitness values to percentages. The script uses the Pandas library to handle data manipulation and conversion into a DataFrame, which is a common practice for structured data analysis in Python.

The visualization is created using Matplotlib and Seaborn, two popular libraries for data visualization in Python. The script sets up a dual-axis plot to display both training and validation fitness on the same graph, allowing for a clear comparison of these metrics over time. The plot is customized with specific styles, labels, and legends to enhance readability and presentation. Finally, the script saves the generated plot as a PNG file, making it easy to share or include in reports. This script is a standalone tool intended for data analysis and visualization, rather than a library or module to be imported elsewhere.
# Imports and Dependencies

---
- `json`
- `matplotlib.pyplot`
- `seaborn`
- `pandas`


# Global Variables

---
### df
- **Type**: `pandas.DataFrame`
- **Description**: The variable `df` is a pandas DataFrame that initially contains data loaded from a JSON file. It is then filtered to include only rows where the 'generationNumber' is less than or equal to 45. This filtering operation reduces the dataset to focus on a specific range of generations for further analysis.
- **Use**: This variable is used to store and manipulate the dataset for plotting training and validation fitness over generations.


---
### fig
- **Type**: `matplotlib.figure.Figure`
- **Description**: The variable `fig` is an instance of a `matplotlib.figure.Figure` object created using the `plt.subplots` function. It represents the entire figure or plot area where the subplots and other plot elements are drawn.
- **Use**: This variable is used to manage and display the overall plot layout, including subplots and other visual elements, and to save the plot as an image file.


---
### ax1
- **Type**: `matplotlib.axes._subplots.AxesSubplot`
- **Description**: `ax1` is a subplot axis object created using Matplotlib's `subplots` function. It is part of a figure that is set up to plot data related to training fitness over generations. The axis is configured with labels, tick parameters, and a line plot for visualizing the average training fitness percentage.
- **Use**: `ax1` is used to plot the average training fitness data and configure the primary y-axis of the plot.


---
### line1
- **Type**: `list`
- **Description**: The variable `line1` is a list containing the Line2D object(s) returned by the `plot` function of the `ax1` axis in Matplotlib. It represents the plotted line for the average training fitness over generations, with specific styling attributes such as line width, marker type, and color.
- **Use**: This variable is used to store the plotted line object for the training fitness, which is later combined with another line for legend creation.


---
### ax2
- **Type**: `matplotlib.axes._axes.Axes`
- **Description**: The variable `ax2` is an instance of a Matplotlib Axes object created as a twin of `ax1`. It shares the same x-axis but has a separate y-axis, allowing for the plotting of a different dataset on the same figure.
- **Use**: `ax2` is used to plot the 'averageValidationFitness' data on a secondary y-axis, enabling the comparison of training and validation fitness on the same graph.


---
### line2
- **Type**: `list`
- **Description**: The variable `line2` is a list containing the Line2D object(s) returned by the `plot` function of the `ax2` axis. It represents the plot of the 'averageValidationFitness' data against 'generationNumber' from the DataFrame `df`. The plot is styled with a specific line width, marker, and color to visually distinguish it as the 'Validation Fitness' line on the graph.
- **Use**: This variable is used to store the plot line for average validation fitness, which is later combined with other plot lines to create a unified legend for the graph.


---
### lines
- **Type**: `list`
- **Description**: The `lines` variable is a list that combines two line objects, `line1` and `line2`, which are created by plotting the average training and validation fitness over generations on a matplotlib plot. Each element in the list represents a line object corresponding to a plot on the graph.
- **Use**: This variable is used to gather all line objects for the purpose of creating a combined legend for the plot.


---
### labels
- **Type**: `list`
- **Description**: The `labels` variable is a list that contains the labels for the lines plotted on the graph. It is created by extracting the label from each line object in the `lines` list using the `get_label()` method.
- **Use**: This variable is used to provide descriptive labels for the legend in the plot, helping to identify each line in the graph.


---
### legend
- **Type**: `matplotlib.legend.Legend`
- **Description**: The `legend` variable is an instance of the `Legend` class from the `matplotlib` library. It represents the legend of the plot, which provides a key to the plotted lines, indicating which line corresponds to 'Training Fitness' and which to 'Validation Fitness'. The legend is positioned in the upper left corner of the plot, with a specified font size, frame, and shadow for better visibility.
- **Use**: This variable is used to display a legend on the plot, helping users identify the plotted data series.


