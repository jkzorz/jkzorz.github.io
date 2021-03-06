---
layout: post
title: "Beginner R Tips"
date: 2020-05-18
---


If you’re new to R and coding, everything can seem very daunting at the beginning. Even the process of asking for help and finding answers online can be a struggle, as often you don’t even know the right questions to google. When you do find answers, they’re usually disjointed, buried in the depths of internet forums filled with barely decipherable jargon. It’s frustrating, and sometimes quite a bit disheartening. It gets better, sometimes very slowly, but it does get better. You’ll start to recognize errors and you’ll have bits of code that you can reuse to accomplish tasks much faster than before.  I’ve compiled here some tips and common errors and solutions that I would have found helpful at the beginning of my R journey. 

## Basic things ## 
**Keep good notes!**

Err on the side of writing notes in too much detail.  You’ll be amazed at how quickly you’ll forget why and how you did things. For me it’s sometimes only a matter of hours. Use some sort of text editor program like RStudio’s script window, or Notepad++, or Jupyter or whatever. I often use basic Notepad because I am a lazy dinosaur (and not a programmer) and it’s fast to access. **Do not use Microsoft Word because it introduces subtle formatting that will mess up your code (for instance the quotes are different and don't translate)**.  

A useful way to embed notes to yourself and others in your code is the *hash* symbol “#”. You may recognize the hashtag because it was borrowed by social media for its own purposes *#blessed*. Anything you add after the hash will not be executable code when copied into R. Here’s an example 

```
#This is a note, and not code
``` 

**Beginner courses**

Depending on your needs, a beginner R course might be a good investment if you think you’ll be using the platform a lot. Courses tend to present topics in the proper order and with the proper background. My intro to R was mostly randomly googling and reading about the things I needed at the time. But in retrospect, I think I would have benefited from a cohesive intro course to get me started.  

**Jargon**

I despise jargon. I try to avoid using it as much as possible, partly because it feels pretentious and is a barrier to learning, and partly because I’m still not very good at or interested in terminology and semantics. That being said, you can’t avoid all jargon, so here is a brief list of some of the more common words you should become familiar with: 


-	***Object:*** Most definitions online of objects in programming language border on the philosphoical. So for basic purposes, an object is your data, and all subsequent alterations of your data in the programming environment. 

-	***“=” or <-:*** the equals sign and the arrow can be used interchangeably (for the most part) as assignment operators in R. These assignment operators will tell R that whatever is on the right side of the operator is now assigned to whatever is on the left side of the operator. (There are subtle differences between the two, and if you want to be hardcore use <- because it's more correct, but I'm not hardcore and lazy and use the operator with only one character) 

-	***Function:*** In very basic terms, a function is technically an object itself, but it is essentially a piece of code that will perform a task for you. You can eventually write your own functions to perform specific tasks if you so desire, but the nice R community has developed many different functions for different uses so for the most part, you don't have to trouble yourself with reinventing the wheel.  

-	***Package:*** collections of functions and data sets developed by the community. Extend the functionalities of R beyond what is possible with base R

-	***Vector:*** The simplest data structure in R. A sequence of data of the same type (i.e. all numeric or all character).  

-	***Matrix:*** Similar to a vector but contains two dimensions (columns *and* rows). All data is of the same type 

-	***Data frame:*** Also data stored in two dimensions, but in contrast to a matrix, the data can be of multiple types. 

-	***Vignette:*** Basically the help page for a function or package 

-	***CRAN:*** contains all up-to-date code and documentation for R and R packages

-	***Console:*** A window in which a user can type R commands, submit them for execution, and view the results. The most user-friendly I've come across for R is [RStudio](https://rstudio.com/)

-	***Environment:*** The environment can be thought of as a collection of objects created during your R session. There is a list under the tab "Environment" in RStudio that contains a list of all the objects in your environment 



## Basic commands ##
**Tab complete**

I’m putting this first because it will save you so much time and trouble. When you are typing almost anything in the R console, you can press the **tab** key on your keyboard, and R will either autocomplete the word you were trying to write or give you a list of options that you can scroll through and select. This is especially helpful when you’re trying to import data, or refer to an object you have already created. Also when you tab complete, you prevent spelling errors! A very very common type of error that you will encounter. 

**Cancelling code** 

*“The power to destroy a thing is the absolute control over it”*

It’s always good to know how to stop something in case you unintentionally create a limitless code loop. Pressing “control” and “c” on your keyboard will stop any command that is spiralling out of control. Also if you can’t see the **“>”** sign it means that R is either still running or expecting some other code from you. 
 Alternatively, if you keep getting **“+”** when you press enter it’s because you’ve ended a command with “+” and R expects more from you. To get back to the **>** sign, press almost any other key (as long as it’s not an object of yours) and R will error out.

**Installing packages**

You only need to do this once for each package you want to install, and then potentially again if you want to update R or the package. Here is an example of how to install the package *Hmisc* (note the quotation marks): 

```
install.packages("Hmisc")
``` 

**Loading packages**

Every time you wish to use a function that’s not present in base R, you need to load the package that contains the function. 

```
library(Hmisc)
```

**Setting your working directory**

“Working directory” is a bit of a jargony term. It essentially refers to the folder that contains the files that you would like to work with. Generally, it’s best if all your files for one project are located in one folder (directory), as it’s not as easy with R as it is with excel to continually change folder locations. There are multiple ways to set your working directory. 
You can do it manually in RStudio by going to **Session** -> **Set working directory** -> **Choose Directory..** 
Or alternatively you can use the command *setwd* and supply the path to your folder. 

```
#set your working directory
setwd("~/R blog")

#find out what directory you’re working from 
getwd()
```

**Loading your data**

I generally exclusively work with csv files (comma separated), but R handles other types of data like tsv, etc. csv files can be made from any excel file that has one tab, by clicking “save as” in excel and selecting the csv (Comma delimited, .csv) option. If you save over an excel file as a csv beware that you will lose all formatting, figures and equations from your original file. I tend to keep an excel file as backup and use a stripped-down csv version for R analysis. 

There are multiple ways to load in data. This is the way I use: 

```
df = read.csv("Your_data.csv", header = TRUE)
```
Where you change “Your_data.csv” to the name of your csv file. *Header = TRUE* means that the first row pf your data is designated as the column names. 
R doesn’t like when column names begin with numbers, or other strange characters. It will add an “X” to the beginning of any such column name. It also doesn’t appreciate spaces or other strange characters (%,&,’) besides underscores and periods in column names. It will change anything else into dots. 

**Looking at your data**

These commands will provide you some exploratory insight into your data to ensure that it was loaded properly and is behaving how you think it should: 

-	*str()* : gives you information about the structure of your object. Mainly, how many variables and observations, what type of object is it (data frame, matrix, etc), and what type of data is contained in each of your variables (if present)
-	*summary()* : when used on a data frame or matrix, will return the summary statistics of each column in your data set, i.e. mean, median, number of occurrences of each factor etc. Useful for finding “NAs” or missing data in columns 
-	*class()* : will tell you the class of your object
- *head()* : displays the top n (5 default) rows of your object in the console
-	*names()* : returns the column name of your object 
-	*ncol()* : returns the number of columns in your object
-	*nrow()* : returns the number of rows in your object


## Errors ##

Anyone who has used R, at any experience level, has at one point or another become extremely frustrated with errors. Here I’ve compiled some of the most common types of errors that I’ve come across, and how to fix them: 

**Typos**

Did you make a spelling error? Are you sure? No really, did you triple check? Make sure the function is spelled correctly too.  Try tab complete to avoid spelling errors as much as possible. Remember R is case sensitive so watch for that as well. Read this twice because the error is honestly probably a typo. 

**Ensure all your brackets and quotes are closed**

This one is pretty common too. All brackets ( ) and quotes " " need to have a partner. This seems obvious in simple commands but can get a bit confusing when you have brackets inside of brackets inside of brackets. RStudio and some text editors have nice features that will tell you when you are missing a closure or will highlight the partner bracket/quote in your code. Also watch for **+** and **,**. R expects these characters to be followed by something, so if they’re found at the end of a piece of code then they will cause an error as well. 

**Object type**

This one is another common problem but is confusing for beginners. Many functions require a certain object type for the function to work. Commonly the issue is that your data is in the form of a data frame, rather than a matrix. To check the requirements and the help file of a function, type ? followed by the function name:

```
#Example 
?dist
```

To check what format your data is in: 
```
class(df)
``` 
If the output says “data.frame” but the function requires a matrix, you need to change your data frame into a matrix. “num” means numeric and signifies the data is in a numeric matrix form. 

```
#to change a data frame (df) to a matrix
df_m = as.matrix(df)

#to change a matrix to a data frame 
df = as.data.frame(df_m)
```
You may also need to change the data type of columns within your object. For instance you may have a column called **year** filled with years, which are numbers. R will read these as a continuous variable or number, but you might want to treat them as a discrete category. This is the code structure to make the switch: 

```
#change column year from numeric to discrete (character) 
df$year = as.character(df$year)

#change column year from discrete to numeric
df$year = as.numeric(df$year)
```
