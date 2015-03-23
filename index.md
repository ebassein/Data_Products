---
title       : Devloping Data Products Class Project
subtitle    : Visualizing and Manipulating HVAC Data
author      : Emma Bassein
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax, quiz]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

--- .class #id 

## Problem Statement

A large part of energy efficiency engineering is analyzing the data from air conditioning units (otherwise known as air handling units or RTUs). Because operation is different at different times of the day and different days of the week, it is imperative to be able to quickly filter and subset the data you are looking at to identify problems.


--- .class #id

## Data Definitions
The key data being visualize are all temperatures.

#### SAT: Supply Air Temperature.
- This is the temperature of conditioned air being supplied to the space.

#### RAT: Return Air Temperature.
- This is the temperature of the air being recycled from the occupied space. It should be about the same as your space set points, so probably between 68 and 72 degrees.

#### OAT: Outside Air Temperature. (self explanatory)

#### MAT: Mixed Air Temperature.
- The unit uses something called an economizer to modulate the percentage of return air and supply air that is mixed together before going through the cooling coils. The economizer's goal is to get the MAT as close to SAT as possible to minimize the work on the refrigerant cycle.



--- .class #id

## App Features

The app lets you select data from one of two different AHUs. It then graphs a full time series of the data. 

The second graph lets you select a specific time range to zoom in on using the date inputs in the side bar

Graph 3 is a plot of all of the variables vs. OAT, and you can filter by day of the week and hour of the day in the side bar.

The fourth graph allows you to plot any two variables against each other and provides the equation for best fit lines. Because many of the trend have a kink in them, I gave the option to break the trend line at a specific OAT so that two different fits could be used.

--- .class #id

## R Features

#### Filtering
One of the key features is being able to filter by date, time of day and day of week. Here is an example of the code that does that.


```r
  output$subsetChart <- renderPlot({
		data = melt(datasetInput(), id="dttm")
		graphdata <- data[data$dttm > as.POSIXct(startInput(),format = "%Y-%m-%d"),]
		graphdata <- graphdata[graphdata$dttm < as.POSIXct(endInput(), format = "%Y-%m-%d"),]
		p2 <- ggplot(graphdata, aes(dttm, value, color = variable)) + geom_line()
			#geom_line(aes(y = SAT, col = "SAT")) + geom_line(aes(y = MAT, col = MAT)),
		return(p2)
	})
```

```
## Error in output$subsetChart <- renderPlot({: object 'output' not found
```

--- .class #id

## Future Direction

There are several improvements that could be made to the is app, including:

- Including OA% directly, which can be calculated using the following equation:

OA% = $\frac{OAT-RAT}{MAT-RAT}$

It should be noted that this will become assymptotic as MAT -> RAT

- Better layout to align filter options with the appropriate graphs.


