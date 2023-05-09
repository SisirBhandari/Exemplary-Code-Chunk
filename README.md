# Exemplary-Code-Chunk
This shows my ability to code by providing an example of coding with output posted. 

# Install necessary packages
install.packages("tidyverse")
install.packages("countrycode")

# Load necessary libraries
library(tidyverse)
library(countrycode)

# Read in the data
Safal <- read.csv("Safal.csv")

# Convert country names to three-letter ISO codes
Safal$code <- countrycode(sourcevar = Safal$country, origin = "country.name", destination = "iso3c")

# Remove observations with missing data
Safal <- na.omit(Safal)

# Plot the data using ggplot2
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

# Save the plot to a file
ggsave("Net Exports by Country and Year.png")

