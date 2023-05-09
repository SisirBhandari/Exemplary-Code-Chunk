# R-Code with explanation

Necessary packages and libraries
`install necessary packages
install.packages("tidyverse")
install.packages("countrycode")`
library(tidyverse)
library(countrycode)`


## Read in the data
Safal <- read.csv("Safal.csv")

## Convert country names to three-letter ISO codes
Safal$code <- countrycode(sourcevar = Safal$country, origin = "country.name", destination = "iso3c")

## Remove observations with missing data
Safal <- na.omit(Safal)

## Plot the data using ggplot2
`ggplot(Safal, aes(year, NE.EXP.GNFS.ZS, color=country, shape = country)) +

  geom_line() +
  
  geom_point(data = Safal %>% group_by(country) %>% 
  
               filter(NE.EXP.GNFS.ZS == max(NE.EXP.GNFS.ZS, na.rm = TRUE)),
               
             size = 3, shape = 22) +
             
  geom_text(data = Safal %>% group_by(country) %>% 
  
              filter(NE.EXP.GNFS.ZS == max(NE.EXP.GNFS.ZS, na.rm = TRUE)), aes(label = round(NE.EXP.GNFS.ZS, 2),
              
    x = year, y = NE.EXP.GNFS.ZS + 0.5), size = 3, vjust = -0.75, hjust = -0.5, size = 3, show.legend = FALSE) +
    
  scale_shape_manual(values = c(19, 17, 15, 13, 11, 9, 7, 5)) + #to change the manual shapes
  
  xlab('Year') + 
  
  ylab('Net Exports (% of GDP)') +
  
  scale_x_continuous(limits = c(2011, 2021), expand = c(0,0.5)) +
  
  scale_y_continuous(limits = c(0, 50), breaks = seq(0, 50, 10), labels = seq(0, 50, 10)) +
  
  xlab('Year') + 
  
  ggtitle("Net Exports (% of GDP) by Country and Year") +
  
  labs(subtitle = "South Asia") +
  
  theme_minimal() +
  
  theme(plot.title = element_text(size = 20, face = "bold"), 
  
        plot.subtitle = element_text(size = 20, face = "bold"),
        
axis.title.x = element_text(size = 14, face = "bold"),

axis.title.y = element_text(size = 14, face = "bold")) +

facet_wrap(~ country, ncol = 2) +

guides(color = FALSE, shape = guide_legend(override.aes = list(color = "black")))`

## Save the plot to a file
ggsave("Net Exports by Country and Year.png")
