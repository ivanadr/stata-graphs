# Welcome
Welcome to my page where I showcase graphs I did in Stata for my data analyses project about abortion attitudes in Slovakia.  
Using [data](https://doi.org/10.4232/1.13441) from the International Social Survey Programme, my aim was to:  
- analyze the level of abortion disapproval by the degree of religiosity  
- investigate differences in attitudes between those of no religion and those affiliated with a Christian religion

## About me
My name is Ivana Drabova, I'm a rising junior at NYUAD, majoring in Economics and minoring in Computer Science.


# Pie chart with the explode option
With title, and intensity change  
**Code:** `graph pie, over (*varname*) plabel(_all percent, color(black))  pie(5,explode color(black)) scheme(burd) title(Religious Landscape in Slovakia) intensity(50)`
![Image](/graph-1.png)

# Regression line with data point visualization in the background 
- **points in dark green:** Visualization of respondents' affiliation the Christian religion
- **Line in light green:** predicted values of abortion attitude by the two groups with 95% confidence levels
 
 **Code**  
`marginsplot, recast(line) plot1opts(lcolor(green))   ciopt(color(green%20)) recastci(rarea) legend(off) title(Model 1: Abortion attitude if family has very low income) subtitle(Predicted values with 95% confidence levels) addplot(scatter abortionDisapproval isChristian, mcolor(teal) xlab(-.25 " " 0 "No Religion" 1 "Christian" 1.25 " ", notick) below jitter(5))  yline(0) yline(1) yline(2) yline(3) ytitle("") scheme(burd) ylabel(0 "Not wrong at all" 1 " Wrong only sometimes" 2 "Almost always wrong" 3 "Always wrong",labsize(small)) xtitle("")`
![Image](/graph-5.png)


# Bar graph with guiding lines
With title, subtitle, ylabel change, implicit ylabel values to display with guiding lines   
 **Code**  
 `graph bar *yvarname*, over(*binary variable on x axis name* ,label(angle(0))) title(Average Abortion Attitude by religion) subtitle(if family has very low income) ylabel(0 "Not wrong at all" 1 " Wrong only sometimes" 2 "Almost always wrong" 3 "Always wrong",labsize(small)) yline(0) yline(1) yline(2) yline(3) ytitle("") bar(1, color( "178 24 43")) note("Note: Christian Sub-groups within the 'Other' category were included") blabel(bar, position(outside) format(%9.2f) )`
![Image](/graph-3.png)

# Frequency bar graph and contract command demonstration
**creating new .dta file and using it within the old .dta file**
It was necessary to use the `contract ` command and export to new .dta file. However, I did *not* want to open a new stata window to work with the new .dta file. So it was necessary to come back to original .dta file with commands save and use as demonstrated below.  

```
save *original-dta-file*, replace
contract * varname1* * varname2* , freq(newf) percent(newp) nomiss zero
save *name-of-the-exported-dta*, replace

use *name-of-the-exported-dta*, clear
graph bar (asis) newp, over(*varname*) over(*varname2*,label(angle(45)))  asyvars bar(1) bar(2) bargap(10) title(Abortion Attitude Shares by Religiosity) subtitle() note("Note: percentages represented by each bar are calculated from total population") blabel(bar, position(outside) format(%9.0f) ) scheme(burd4)
//ysize(5)  graphregion(margin(large))
graph export "graph collated 2 percentage religio abortionDisapproval.png", replace
use *original-dta-file*, clear
```
![Image](/graph-9.png)


# Histogram
With dicrete option, title and xlabel modification  
`hist *varname*, percent discrete start(75) xlabel(0(500)4000) title(Household Income in EUR) `
![Image](/graph-2.png)

# Bar graph with multiple sub-categories
In percentages
 **Code**  
`graph bar abortionDisapproval, over(top4Religions,label(angle(45))) title(Average Abortion Attitude by religious groups) subtitle(if family has very low income) ylabel(0 "Not wrong at all" 1 " Wrong only sometimes" 2 "Almost always wrong" 3 "Always wrong",labsize(small)) yline(0) yline(1) yline(2) yline(3) ytitle("") bar(1, color( "178 24 43")) note("Note: Categories of size less than 1% of total population were omitted")`
![Image](/graph-7.png)


# Bar graph with multiple categories 
 **Code**  
`graph bar abortionDisapproval, over(top4Religions,label(angle(45))) title(Average Abortion Attitude by religious groups) subtitle(if family has very low income) ylabel(0 "Not wrong at all" 1 " Wrong only sometimes" 2 "Almost always wrong" 3 "Always wrong",labsize(small)) yline(0) yline(1) yline(2) yline(3) ytitle("") bar(1, color( "178 24 43")) note("Note: Categories of size less than 1% of total population were omitted")
graph export "graph bar average abortion attitude by top 4 religion.png", replace`
![Image](/graph-4.png)

## Acknowledgements
**Data source:**    
ISSP Research Group (2020): International Social Survey Programme: Religion IV - ISSP 2018. GESIS Data Archive, Cologne. ZA7570 Data file Version 1.0.0, https://doi.org/10.4232/1.13441  

**Stata graph scheme:** [Burd](https://github.com/briatte/burd)
