---
title: Examples of using regular expressions in R
author: mbrousil
date: '2021-12-20'
slug: regex-examples
categories: []
tags:
  - Teaching
  - Live coding
subtitle: ''
summary: ''
authors: []
lastmod: '2021-12-20T21:53:48-08:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


In this post I'll be walking through some basic examples of [regex](https://en.wikipedia.org/wiki/Regular_expression), or regular expressions, and also providing a couple real-world examples.

To quote Hadley Wickham and Garrett Grolemund ([2017](https://r4ds.had.co.nz/strings.html#matching-patterns-with-regular-expressions)), regex is a *"language that allow[s] you to describe patterns in strings."* When we have string/character data in R, we may want to extract information from it. And regex is a tool for doing this.

As a conceptual example, consider the sentence below:

`“The user’s phone number is (555) 555-5555.”`

This string of text could be something scraped from a PDF, or perhaps provided as input to a survey. Our end goal might be to extract and store the phone number in that sentence. One way to do this would be to write a **regular expression** that describes the pattern of a phone number, which R can then use to extract the phone number from the string.

Another common example is pulling metadata from filenames. If you have a folder of rasters with names like `"farm_aerial_imagery_whitman_county_2021.tiff"`, you might want to filter them based on their contents. Using regex we can give R instructions to separate out the county name and year from the filenames, which allows us to determine the spatial and temporal coverage of our files.

Now I'll go through a few examples. If you want to follow along, you can download the `.zip` folder from [here](https://drive.google.com/drive/folders/1jADPyF12N6dPzBQs-Kwn3OQyca96ZNI8?usp=sharing). Make sure to unzip the folder and open the script it contains. We'll fill it in below:


```r
library(tidyverse)
library(stringi)
```

Note that tidyverse contains the `stringr` package, which contains a variety of useful functions for working with strings. We'll barely scratch the surface of its capabilities here.

## 1. A simple example of regex

A common application of regex is to extract numbers from text data. Let's say we have this filename:

```r
filename <- "2021A.txt"
```

This is how we could confirm the presence of just the year from it using base R: We could search for the year verbatim, returning a logical T/F with `grepl()`.

```r
grepl(pattern = "2021", x = filename)
```

```
## [1] TRUE
```

<br>

Or we could generalize it to just identifying where a number occurs. We use a pattern to identifying numbers 0 through 9:

```r
grepl(pattern = "[0-9]", x = filename)
```

```
## [1] TRUE
```

<br>

More helpful would be to find four numbers in a row, as indicated with our bracketed 4:

```r
grepl(pattern = "[0-9]{4}", x = filename)
```

```
## [1] TRUE
```

<br>

And then finally to actually return those numbers to us using `str_extract()` so that we can use them:

```r
?str_extract
```



```r
str_extract(string = filename, pattern = "[0-9]{4}")
```

```
## [1] "2021"
```

<br>

Now you might imagine that we could take this one step further and pull the dates from an entire vector of filenames. This will be especially useful if we want to extract these data from an entire column, for example.

```r
file_list <- c("2021A.txt", "1995A.txt", "2021B.txt", "1995B.txt")


str_extract(string = file_list, pattern = "[0-9]{4}")
```

```
## [1] "2021" "1995" "2021" "1995"
```

<br>


## 2. Separating and extracting

Pretend that we're given a list of student names with grades. We want to use this for an analysis, but the names and grades are combined in a random order into a single string. We'll need to separate them somehow. Luckily regex provides us the tools to do so.


```r
grade_data <- read.csv(file = "grades.csv")

head(grade_data)
```

```
##           grades
## 1   COSTA.gradeB
## 2    gradeD.LOWE
## 3  gradeC.SEXTON
## 4 ELLIOTT.gradeD
## 5   MARIN.gradeD
## 6  GUERRA.gradeC
```

Let's test out a potential regex pattern to find grades. We want a single capital letter, right?

```r
str_extract(string = grade_data$grades,
            pattern = "[A-Z]{1}")
```

```
##   [1] "C" "D" "C" "E" "M" "G" "B" "D" "B" "F" "C" "B" "D" "C" "H" "S" "F" "B"
##  [19] "D" "G" "H" "C" "H" "H" "B" "F" "R" "F" "G" "B" "W" "R" "A" "S" "W" "D"
##  [37] "W" "D" "W" "R" "B" "M" "C" "C" "E" "M" "I" "B" "A" "Z" "K" "C" "B" "C"
##  [55] "B" "R" "C" "D" "F" "C" "D" "D" "D" "T" "C" "M" "K" "C" "B" "C" "C" "B"
##  [73] "B" "A" "A" "D" "F" "F" "A" "Q" "L" "A" "D" "F" "B" "D" "F" "R" "A" "A"
##  [91] "B" "D" "T" "F" "F" "V" "A" "D" "B" "C"
```

Hmm, that's the general idea, but it's clearly just extracting the first capital letter in the text string, which isn't always a grade.

Instead, look for `"grade"` followed by a single capital letter.

```r
str_extract(string = grade_data$grades,
            pattern = "grade[A-Z]{1}")
```

```
##   [1] "gradeB" "gradeD" "gradeC" "gradeD" "gradeD" "gradeC" "gradeB" "gradeD"
##   [9] "gradeB" "gradeF" "gradeD" "gradeB" "gradeD" "gradeF" "gradeD" "gradeA"
##  [17] "gradeF" "gradeB" "gradeD" "gradeA" "gradeC" "gradeC" "gradeA" "gradeA"
##  [25] "gradeB" "gradeF" "gradeC" "gradeF" "gradeF" "gradeB" "gradeB" "gradeB"
##  [33] "gradeA" "gradeD" "gradeA" "gradeD" "gradeF" "gradeD" "gradeF" "gradeB"
##  [41] "gradeB" "gradeF" "gradeB" "gradeC" "gradeD" "gradeC" "gradeD" "gradeA"
##  [49] "gradeA" "gradeA" "gradeD" "gradeC" "gradeA" "gradeC" "gradeB" "gradeA"
##  [57] "gradeC" "gradeD" "gradeF" "gradeC" "gradeD" "gradeD" "gradeD" "gradeC"
##  [65] "gradeC" "gradeD" "gradeC" "gradeC" "gradeB" "gradeC" "gradeD" "gradeB"
##  [73] "gradeB" "gradeA" "gradeA" "gradeB" "gradeF" "gradeF" "gradeA" "gradeC"
##  [81] "gradeC" "gradeA" "gradeD" "gradeF" "gradeB" "gradeD" "gradeF" "gradeF"
##  [89] "gradeC" "gradeB" "gradeB" "gradeD" "gradeB" "gradeF" "gradeF" "gradeA"
##  [97] "gradeA" "gradeD" "gradeA" "gradeC"
```

That works pretty much exactly as we intended...Save it for later:

```r
grades <- str_extract(string = grade_data$grades,
                      pattern = "grade[A-Z]{1}")

# And remove "grade" from it
grades <- gsub(pattern = "grade", replacement = "", x = grades)
```

<br>

What about last names? Well, there are probably many ways to do this one. A "easy" way could be to first remove the grade info, then keep the rest:


```r
grade_remove <- gsub(pattern = "grade[A-Z]{1}", replacement = "", x = grade_data$grades)

head(grade_remove)
```

```
## [1] "COSTA."   ".LOWE"    ".SEXTON"  "ELLIOTT." "MARIN."   "GUERRA."
```

Ok, so far so good. There's also a pesky period in there...

```r
gsub(pattern = ".", replacement = "", x = grade_remove)
```

```
##   [1] "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
##  [26] "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
##  [51] "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
##  [76] "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
```

That doesn't look right! The period `.` is a pattern that is used in regex as a wildcard, standing in for any character. We have to "escape" it in order to use it to signify a literal period.

We use the double backslash in R to "escape" these special characters. We use two instead of one because of the way that R uses strings. We're not going to get into the detail here


```r
head(gsub(pattern = "\\.", replacement = "", x = grade_remove))
```

```
## [1] "COSTA"   "LOWE"    "SEXTON"  "ELLIOTT" "MARIN"   "GUERRA"
```

<br>

Save it for later:

```r
student_names <- gsub(pattern = "\\.", replacement = "", x = grade_remove)
```

We can now combine these tricks to make a new data frame!

```r
grade_records <- tibble(
  student_names,
  grades
)
```

<br>

## 3. Kick it up a notch

In this section I want to share some regex routines I've used in my real-world work to speed up or simplify data cleaning. 

They're going to be a little higher level, but hopefully useful to show how these things can be applied day-to-day.

### 3a. Lookahead/Lookbehind
We have a file with a column of coordinates that needs to be reformatted. I'll show how lookahead/lookbehind methods can help here.


```r
coordinates <- read.csv(file = "coordinates.csv")

head(coordinates)
```

```
##                   coordinates count       date
## 1 N 46° 43.9214 W 117° 9.6333    28 10/10/2020
## 2    N 21° 20.04 W 158° 50.05    13 10/10/2020
## 3 N 53° 31.0101 E 108° 6.0512    47  10/5/2020
## 4 N 46° 43.9214 W 117° 9.6333    96 10/10/2020
## 5    N 21° 20.04 W 158° 50.05    95  10/5/2020
## 6    N 21° 20.04 W 158° 50.05    66 10/10/2020
```

<br>

We want to identify pieces of text in the coordinates column that are latitudes or longitudes. Latitudes = pieces of text followed by " W" or " E". Longitudes = pieces of text preceded by " W" or " E".

**Extract Latitude**:
We want to use a *positive lookahead*. i.e., our piece of targeted text (latitude) exists BEFORE a specific textual pattern ("W" or "E"). The text pattern in the lookahead is NOT included in the matched text, so we won't end up storing the W or E!

<br>

The regex syntax for this is: `"(?=...)"`. We'll include the leading space before the W/E as well. Note that the `"|"` stands for "OR" and `".*"` signifies "any character zero or more times". [Here's](https://www.keycdn.com/support/regex-cheatsheet) a good reference for the syntax.

```r
coordinates %>%
  mutate(latitude = str_extract(string = coordinates,
                                pattern = ".*(?= E| W)"))
```

```
##                    coordinates count       date      latitude
## 1  N 46° 43.9214 W 117° 9.6333    28 10/10/2020 N 46° 43.9214
## 2     N 21° 20.04 W 158° 50.05    13 10/10/2020   N 21° 20.04
## 3  N 53° 31.0101 E 108° 6.0512    47  10/5/2020 N 53° 31.0101
## 4  N 46° 43.9214 W 117° 9.6333    96 10/10/2020 N 46° 43.9214
## 5     N 21° 20.04 W 158° 50.05    95  10/5/2020   N 21° 20.04
## 6     N 21° 20.04 W 158° 50.05    66 10/10/2020   N 21° 20.04
## 7  N 53° 31.0101 E 108° 6.0512    74  10/5/2020 N 53° 31.0101
## 8  N 53° 31.0101 E 108° 6.0512    62 10/10/2020 N 53° 31.0101
## 9  N 53° 31.0101 E 108° 6.0512    66 10/20/2020 N 53° 31.0101
## 10 N 46° 43.9214 W 117° 9.6333    58 10/20/2020 N 46° 43.9214
## 11 N 53° 31.0101 E 108° 6.0512    36 10/20/2020 N 53° 31.0101
## 12 N 46° 43.9214 W 117° 9.6333    49 10/20/2020 N 46° 43.9214
## 13 N 53° 31.0101 E 108° 6.0512    39  9/25/2020 N 53° 31.0101
## 14 N 53° 31.0101 E 108° 6.0512    15 10/20/2020 N 53° 31.0101
## 15 N 46° 43.9214 W 117° 9.6333    75  9/25/2020 N 46° 43.9214
```

Here's a quick summary of the other lookahead/behind options:

***Negative lookahead***: Our piece of targeted text is NOT followed by a specific
textual pattern. Lookahead not included in match.
Syntax: `"(?!...)"`

***Positive lookbehind***: Our piece of targeted text exists AFTER a specific textual pattern. That pattern is NOT included in the matched text.
Syntax: `"(?<=...)"`

***Negative Lookbehind***: Our piece of targeted text does NOT exist AFTER a specific textual pattern. That pattern is NOT included in the matched text.
Negative Lookbehind: `"(?<!...)"`

<br>

**Extract Longitude**:
To get the longitude from our coordinate column we can find the pieces of text that begin with either an E or W. It's a little simpler than what we did for latitude:

```r
coordinates %>%
  mutate(longitude = str_extract(string = coordinates,
                                 pattern = "E.*|W.*"))
```

```
##                    coordinates count       date     longitude
## 1  N 46° 43.9214 W 117° 9.6333    28 10/10/2020 W 117° 9.6333
## 2     N 21° 20.04 W 158° 50.05    13 10/10/2020  W 158° 50.05
## 3  N 53° 31.0101 E 108° 6.0512    47  10/5/2020 E 108° 6.0512
## 4  N 46° 43.9214 W 117° 9.6333    96 10/10/2020 W 117° 9.6333
## 5     N 21° 20.04 W 158° 50.05    95  10/5/2020  W 158° 50.05
## 6     N 21° 20.04 W 158° 50.05    66 10/10/2020  W 158° 50.05
## 7  N 53° 31.0101 E 108° 6.0512    74  10/5/2020 E 108° 6.0512
## 8  N 53° 31.0101 E 108° 6.0512    62 10/10/2020 E 108° 6.0512
## 9  N 53° 31.0101 E 108° 6.0512    66 10/20/2020 E 108° 6.0512
## 10 N 46° 43.9214 W 117° 9.6333    58 10/20/2020 W 117° 9.6333
## 11 N 53° 31.0101 E 108° 6.0512    36 10/20/2020 E 108° 6.0512
## 12 N 46° 43.9214 W 117° 9.6333    49 10/20/2020 W 117° 9.6333
## 13 N 53° 31.0101 E 108° 6.0512    39  9/25/2020 E 108° 6.0512
## 14 N 53° 31.0101 E 108° 6.0512    15 10/20/2020 E 108° 6.0512
## 15 N 46° 43.9214 W 117° 9.6333    75  9/25/2020 W 117° 9.6333
```

<br>

Now let's combine these two approaches:

```r
coordinates %>%
  mutate(latitude = str_extract(string = coordinates,
                                pattern = ".*(?= E| W)"),
         longitude = str_extract(string = coordinates,
                                 pattern = "E.*|W.*"))
```

```
##                    coordinates count       date      latitude     longitude
## 1  N 46° 43.9214 W 117° 9.6333    28 10/10/2020 N 46° 43.9214 W 117° 9.6333
## 2     N 21° 20.04 W 158° 50.05    13 10/10/2020   N 21° 20.04  W 158° 50.05
## 3  N 53° 31.0101 E 108° 6.0512    47  10/5/2020 N 53° 31.0101 E 108° 6.0512
## 4  N 46° 43.9214 W 117° 9.6333    96 10/10/2020 N 46° 43.9214 W 117° 9.6333
## 5     N 21° 20.04 W 158° 50.05    95  10/5/2020   N 21° 20.04  W 158° 50.05
## 6     N 21° 20.04 W 158° 50.05    66 10/10/2020   N 21° 20.04  W 158° 50.05
## 7  N 53° 31.0101 E 108° 6.0512    74  10/5/2020 N 53° 31.0101 E 108° 6.0512
## 8  N 53° 31.0101 E 108° 6.0512    62 10/10/2020 N 53° 31.0101 E 108° 6.0512
## 9  N 53° 31.0101 E 108° 6.0512    66 10/20/2020 N 53° 31.0101 E 108° 6.0512
## 10 N 46° 43.9214 W 117° 9.6333    58 10/20/2020 N 46° 43.9214 W 117° 9.6333
## 11 N 53° 31.0101 E 108° 6.0512    36 10/20/2020 N 53° 31.0101 E 108° 6.0512
## 12 N 46° 43.9214 W 117° 9.6333    49 10/20/2020 N 46° 43.9214 W 117° 9.6333
## 13 N 53° 31.0101 E 108° 6.0512    39  9/25/2020 N 53° 31.0101 E 108° 6.0512
## 14 N 53° 31.0101 E 108° 6.0512    15 10/20/2020 N 53° 31.0101 E 108° 6.0512
## 15 N 46° 43.9214 W 117° 9.6333    75  9/25/2020 N 46° 43.9214 W 117° 9.6333
```

<br>

Alternatively, we could use the `separate()` function along with regex for the column separator. Here I tell R to separate the two columns where there is a number followed by a space and then either W or E. The two parentheses enclose lookahead and lookbehind phrases that shouldn't be included in the extracted text but which we ***do*** want to check for.

```r
coordinates %>%
  separate(col = coordinates,
           into = c("latitude", "longitude"),
           sep = "(?<=[0-9]) (?=W|E)")
```

```
##         latitude     longitude count       date
## 1  N 46° 43.9214 W 117° 9.6333    28 10/10/2020
## 2    N 21° 20.04  W 158° 50.05    13 10/10/2020
## 3  N 53° 31.0101 E 108° 6.0512    47  10/5/2020
## 4  N 46° 43.9214 W 117° 9.6333    96 10/10/2020
## 5    N 21° 20.04  W 158° 50.05    95  10/5/2020
## 6    N 21° 20.04  W 158° 50.05    66 10/10/2020
## 7  N 53° 31.0101 E 108° 6.0512    74  10/5/2020
## 8  N 53° 31.0101 E 108° 6.0512    62 10/10/2020
## 9  N 53° 31.0101 E 108° 6.0512    66 10/20/2020
## 10 N 46° 43.9214 W 117° 9.6333    58 10/20/2020
## 11 N 53° 31.0101 E 108° 6.0512    36 10/20/2020
## 12 N 46° 43.9214 W 117° 9.6333    49 10/20/2020
## 13 N 53° 31.0101 E 108° 6.0512    39  9/25/2020
## 14 N 53° 31.0101 E 108° 6.0512    15 10/20/2020
## 15 N 46° 43.9214 W 117° 9.6333    75  9/25/2020
```

<br>

### 3b. Group capture

Looking at the date column in this dataset we might also want to reformat it for one reason or another. I actually wouldn't typically do this with data, as there are functions to format date in the lubridate package. But where I have used this to reformat dates that are hard-coded into R scripts. In that situation I used the Find & Replace tool in RStudio along with the regex option.


```r
head(coordinates)
```

```
##                   coordinates count       date
## 1 N 46° 43.9214 W 117° 9.6333    28 10/10/2020
## 2    N 21° 20.04 W 158° 50.05    13 10/10/2020
## 3 N 53° 31.0101 E 108° 6.0512    47  10/5/2020
## 4 N 46° 43.9214 W 117° 9.6333    96 10/10/2020
## 5    N 21° 20.04 W 158° 50.05    95  10/5/2020
## 6    N 21° 20.04 W 158° 50.05    66 10/10/2020
```

We have dates in this format:  `MM/DD/YYYY`

```r
coordinates$date
```

```
##  [1] "10/10/2020" "10/10/2020" "10/5/2020"  "10/10/2020" "10/5/2020" 
##  [6] "10/10/2020" "10/5/2020"  "10/10/2020" "10/20/2020" "10/20/2020"
## [11] "10/20/2020" "10/20/2020" "9/25/2020"  "10/20/2020" "9/25/2020"
```

<br>

Let's manually convert the forward slashes to underscores, but do so using group captures. This syntax allows us to group parts of a text string, for example as a means of ensuring that the data we're editing meets certain conditions (e.g., for quality control).

To do this we denote groups with parentheses: `( )`. So, for example, we could use the following lines of code to "capture" the pieces of our dates:
`"([0-9]{1,2})/([0-9]{1,2})/([0-9]{4})"`

Then we can refer to the date pieces by numbers, in the order they are written. 
Depending on the function we're using, we refer to each group number with a dollar sign OR two backslashes ahead of its label:
`"$1_$2_$3"` 
or
`"\\1_\\2_\\3"`

So, if we put these two pieces together we can create a new date column like this:

With the `stringr` package:

```r
coordinates %>%
  mutate(new_date = str_replace(string = date,
                                pattern = "([0-9]{1,2})/([0-9]{1,2})/([0-9]{4})",
                                replacement = "\\1_\\2_\\3"))
```

```
##                    coordinates count       date   new_date
## 1  N 46° 43.9214 W 117° 9.6333    28 10/10/2020 10_10_2020
## 2     N 21° 20.04 W 158° 50.05    13 10/10/2020 10_10_2020
## 3  N 53° 31.0101 E 108° 6.0512    47  10/5/2020  10_5_2020
## 4  N 46° 43.9214 W 117° 9.6333    96 10/10/2020 10_10_2020
## 5     N 21° 20.04 W 158° 50.05    95  10/5/2020  10_5_2020
## 6     N 21° 20.04 W 158° 50.05    66 10/10/2020 10_10_2020
## 7  N 53° 31.0101 E 108° 6.0512    74  10/5/2020  10_5_2020
## 8  N 53° 31.0101 E 108° 6.0512    62 10/10/2020 10_10_2020
## 9  N 53° 31.0101 E 108° 6.0512    66 10/20/2020 10_20_2020
## 10 N 46° 43.9214 W 117° 9.6333    58 10/20/2020 10_20_2020
## 11 N 53° 31.0101 E 108° 6.0512    36 10/20/2020 10_20_2020
## 12 N 46° 43.9214 W 117° 9.6333    49 10/20/2020 10_20_2020
## 13 N 53° 31.0101 E 108° 6.0512    39  9/25/2020  9_25_2020
## 14 N 53° 31.0101 E 108° 6.0512    15 10/20/2020 10_20_2020
## 15 N 46° 43.9214 W 117° 9.6333    75  9/25/2020  9_25_2020
```

<br>

Or with the `stringi` package:

```r
coordinates %>%
  mutate(new_date = stri_replace(str = date,
                                 regex = "([0-9]{1,2})/([0-9]{1,2})/([0-9]{4})",
                                 replacement = "$1_$2_$3"))
```

```
##                    coordinates count       date   new_date
## 1  N 46° 43.9214 W 117° 9.6333    28 10/10/2020 10_10_2020
## 2     N 21° 20.04 W 158° 50.05    13 10/10/2020 10_10_2020
## 3  N 53° 31.0101 E 108° 6.0512    47  10/5/2020  10_5_2020
## 4  N 46° 43.9214 W 117° 9.6333    96 10/10/2020 10_10_2020
## 5     N 21° 20.04 W 158° 50.05    95  10/5/2020  10_5_2020
## 6     N 21° 20.04 W 158° 50.05    66 10/10/2020 10_10_2020
## 7  N 53° 31.0101 E 108° 6.0512    74  10/5/2020  10_5_2020
## 8  N 53° 31.0101 E 108° 6.0512    62 10/10/2020 10_10_2020
## 9  N 53° 31.0101 E 108° 6.0512    66 10/20/2020 10_20_2020
## 10 N 46° 43.9214 W 117° 9.6333    58 10/20/2020 10_20_2020
## 11 N 53° 31.0101 E 108° 6.0512    36 10/20/2020 10_20_2020
## 12 N 46° 43.9214 W 117° 9.6333    49 10/20/2020 10_20_2020
## 13 N 53° 31.0101 E 108° 6.0512    39  9/25/2020  9_25_2020
## 14 N 53° 31.0101 E 108° 6.0512    15 10/20/2020 10_20_2020
## 15 N 46° 43.9214 W 117° 9.6333    75  9/25/2020  9_25_2020
```

<br>

## References:
+ Grolemund, G., & Wickham, H. (2017). R for Data Science. O’Reilly Media.
