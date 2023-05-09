
# Introduction to the Following Code
The code uses the R programming language and the ggplot2 library to create a visualization of net exports as a percentage of GDP over time for different countries in South Asia. The data is read from a CSV file named "Safal.csv" and then processed to convert the country names to three-letter ISO codes and remove any observations with missing data. The ggplot function is used to create the plot, with the x-axis representing the year and the y-axis representing net exports as a percentage of GDP. The colour and shape aesthetics differentiate the different countries, with each country represented by a unique colour and shape.

In addition, the geom_line function is used to create a line plot, showing the trend in net exports over time for each country. The geom_point function is used to add points to the plot, with the size and shape of the points indicating the country. The geom_text function adds labels to the plot, showing the exact value of the maximum net exports as a percentage of GDP for each country. The scale_shape_manual function is used to customize the shapes used to represent the different countries. The plot is further customized using various ggplot2 functions, such as xlab, ylab, ggtitle, labs, and theme, to add axis labels, a title, a subtitle, and to change the plot's theme. The facet_wrap function is used to create multiple plots, one for each country, arranged in a grid.

Finally, the function of the guide is used to remove the legend for colour and override the legend for shape with black colour.

**The data for this was downloaded from the World Bank, world development indicators data, which is available at the World Bank website**


**Necessary packages and libraries**

`install.packages("tidyverse")`

`install.packages("countrycode")`

`install.packages("ggplot2")`

`install.packages("pacman")`

`library(tidyverse)`

`library(countrycode)`

`library(pacman)`


you can also upload other packages such as to read xl, csv and other as per need.

**Read in the data**

`Safal <- read.csv("Safal.csv")` #**_you can read the data directly from your depository using the read.csv function** 

`Safal$code <- countrycode(sourcevar = Safal$country, origin = "country.name", destination = "iso3c")` _#**Convert country names to three-letter ISO codes**_

`Safal <- na.omit(Safal)` _#**Remove observations with missing data**_

**Plot the data using ggplot2**

```library(ggplot2) # load ggplot2 package for plotting

ggplot(Safal, aes(year, NE.EXP.GNFS.ZS, color=country, shape = country)) + # create ggplot object with Safal data and mapping aesthetics

  geom_line() + # add line plot
  
  geom_point(data = Safal %>% group_by(country) %>%
  
               filter(NE.EXP.GNFS.ZS == max(NE.EXP.GNFS.ZS, na.rm = TRUE)),
               
             size = 3, shape = 22) + # add point plot for maximum values
             
  geom_text(data = Safal %>% group_by(country) %>% 
  
              filter(NE.EXP.GNFS.ZS == max(NE.EXP.GNFS.ZS, na.rm = TRUE)), aes(label = round(NE.EXP.GNFS.ZS, 2), 
              
    x = year, y = NE.EXP.GNFS.ZS + 0.5), size = 3, vjust = -0.75, hjust = -0.5, size = 3, show.legend = FALSE) + # add labels for maximum values
    
  scale_shape_manual(values = c(19, 17, 15, 13, 11, 9, 7, 5)) + # set manual shapes for legends
  
  xlab('Year') + # set x-axis label
  
  ylab('Net Exports (% of GDP)') + # set y-axis label
  
  scale_x_continuous(limits = c(2011, 2021), expand = c(0,0.5)) + # set limits and expansion for x-axis
  
  scale_y_continuous(limits = c(0, 50), breaks = seq(0, 50, 10), labels = seq(0, 50, 10)) + # set limits, breaks, and labels for y-axis
  
  xlab('Year') + # set x-axis label again (redundant)
  
  ggtitle("Net Exports (% of GDP) by Country and Year") + # set plot title
  
  labs(subtitle = "South Asia") + # set plot subtitle
  
  theme_minimal() + # set minimal theme
  
  theme(plot.title = element_text(size = 20, face = "bold"), 
  
        plot.subtitle = element_text(size = 20, face = "bold"),
        
        axis.title.x = element_text(size = 14, face = "bold"),
        
        axis.title.y = element_text(size = 14, face = "bold")) + # set theme elements for plot title and axis labels
        
  facet_wrap(~ country, ncol = 2) + # create facets for each country with two columns
  
  guides(color = FALSE, shape = guide_legend(override.aes = list(color = "black"))) # remove color legend and set black color for shape legend
```

**Save the plot to a file**
`ggsave("Net Exports by Country and Year.png")` _**#save in the depository**_

**To ues this code, make sure to replace `Safal` with the name of your dataset, and adjust the section headers and text as appropriate for your project.**

To see the results, check the .jpeg file in this repository. 
