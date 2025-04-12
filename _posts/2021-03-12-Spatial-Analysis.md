---
layout: post
title: "Data Science - Spatial Analysis"
description: "Understood social distancing metrics from SafeGraph by visualizing multidimensional data."
categories: [DataScience]
tags: [Spatial Analysis, Data Visualization]
redirect_from:
  - /2021/03/12/
---

## Data Science - Spatial Analysis



SafeGraph (SG) collected the [location data](https://docs.safegraph.com/docs/social-distancing-metrics) to track changes in social distancing behavior during the COVID-19 pandemic. This blog explores the use of space-time pattern mining tools to visualize and analyze travel patterns in California, focusing on behaviors such as staying at home, delivery driver activity, and work patterns. By leveraging spatial analysis techniques, we can gain insights into the impacts of the pandemic on human mobility and social distancing.

#### Map 1: % devices with a stay at home behavior
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116829822-20378e80-ab74-11eb-9626-63efdd85a5d1.png">

- The lighter blue areas in map one indicate lower percentages of the sampled devices showing the pattern described as stay-at-home behavior.

In the data clocks below, each data clocks' concentric circle represents a week, while each circle segment represents a day of the week. The date range (May 1, 2020, to June 15, 2020) begins in the center and expands outward. The color of each wedge represents the variable value. The data clocks represent the mean value of the percentage of devices with stay-at-home behavior across all the block groups.

#### Figure 1: Change in mean % devices with a stay at home behavior by Days over Weeks
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116827615-e6608b00-ab67-11eb-81ec-7d75469d58a6.png">

- Figure one shows that Sunday tends to be a popular day for staying at home.

#### Figure 2: Change in mean % devices with a delivery driver behavior by Days over Weeks
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116827640-10b24880-ab68-11eb-8819-74fed7e59304.png">

- Figure two shows an increase in delivery activity on Friday. It can be that this is a popular day to order delivery takeout and groceries.

#### Figure 3: Change in mean % devices with full-time worker behavior by Days over Weeks
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116827648-245daf00-ab68-11eb-8a17-e77cea949780.png">

- Figure three shows that for devices with full-time worker behavior, Monday, Tuesday, Wednesday, Thursday, and Friday are peak activity days.

#### Figure 4: Change in mean % devices with part-time worker behavior by Days over Weeks
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116827685-5a9b2e80-ab68-11eb-8df6-53e111bb5992.png">

- Figure four shows that for devices with part-time worker behavior. The percentage values are between 5% and 7.7%, which is relatively low but higher than full-time workers.

The line chart below shows the change in the % devices with a stay-at-home behavior variable from May 1, 2020, to June 14, 2020. % of devices with a stay-at-home behavior goes down over the date range, as well as a cyclic, weekly pattern.

#### Figure 5: % devices with a stay at home behavior
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116827755-b4035d80-ab68-11eb-8cde-2bd1894693a6.png">

The visualizations of the emerging hot spot analysis  identify trends in the values of the space-time cube. The analysis categorizes the data in the spatial bins classifying what is occurring over time. 

#### Map 2: PctHome_EmergingHotSpotAnalysis
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116827902-9aaee100-ab69-11eb-9e12-6eabd1f8eacf.png">

- Map two shows a preponderance of the diminishing hot spot pattern in the San Francisco area. These cells have been statistically significant hot spots for 90 percent of the time-step intervals, including the final time step. In addition, the intensity of clustering in each time step decreases overall, and this decrease is statistically significant.

#### Map 3: PctFulltime_EmergingHotSpotAnalysis
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116827947-cd58d980-ab69-11eb-8ffa-be698201528b.png">

- Areas east of Los Angeles show both oscillating cold spots and sporadic cold spots. The oscillating cold spot east of Los Angeles has a statistically significant cold spot for the final time-step interval, but it has been a hot spot in prior intervals. The sporadic cold spot to the east of that area indicates that there are no periods that are statistically significant hot spots, and less than 90% of the time-step intervals have been statistically significant cold spots.

The 3D chart visualizes a variable in the multidimensional space-time cube. The earlier temporal bins are at the bottom, and more recent temporal bins are at the top.

#### Figure 6: 3D - PCTHOME_MEAN_SPATIAL_NEIGHBORS
<img width="756" alt="X111" src="https://user-images.githubusercontent.com/76184559/116828067-6b4ca400-ab6a-11eb-9b29-f126e421e94d.png">

- Figure six shows the location has been a trend to less stay-at-home behavior if a column starts dark blue and gradually transitions to lighter blues, pinks, and dark red.

### Conclusion
These visualizations demonstrate the significant impact of the COVID-19 pandemic on travel patterns and social distancing behavior in California. By using space-time pattern mining tools, we can better understand regional trends and the evolving nature of public response to the pandemic. Future work could extend this analysis to explore similar patterns in other states and regions, offering a more comprehensive view of mobility changes during the pandemic.
