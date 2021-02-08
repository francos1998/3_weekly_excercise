---
title: 'Weekly Exercises #3'
author: "Franco Salinas"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
```

```
## ── Attaching packages ───────────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
## ✓ readr   1.3.1     ✓ forcats 0.5.0
```

```
## ── Conflicts ──────────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday data
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

```
## Parsed with column specification:
## cols(
##   state = col_character(),
##   variable = col_character(),
##   year = col_double(),
##   raw = col_double(),
##   inf_adj = col_double(),
##   inf_adj_perchild = col_double()
## )
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>% 
  mutate(day = wday(date, label = TRUE)) %>% 
  group_by(vegetable, day) %>% 
  summarize(tot_harvest = sum(weight)*0.00220462) %>% 
  pivot_wider(id_cols = vegetable, names_from = day, values_from = tot_harvest)
```

```
## `summarise()` regrouping output by 'vegetable' (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sat"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Sun"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"basil","2":"0.41005932","3":"0.0661386","4":"0.11023100","5":"0.02645544","6":"0.46737944","7":"NA","8":"NA"},{"1":"beans","2":"4.70906832","3":"6.5080382","4":"4.38719380","5":"3.39291018","6":"1.52559704","7":"1.91361016","8":"4.08295624"},{"1":"beets","2":"0.37919464","3":"0.6724091","4":"0.15873264","5":"11.89172028","6":"0.02425082","7":"0.32187452","8":"0.18298346"},{"1":"broccoli","2":"NA","3":"0.8201186","4":"NA","5":"NA","6":"0.16534650","7":"1.25883802","8":"0.70768302"},{"1":"carrots","2":"2.33028334","3":"0.8708249","4":"0.35273920","5":"2.67420406","6":"2.13848140","7":"2.93655384","8":"5.56225626"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"0.01763696"},{"1":"cilantro","2":"0.03747854","3":"NA","4":"0.00440924","5":"NA","6":"0.07275246","7":"NA","8":"NA"},{"1":"corn","2":"1.31615814","3":"0.7583893","4":"0.72752460","5":"NA","6":"3.44802568","7":"1.45725382","8":"5.30211110"},{"1":"cucumbers","2":"9.64080326","3":"4.7752069","4":"10.04645334","5":"3.30693000","6":"7.42956940","7":"3.10410496","8":"5.30652034"},{"1":"edamame","2":"4.68922674","3":"NA","4":"1.40213832","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"hot peppers","2":"NA","3":"1.2588380","4":"0.14109568","5":"NA","6":"NA","7":"NA","8":"0.06834322"},{"1":"jalapeño","2":"1.50796008","3":"5.5534378","4":"0.54895038","5":"0.22487124","6":"1.29411194","7":"0.26234978","8":"0.48060716"},{"1":"kale","2":"1.49032312","3":"2.0679336","4":"0.28219136","5":"0.27998674","6":"0.38139926","7":"0.82673250","8":"0.61729360"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"0.42108242","6":"NA","7":"NA","8":"NA"},{"1":"lettuce","2":"1.31615814","3":"2.4581513","4":"0.91712192","5":"2.45153744","6":"1.80117454","7":"1.46607230","8":"1.18608556"},{"1":"onions","2":"1.91361016","3":"0.5092672","4":"0.70768302","5":"0.60186126","6":"0.07275246","7":"0.26014516","8":"NA"},{"1":"peas","2":"2.85277828","3":"4.6341112","4":"2.06793356","5":"3.39731942","6":"0.93696350","7":"2.05691046","8":"1.08026380"},{"1":"peppers","2":"1.38229674","3":"2.5264945","4":"1.44402610","5":"0.70988764","6":"0.33510224","7":"0.50265336","8":"2.44271896"},{"1":"potatoes","2":"2.80207202","3":"0.9700328","4":"NA","5":"11.85203712","6":"3.74124014","7":"NA","8":"4.57017726"},{"1":"pumpkins","2":"92.68883866","3":"30.1195184","4":"31.85675900","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"radish","2":"0.23148510","3":"0.1962112","4":"0.09479866","5":"0.14770954","6":"0.19400656","7":"0.08157094","8":"NA"},{"1":"raspberries","2":"0.53351804","3":"0.1300726","4":"0.33510224","5":"0.28880522","6":"0.57099658","7":"NA","8":"NA"},{"1":"rutabaga","2":"6.89825598","3":"NA","4":"NA","5":"NA","6":"3.57809826","7":"19.26396956","8":"NA"},{"1":"spinach","2":"0.26014516","3":"0.1477095","4":"0.49603950","5":"0.23368972","6":"0.19621118","7":"0.48722102","8":"0.21384814"},{"1":"squash","2":"56.22221924","3":"24.3345956","4":"18.46810174","5":"NA","6":"NA","7":"NA","8":"NA"},{"1":"strawberries","2":"0.16975574","3":"0.4784025","4":"NA","5":"0.08818480","6":"0.48722102","7":"0.08157094","8":"NA"},{"1":"Swiss chard","2":"0.73413846","3":"1.0736499","4":"0.07054784","5":"2.23107544","6":"0.61729360","7":"1.24781492","8":"0.90830344"},{"1":"tomatoes","2":"35.12621046","3":"11.4926841","4":"48.75076206","5":"34.51773534","6":"85.07628580","7":"75.60964752","8":"58.26590198"},{"1":"zucchini","2":"3.41495638","3":"12.1959578","4":"16.46851140","5":"34.63017096","6":"18.72163304","7":"12.23564100","8":"2.04147812"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
head(garden_planting)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["plot"],"name":[1],"type":["chr"],"align":["left"]},{"label":["vegetable"],"name":[2],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[3],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[5],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[6],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[7],"type":["chr"],"align":["left"]}],"data":[{"1":"A","2":"peas","3":"Super Sugar Snap","4":"22","5":"2020-04-19","6":"TRUE","7":"NA"},{"1":"B","2":"peas","3":"Magnolia Blossom","4":"24","5":"2020-04-19","6":"TRUE","7":"NA"},{"1":"H","2":"onions","3":"Long Keeping Rainbow","4":"40","5":"2020-04-26","6":"FALSE","7":"NA"},{"1":"P","2":"onions","3":"Delicious Duo","4":"25","5":"2020-04-26","6":"FALSE","7":"NA"},{"1":"C","2":"lettuce","3":"Farmer's Market Blend","4":"60","5":"2020-05-02","6":"FALSE","7":"NA"},{"1":"C","2":"radish","3":"Garden Party Mix","4":"20","5":"2020-05-02","6":"FALSE","7":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
garden_harvest %>% 
  group_by(variety) %>% 
  summarize(tot_harvest = sum(weight)*0.00220462)  %>% 
  left_join(garden_planting, by = 'variety' )
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]},{"label":["tot_harvest"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[3],"type":["chr"],"align":["left"]},{"label":["vegetable"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"Amish Paste","2":"65.67342518","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Amish Paste","2":"65.67342518","3":"N","4":"tomatoes","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"asparagus","2":"0.04409240","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Better Boy","2":"34.00846812","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Better Boy","2":"34.00846812","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Big Beef","2":"24.99377694","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Black Krim","2":"15.80712540","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Blue (saved)","2":"41.52401770","3":"A","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Blue (saved)","2":"41.52401770","3":"B","4":"squash","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Bolero","2":"8.29157582","3":"H","4":"carrots","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Bolero","2":"8.29157582","3":"L","4":"carrots","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Bonny Best","2":"24.92322910","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Brandywine","2":"15.64618814","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Bush Bush Slender","2":"22.12997556","3":"M","4":"beans","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Bush Bush Slender","2":"22.12997556","3":"D","4":"beans","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"Catalina","2":"2.03486426","3":"H","4":"spinach","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Catalina","2":"2.03486426","3":"E","4":"spinach","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Cherokee Purple","2":"15.71232674","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Chinese Red Noodle","2":"0.78484472","3":"K","4":"beans","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"Chinese Red Noodle","2":"0.78484472","3":"L","4":"beans","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"potD","4":"cilantro","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"0.11464024","3":"E","4":"cilantro","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Cinderella's Carraige","2":"32.87308882","3":"B","4":"pumpkins","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Classic Slenderette","2":"3.60455370","3":"E","4":"beans","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"Crispy Colors Duo","2":"0.42108242","3":"front","4":"kohlrabi","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"delicata","2":"10.49840044","3":"K","4":"squash","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"Delicious Duo","2":"0.75398004","3":"P","4":"onions","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"Dorinny Sweet","2":"11.40670388","3":"A","4":"corn","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Dragon","2":"4.10500244","3":"H","4":"carrots","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Dragon","2":"4.10500244","3":"L","4":"carrots","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"6.09136506","3":"O","4":"edamame","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Farmer's Market Blend","2":"3.80296950","3":"C","4":"lettuce","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Farmer's Market Blend","2":"3.80296950","3":"L","4":"lettuce","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Garden Party Mix","2":"0.94578198","3":"C","4":"radish","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Garden Party Mix","2":"0.94578198","3":"G","4":"radish","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Garden Party Mix","2":"0.94578198","3":"H","4":"radish","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"giant","2":"9.87228836","3":"L","4":"jalapeño","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"Golden Bantam","2":"1.60275874","3":"B","4":"corn","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Gourmet Golden","2":"7.02171470","3":"H","4":"beets","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"grape","2":"32.39468628","3":"O","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"green","2":"5.69232884","3":"K","4":"peppers","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"green","2":"5.69232884","3":"O","4":"peppers","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"greens","2":"0.37258078","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Heirloom Lacinto","2":"5.94586014","3":"P","4":"kale","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Heirloom Lacinto","2":"5.94586014","3":"front","4":"kale","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Improved Helenor","2":"29.74032380","3":"E","4":"rudabaga","5":"30","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"Isle of Naxos","2":"1.08026380","3":"potB","4":"basil","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"Jet Star","2":"15.02448530","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"King Midas","2":"4.09618396","3":"H","4":"carrots","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"King Midas","2":"4.09618396","3":"L","4":"carrots","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"leaves","2":"0.22266662","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Lettuce Mixture","2":"4.74875148","3":"G","4":"lettuce","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"Long Keeping Rainbow","2":"3.31133924","3":"H","4":"onions","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"Magnolia Blossom","2":"7.45822946","3":"B","4":"peas","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"Main Crop Bravado","2":"2.13186754","3":"D","4":"broccoli","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"Main Crop Bravado","2":"2.13186754","3":"I","4":"broccoli","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"Mortgage Lifter","2":"26.32536742","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"Mortgage Lifter","2":"26.32536742","3":"N","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"mustard greens","2":"0.05070626","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Neon Glow","2":"6.88282364","3":"M","4":"Swiss chard","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"New England Sugar","2":"44.85960776","3":"K","4":"pumpkins","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"Old German","2":"26.71778978","3":"J","4":"tomatoes","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"perrenial","2":"3.18126666","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"pickling","2":"43.60958822","3":"L","4":"cucumbers","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"purple","2":"3.00930630","3":"D","4":"potatoes","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"red","2":"4.43349082","3":"I","4":"potatoes","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"Red Kuri","2":"22.73183682","3":"A","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Red Kuri","2":"22.73183682","3":"B","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Red Kuri","2":"22.73183682","3":"side","4":"squash","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"reseed","2":"0.09920790","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Romanesco","2":"99.70834874","3":"D","4":"zucchini","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"Russet","2":"9.09185288","3":"D","4":"potatoes","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"saved","2":"76.93241952","3":"B","4":"pumpkins","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Super Sugar Snap","2":"9.56805080","3":"A","4":"peas","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"Sweet Merlin","2":"6.38678414","3":"H","4":"beets","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"Tatsoi","2":"2.89466606","3":"P","4":"lettuce","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"thai","2":"0.14770954","3":"potB","4":"hot peppers","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"unknown","2":"0.34392072","3":"NA","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"variety","2":"4.97141810","3":"potA","4":"peppers","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"variety","2":"4.97141810","3":"potA","4":"peppers","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"variety","2":"4.97141810","3":"potC","4":"hot peppers","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"variety","2":"4.97141810","3":"potD","4":"peppers","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"N","4":"tomatoes","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"J","4":"tomatoes","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"front","4":"tomatoes","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"volunteers","2":"51.61235882","3":"O","4":"tomatoes","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"Waltham Butternut","2":"24.27066158","3":"A","4":"squash","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"Waltham Butternut","2":"24.27066158","3":"K","4":"squash","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"yellow","2":"7.40090934","3":"I","4":"potatoes","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"yellow","2":"7.40090934","3":"I","4":"potatoes","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"Yod Fah","2":"0.82011864","3":"P","4":"broccoli","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  

```r
garden_harvest %>% 
  group_by(variety) %>% 
  summarize(tot_harvest = sum(weight)*0.00220462)
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]},{"label":["tot_harvest"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"Amish Paste","2":"65.67342518"},{"1":"asparagus","2":"0.04409240"},{"1":"Better Boy","2":"34.00846812"},{"1":"Big Beef","2":"24.99377694"},{"1":"Black Krim","2":"15.80712540"},{"1":"Blue (saved)","2":"41.52401770"},{"1":"Bolero","2":"8.29157582"},{"1":"Bonny Best","2":"24.92322910"},{"1":"Brandywine","2":"15.64618814"},{"1":"Bush Bush Slender","2":"22.12997556"},{"1":"Catalina","2":"2.03486426"},{"1":"Cherokee Purple","2":"15.71232674"},{"1":"Chinese Red Noodle","2":"0.78484472"},{"1":"cilantro","2":"0.11464024"},{"1":"Cinderella's Carraige","2":"32.87308882"},{"1":"Classic Slenderette","2":"3.60455370"},{"1":"Crispy Colors Duo","2":"0.42108242"},{"1":"delicata","2":"10.49840044"},{"1":"Delicious Duo","2":"0.75398004"},{"1":"Dorinny Sweet","2":"11.40670388"},{"1":"Dragon","2":"4.10500244"},{"1":"edamame","2":"6.09136506"},{"1":"Farmer's Market Blend","2":"3.80296950"},{"1":"Garden Party Mix","2":"0.94578198"},{"1":"giant","2":"9.87228836"},{"1":"Golden Bantam","2":"1.60275874"},{"1":"Gourmet Golden","2":"7.02171470"},{"1":"grape","2":"32.39468628"},{"1":"green","2":"5.69232884"},{"1":"greens","2":"0.37258078"},{"1":"Heirloom Lacinto","2":"5.94586014"},{"1":"Improved Helenor","2":"29.74032380"},{"1":"Isle of Naxos","2":"1.08026380"},{"1":"Jet Star","2":"15.02448530"},{"1":"King Midas","2":"4.09618396"},{"1":"leaves","2":"0.22266662"},{"1":"Lettuce Mixture","2":"4.74875148"},{"1":"Long Keeping Rainbow","2":"3.31133924"},{"1":"Magnolia Blossom","2":"7.45822946"},{"1":"Main Crop Bravado","2":"2.13186754"},{"1":"Mortgage Lifter","2":"26.32536742"},{"1":"mustard greens","2":"0.05070626"},{"1":"Neon Glow","2":"6.88282364"},{"1":"New England Sugar","2":"44.85960776"},{"1":"Old German","2":"26.71778978"},{"1":"perrenial","2":"3.18126666"},{"1":"pickling","2":"43.60958822"},{"1":"purple","2":"3.00930630"},{"1":"red","2":"4.43349082"},{"1":"Red Kuri","2":"22.73183682"},{"1":"reseed","2":"0.09920790"},{"1":"Romanesco","2":"99.70834874"},{"1":"Russet","2":"9.09185288"},{"1":"saved","2":"76.93241952"},{"1":"Super Sugar Snap","2":"9.56805080"},{"1":"Sweet Merlin","2":"6.38678414"},{"1":"Tatsoi","2":"2.89466606"},{"1":"thai","2":"0.14770954"},{"1":"unknown","2":"0.34392072"},{"1":"variety","2":"4.97141810"},{"1":"volunteers","2":"51.61235882"},{"1":"Waltham Butternut","2":"24.27066158"},{"1":"yellow","2":"7.40090934"},{"1":"Yod Fah","2":"0.82011864"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
garden_spending 
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["brand"],"name":[3],"type":["chr"],"align":["left"]},{"label":["eggplant_item_number"],"name":[4],"type":["chr"],"align":["left"]},{"label":["price"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["price_with_tax"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"beans","2":"Bush Bush Slender","3":"Renee's Garden","4":"2156","5":"2.790000","6":"3.009713"},{"1":"beans","2":"Chinese Red Noodle","3":"Baker Creek","4":"2138","5":"3.000000","6":"3.236250"},{"1":"beans","2":"Classic Slenderette","3":"Renee's Garden","4":"2157","5":"2.990000","6":"3.225463"},{"1":"beets","2":"Gourmet Golden","3":"Renee's Garden","4":"1018","5":"3.190000","6":"3.441213"},{"1":"beets","2":"Sweet Merlin","3":"Renee's Garden","4":"2114","5":"2.990000","6":"3.225463"},{"1":"broccoli","2":"Yod Fah","3":"Baker Creek","4":"37097","5":"3.000000","6":"3.236250"},{"1":"broccoli","2":"Main Crop Bravado","3":"Renee's Garden","4":"7163","5":"3.190000","6":"3.441213"},{"1":"brussels sprouts","2":"Long Island","3":"Seed Savers","4":"3296","5":"3.250000","6":"3.505938"},{"1":"carrots","2":"Bolero","3":"Renee's Garden","4":"2160","5":"2.990000","6":"3.225463"},{"1":"carrots","2":"Dragon","3":"Seed Savers","4":"1581","5":"3.250000","6":"3.505938"},{"1":"carrots","2":"King Midas","3":"Renee's Garden","4":"2337","5":"2.790000","6":"3.009713"},{"1":"corn","2":"Golden Bantam","3":"Seed Savers","4":"1364","5":"3.250000","6":"3.505938"},{"1":"corn","2":"Dorinny Sweet","3":"Baker Creek","4":"6134","5":"3.500000","6":"3.775625"},{"1":"cucumbers","2":"pickling","3":"Renee's Garden","4":"2169","5":"2.790000","6":"3.009713"},{"1":"edamame","2":"edamame","3":"Renee's Garden","4":"1012","5":"3.190000","6":"3.441213"},{"1":"kale","2":"Heirloom Lacinto","3":"Renee's Garden","4":"1058","5":"2.790000","6":"3.009713"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"Renee's Garden","4":"1059","5":"3.290000","6":"3.549088"},{"1":"lettuce","2":"Farmer's Market Blend","3":"Renee's Garden","4":"1067","5":"2.790000","6":"3.009713"},{"1":"lettuce","2":"Tatsoi","3":"Seed Savers","4":"3334","5":"3.250000","6":"3.505938"},{"1":"nasturtium","2":"Alaska Mix","3":"Renee's Garden","4":"1080","5":"2.790000","6":"3.009713"},{"1":"onions","2":"Long Keeping Rainbow","3":"Renee's Garden","4":"6599","5":"3.290000","6":"3.549088"},{"1":"onions","2":"Delicious Duo","3":"Renee's Garden","4":"2185","5":"2.990000","6":"3.225463"},{"1":"peas","2":"Magnolia Blossom","3":"Renee's Garden","4":"6601","5":"2.990000","6":"3.225463"},{"1":"peas","2":"Super Sugar Snap","3":"Renee's Garden","4":"1090","5":"2.790000","6":"3.009713"},{"1":"pumpkins","2":"Big Max","3":"Baker Creek","4":"7396","5":"3.000000","6":"3.236250"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"Renee's Garden","4":"2193","5":"2.990000","6":"3.225463"},{"1":"pumpkins","2":"New England Sugar","3":"Baker Creek","4":"6244","5":"2.500000","6":"2.696875"},{"1":"radish","2":"Garden Party Mix","3":"Renee's Garden","4":"2083","5":"2.990000","6":"3.225463"},{"1":"rutabaga","2":"Improved Helenor","3":"Renee's Garden","4":"6604","5":"2.990000","6":"3.225463"},{"1":"spinach","2":"Catalina","3":"Renee's Garden","4":"1102","5":"2.990000","6":"3.225463"},{"1":"squash","2":"Red Kuri","3":"Baker Creek","4":"1106","5":"3.500000","6":"3.775625"},{"1":"squash","2":"Waltham Butternut","3":"Seed Savers","4":"1421","5":"3.250000","6":"3.505938"},{"1":"sunflower","2":"Autumn Beauty","3":"Baker Creek","4":"37067","5":"2.500000","6":"2.696875"},{"1":"sunflower","2":"Giant Titan","3":"Renee's Garden","4":"1111","5":"2.990000","6":"3.225463"},{"1":"swiss chard","2":"Neon Glow","3":"Renee's Garden","4":"1037","5":"2.990000","6":"3.225463"},{"1":"watermelon","2":"Doll Babies","3":"Renee's Garden","4":"5224","5":"3.990000","6":"4.304213"},{"1":"zucchini","2":"Romanesco","3":"Renee's Garden","4":"2204","5":"2.990000","6":"3.225463"},{"1":"dirt","2":"Seed Starter","3":"__NA__","4":"3980","5":"11.990000","6":"12.934213"},{"1":"cilantro","2":"cilantro","3":"Seed Savers","4":"__NA__","5":"3.250000","6":"3.505938"},{"1":"dill","2":"Grandma Einck's","3":"Seed Savers","4":"__NA__","5":"3.250000","6":"3.505938"},{"1":"basil","2":"Isle of Naxos","3":"Seed Savers","4":"__NA__","5":"3.250000","6":"3.505938"},{"1":"enriched topsoil","2":"1yd","3":"Kern","4":"__NA__","5":"50.000000","6":"53.937500"},{"1":"raised garden blend","2":"1yd","3":"Kern","4":"__NA__","5":"78.000000","6":"84.142500"},{"1":"tomatoes","2":"Mortgage Lifter","3":"farmer's market","4":"__NA__","5":"3.090139","6":"3.333488"},{"1":"tomatoes","2":"Old German","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"tomatoes","2":"Better Boy","3":"farmer's market","4":"__NA__","5":"3.089996","6":"3.333333"},{"1":"tomatoes","2":"Amish Paste","3":"farmer's market","4":"__NA__","5":"3.089996","6":"3.333333"},{"1":"tomatoes","2":"Cherokee Purple","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"tomatoes","2":"Brandywine","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"tomatoes","2":"Bonny Best","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"tomatoes","2":"Big Beef","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"tomatoes","2":"Black Krim","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"tomatoes","2":"Jet Star","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"tomatoes","2":"grape","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"lettuce","2":"Lettuce Mixture","3":"Seed Savers","4":"__NA__","5":"3.250000","6":"3.505938"},{"1":"jalapeño","2":"giant","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"jalapeño","2":"giant","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"jalapeño","2":"giant","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"peppers","2":"green","3":"Naomi","4":"__NA__","5":"0.000000","6":"0.000000"},{"1":"peppers","2":"variety","3":"Naomi","4":"__NA__","5":"0.000000","6":"0.000000"},{"1":"hot peppers","2":"variety","3":"Adrienne","4":"__NA__","5":"0.000000","6":"0.000000"},{"1":"peppers","2":"thai","3":"farmer's market","4":"__NA__","5":"1.544998","6":"1.666667"},{"1":"potatoes","2":"purple","3":"mine","4":"__NA__","5":"0.000000","6":"0.000000"},{"1":"potatoes","2":"yellow","3":"leftover","4":"__NA__","5":"0.000000","6":"0.000000"},{"1":"potatoes","2":"red","3":"leftover","4":"__NA__","5":"0.000000","6":"0.000000"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
garden_planting%>% 
  group_by(variety) %>% 
  summarize(tot_seeds = sum(number_seeds_planted))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]},{"label":["tot_seeds"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"Amish Paste","2":"3"},{"1":"Better Boy","2":"2"},{"1":"Big Beef","2":"1"},{"1":"Big Max","2":"6"},{"1":"Black Krim","2":"1"},{"1":"Blue (saved)","2":"12"},{"1":"Bolero","2":"100"},{"1":"Bonny Best","2":"1"},{"1":"Brandywine","2":"1"},{"1":"Bush Bush Slender","2":"40"},{"1":"Butternut (saved)","2":"8"},{"1":"Catalina","2":"150"},{"1":"Cherokee Purple","2":"1"},{"1":"Chinese Red Noodle","2":"10"},{"1":"cilantro","2":"35"},{"1":"Cinderalla's Carraige","2":"9"},{"1":"Cinderella's Carraige","2":"3"},{"1":"Classic Slenderette","2":"29"},{"1":"Crispy Colors Duo","2":"10"},{"1":"delicata","2":"8"},{"1":"Delicious Duo","2":"25"},{"1":"Doll Babies","2":"8"},{"1":"Dorinny Sweet","2":"20"},{"1":"Dragon","2":"90"},{"1":"edamame","2":"25"},{"1":"Farmer's Market Blend","2":"120"},{"1":"Garden Party Mix","2":"65"},{"1":"giant","2":"4"},{"1":"Golden Bantam","2":"20"},{"1":"Gourmet Golden","2":"40"},{"1":"Grandma Einck's","2":"40"},{"1":"grape","2":"1"},{"1":"green","2":"17"},{"1":"Heirloom Lacinto","2":"60"},{"1":"honeydew","2":"5"},{"1":"Improved Helenor","2":"30"},{"1":"Isle of Naxos","2":"40"},{"1":"Jet Star","2":"1"},{"1":"King Midas","2":"100"},{"1":"Lettuce Mixture","2":"200"},{"1":"Long Island","2":"13"},{"1":"Long Keeping Rainbow","2":"40"},{"1":"Magnolia Blossom","2":"24"},{"1":"Main Crop Bravado","2":"14"},{"1":"Mortgage Lifter","2":"2"},{"1":"Neon Glow","2":"25"},{"1":"New England Sugar","2":"4"},{"1":"Old German","2":"1"},{"1":"perennial","2":"NA"},{"1":"pickling","2":"20"},{"1":"purple","2":"5"},{"1":"red","2":"3"},{"1":"Red Kuri","2":"9"},{"1":"Romanesco","2":"3"},{"1":"Russet","2":"8"},{"1":"saved","2":"8"},{"1":"Super Sugar Snap","2":"22"},{"1":"Sweet Merlin","2":"40"},{"1":"Tatsoi","2":"25"},{"1":"thai","2":"1"},{"1":"variety","2":"13"},{"1":"volunteers","2":"9"},{"1":"Waltham Butternut","2":"10"},{"1":"yellow","2":"18"},{"1":"Yod Fah","2":"25"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  
  
>I would group the observations on the garden harvest data by variety and summarize their total weight in pounds. 
I would then left join the garden harvest data and the garden spending data. 
Then I would group by variety the garden planting data and summarize to show the total number of seeds per variety. 
Then I would left joing the new table with the other two. 
Then I would gruop the store data by variety and transform the names of the observations in order for R to match the varieties with their correct price per pound. 
Then I would left join it to the other tables. 
I would also create another variable called net costs that would multiply the amount
of seeds used and their price plus taxes. 
After, I would create a new variable called gross savings that multiply the total harvest weight by the price from the data set. 
Then I would sum the values of the gross savings variable. And also separately sum the values of the net costs. Then I would find the difference between both to find the savings. 

  

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  mutate(variety2 = fct_reorder(variety, date, .fun = min)) %>% 
  group_by(variety2) %>% 
  summarise(date, variety2,
            weight, 
            tot_harvest = sum(weight)*0.00220462) %>% 
  ggplot(aes(y = fct_rev(variety2), x = tot_harvest)) +
  geom_col()
```

```
## `summarise()` regrouping output by 'variety2' (override with `.groups` argument)
```

![](03_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. 
Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(varlower = str_to_lower(variety),
         lengthvar = str_length(variety)) %>%
  group_by(vegetable) %>% 
  arrange(lengthvar) %>% 
  distinct(varlower)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["varlower"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"potatoes","2":"red"},{"1":"hot peppers","2":"thai"},{"1":"tomatoes","2":"grape"},{"1":"jalapeño","2":"giant"},{"1":"peppers","2":"green"},{"1":"pumpkins","2":"saved"},{"1":"lettuce","2":"reseed"},{"1":"beets","2":"leaves"},{"1":"lettuce","2":"tatsoi"},{"1":"carrots","2":"dragon"},{"1":"carrots","2":"bolero"},{"1":"potatoes","2":"purple"},{"1":"potatoes","2":"yellow"},{"1":"carrots","2":"greens"},{"1":"potatoes","2":"russet"},{"1":"hot peppers","2":"variety"},{"1":"peppers","2":"variety"},{"1":"broccoli","2":"yod fah"},{"1":"edamame","2":"edamame"},{"1":"apple","2":"unknown"},{"1":"spinach","2":"catalina"},{"1":"cilantro","2":"cilantro"},{"1":"cucumbers","2":"pickling"},{"1":"tomatoes","2":"big beef"},{"1":"tomatoes","2":"jet star"},{"1":"squash","2":"delicata"},{"1":"squash","2":"red kuri"},{"1":"chives","2":"perrenial"},{"1":"strawberries","2":"perrenial"},{"1":"asparagus","2":"asparagus"},{"1":"Swiss chard","2":"neon glow"},{"1":"raspberries","2":"perrenial"},{"1":"zucchini","2":"romanesco"},{"1":"tomatoes","2":"bonny best"},{"1":"carrots","2":"king midas"},{"1":"tomatoes","2":"better boy"},{"1":"tomatoes","2":"old german"},{"1":"tomatoes","2":"brandywine"},{"1":"tomatoes","2":"black krim"},{"1":"tomatoes","2":"volunteers"},{"1":"tomatoes","2":"amish paste"},{"1":"beets","2":"sweet merlin"},{"1":"squash","2":"blue (saved)"},{"1":"basil","2":"isle of naxos"},{"1":"onions","2":"delicious duo"},{"1":"corn","2":"dorinny sweet"},{"1":"corn","2":"golden bantam"},{"1":"lettuce","2":"mustard greens"},{"1":"beets","2":"gourmet golden"},{"1":"lettuce","2":"lettuce mixture"},{"1":"tomatoes","2":"cherokee purple"},{"1":"tomatoes","2":"mortgage lifter"},{"1":"radish","2":"garden party mix"},{"1":"kale","2":"heirloom lacinto"},{"1":"peas","2":"magnolia blossom"},{"1":"peas","2":"super sugar snap"},{"1":"rutabaga","2":"improved helenor"},{"1":"beans","2":"bush bush slender"},{"1":"broccoli","2":"main crop bravado"},{"1":"kohlrabi","2":"crispy colors duo"},{"1":"squash","2":"waltham butternut"},{"1":"pumpkins","2":"new england sugar"},{"1":"beans","2":"chinese red noodle"},{"1":"beans","2":"classic slenderette"},{"1":"onions","2":"long keeping rainbow"},{"1":"lettuce","2":"farmer's market blend"},{"1":"pumpkins","2":"cinderella's carraige"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  mutate(ar_er = str_detect(variety, "ar|er")) %>% 
  filter(ar_er == TRUE) %>% 
  distinct(variety)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["variety"],"name":[1],"type":["chr"],"align":["left"]}],"data":[{"1":"Garden Party Mix"},{"1":"Farmer's Market Blend"},{"1":"Super Sugar Snap"},{"1":"perrenial"},{"1":"asparagus"},{"1":"mustard greens"},{"1":"Bush Bush Slender"},{"1":"Sweet Merlin"},{"1":"variety"},{"1":"Cherokee Purple"},{"1":"Better Boy"},{"1":"Mortgage Lifter"},{"1":"Old German"},{"1":"Jet Star"},{"1":"Bolero"},{"1":"volunteers"},{"1":"Classic Slenderette"},{"1":"Cinderella's Carraige"},{"1":"Waltham Butternut"},{"1":"New England Sugar"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){300px}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){300px}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data-Small.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## Parsed with column specification:
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(x = sdate)) +
  geom_density()
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(hour = hour(sdate),
         minute = minute(sdate)*1/60,
         time_day = hour+minute) %>% 
  ggplot(aes(x = time_day)) +
  geom_density()
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>%
  mutate(week_day = wday(sdate, label = TRUE)) %>% 
  ggplot(aes(y = week_day))+
  geom_bar()+
  labs(y = '')
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  


  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

  
### Spatiotemporal patterns

  17. Make a table with the ten station-date combinations (e.g., 14th & V St., 2014-10-14) with the highest number of departures, sorted from most departures to fewest. Save this to a new dataset and print out the dataset. Hint: `as_date(sdate)` converts `sdate` from date-time format to date format. 
  

  
  18. Use a join operation to make a table with only those trips whose departures match those top ten station-date combinations from the previous part.
  

  
  19. Build on the code from the previous problem (ie. copy that code below and then %>% into the next step.) and group the trips by client type and day of the week (use the name, not the number). Find the proportion of trips by day within each client type (ie. the proportions for all 7 days within each client type add up to 1). Display your results so day of week is a column and there is a column for each client type. Interpret your results.

**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
