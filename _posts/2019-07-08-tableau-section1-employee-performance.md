---
title: "Tableau: Comparing Employee Performance on a Test Dataset"
categories:
  - Firecode
tags:
  - firecode
  - python
---
Using a given dataset I plotted to compare employee performance based on total sale numbers.  The dataset can be found [here](https://sds-platform-private.s3-us-east-2.amazonaws.com/uploads/P1-OfficeSupplies.csv).

![Tableau Plot](https://i.imgur.com/9htdePz.png "Employee Performance")

## Explanation

* I created a calculated field for total sales by multiplying total units sold and unit value for each salesman.
* I plotted the salesman against each other to compare by region, using the Region as the first column followed by rep(salesman).
* I used the calculated field as the row value and colored based on the region to visualize differences in salesman for regions.
* Created labels to show the exact money value for each rep above the bars and also displayed using dollar amounts and in K(with one decimal) and formatted the chart, axis', and titles.
