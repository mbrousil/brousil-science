---
title: Workflow management with drake
summary: A workshop lesson covering workflow management with `drake` and a brief introduction to functions in R.
tags:
- Teaching
- Workshops
date: "2020-11-20"
weight: 2

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: An example drake dependency graph
  focal_point: Smart

links:
icon: ""
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: example
---

In fall 2020 the [Center for Environmental Research, Education and Outreach](https://cereo.wsu.edu/) at Washington State University hosted [a workshop](https://mbrousil.github.io/workshops/2020-workshop-1) covering reproducible research techniques in R for graduate students. We wanted to cover `drake` workflows as one day of the workshop to show students what R-specific options there are for managing workflows. We expected that workflow management and to some extent R functions would be unfamiliar topics to many students, so the workshop day included discussion of why one would use workflow management software and some basic examples of building realistic functions. At the time that we ran the workshop I wasn't aware of [`targets`](https://github.com/ropensci/targets), so in the future we may repurpose this example and replace `drake` with that package.
 
I put together an R Markdown document {{% staticref "uploads/drake_wkshp.html" %}}here{{% /staticref %}} where I've combined the contents from the intro presentation I used along with the lesson material in a single document. The (mostly) empty folder structure I provided for the walkthrough is [here](https://github.com/mbrousil/drake_template) and an example of the finished product is in a repo [here](https://github.com/mbrousil/example_drake_project). We used Fanaee-T and Gama's (2013) dataset on bike sharing in DC. More info [here](https://archive.ics.uci.edu/ml/datasets/Bike+Sharing+Dataset).

Link to [workshop website](https://cereo.wsu.edu/reproducible-r-workshop-2021s/).