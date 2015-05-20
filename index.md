---
title       : Exponential Distribution Simulation
subtitle    : Course Project for Developing Data Products
author      : Richard Soon
job         : Teaching Assistant
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Create ShinyUI
In order to contain both the main interface and supporting document,we use tab pages.The first tab page defines the input parameters and output methods.

```r
    tabPanel("Plot", sidebarLayout(sidebarPanel(
    h2('Exponential Distribution Simulation Settings'),
    sliderInput('lambda', 'Numeric input, labeled lambda', 0.05, min = 0.05, max = 1, step = 0.05),
    numericInput('popSize',  "Population Size",10,min=10,max=100,step=10),
    numericInput('repTimes',  "Repetition Times",100,min=100,max=2000,step=100),
    checkboxInput("meanLine", "Show the Mean Line", FALSE)
    ), mainPanel(h2('Distribution of Average'),plotOutput('figure'))
   )  
```
And the second page shows the supporting document

```r
   tabPanel("Supporting Document", h3("This application aims to...."),
   p("1.Users can set ..... "), p("2.Users can choose ......") )
  )
```

--- .class #id 

## Create Server
Using the input values,we create a matrix of exponentional samples, then calculate average of each population.Finally,we plot a histogram of averages.

```r
cfunc <- function(x,lambda,n) sqrt(n) * (mean(x) - 1/lambda) / 1/lambda
shinyServer(
function(input, output) {
output$figure<-renderPlot({
lambda<-input$lambda;n<-input$popSize;scale<-input$repTimes
set.seed(111)
expArray<-matrix(rexp(n*scale,lambda),scale) 
meanArray<-apply(expArray,1,cfunc,lambda,n) 
hist(meanArray,breaks=20,freq=FALSE,xlab="Mean of The Samples",main="Histogram Of Averages of Exponential Distribution")
if(input$meanLine) abline(v=0,col="red",lwd=3)
})})
```

--- .class #id 

## Install Shinyapps Packages
Complete and save those codes in ui.r and server.r respectively in a directory DeployFiles, start the shiny application on your computer.

```r
runApp("DeployFiles")
```
You will see the application in a webpage.If you hope to publish this application online,you need install shinyapps packages from CRAN.

```r
install.packages('devtools')
```
With devtools installed you can install the shinyapps package directly from the GitHub. Run following code in your R console:

```r
devtools::install_github('rstudio/shinyapps')
```

--- .class #id 

## Deploy Applications Online
Before we upload our application online,we should create an account on shinyapps.io website.Once we have done that,we're ready to deploy our shiny application.Remember,set the path where your ui.r and server.r located.

```r
library(shinyapps);library(rmarkdown)
deployApp("DeployFiles")
```
In the working director,we'll find a shinyapps folder.Also,the application has been installed on shinyapps.io successfully.Here's my link:

```r
browseURL("https://topsun888.shinyapps.io/DeployFiles")
```
The interface of this application is as following:

```r
browseURL("Exponential Distribution.JPG")
```





