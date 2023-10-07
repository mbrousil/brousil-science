---
title: Global lake area, climate, and population dataset
summary: A global dataset containing lake and reservoir surface area, basin-level temperature, precipitation, and population data.
tags:
date: "2020-06-11"
weight: 2

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: A portion of the map from Figure 3 in [Meyer et al., 2020](https://aslopubs.onlinelibrary.wiley.com/doi/10.1002/lob.10406)
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

The global lake area, climate, and population dataset (GLCP) is a dataset containing lake surface area for 1.42+ million lakes and reservoirs from 1995 to 2015 with basin-level temperature, precipitation, and population data.

I joined the team working on this dataset in 2018 to support quality control, data management, and streamlining the workflow for the project. Over the course of our time putting the dataset together, I performed several main tasks:
 1. I reviewed the R scripts used in the workflow for accuracy and formatting; applied a consistent format and style to the scripts and their in-line documentation; and assisted with some parts of the R workflow such as data visualization, data subsetting, and mapping.
 2. I advised on how to improve the file structure and documentation (e.g. READMEs) for the project to ensure reproducibility.
 3. I helped build the QA & QC processes for the dataset.

The scripts for this dataset are publicly available at the Environmental Data Initiative, [here](https://portal.edirepository.org/nis/mapbrowse?packageid=edi.394.4) as `glcp_scripts.tar.gz`.
