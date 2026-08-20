---
title: "AquaMatchr: An R package to assist with downloads, matchups, and analyses using AquaMatch data products"
summary: A package to assist with downloads, matchups, and analyses using [AquaMatch](https://aquasat.github.io/AquaMatch_documentation/) data products. Provides methods for downloading and joining harmonized in-situ AquaMatch datasets with lakeSR, siteSR, and riverSR datasets.
tags:
- Programming in R
- Data Science
- Open Source
date: "2026-08-18"
weight: 1

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Example use of the AquaMatchr package
  focal_point: Smart

links:
# icon: ""
url_code: "https://github.com/AquaSat/AquaMatchr/"
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

**AquaMatchr** is an open-source R package that I am developing in my capacity as a Lead Data Scientist for the [Radical Open Science Syndicate](https://www.rossyndicate.com/) (ROSS) at Colorado State University.

The goal of the package is to streamline the ability for researchers, resource managers, and stakeholders to access and easily make use of data products from the ROSS lab's [AquaSat 2.0](https://www.rossyndicate.com/projects/aquasat) project. Through AquaSat, the lab has produced both *in-situ* and remotely sensed data products related to inland water quality, which are intended to be matched spatiotemporally. Version 1.0 was the largest inland water quality matchup dataset assembled to date, and 2.0 builds upon that baseline further.

AquaSat 2.0 data products are large and we've made a concerted effort to make every step of their production scientifically and programmatically reproducible (e.g., [GitHub](https://github.com/AquaSat/AquaMatch_harmonize_WQP), [EDI](https://portal.edirepository.org/nis/mapbrowse?packageid=edi.2380.1)).

The **challenge** is that AquaSat 2.0 products are also intended to allow for customization on the end user's part, for example with training and validation of remote sensing models. The process of joining millions of rows of *in-situ* data with hundreds of millions of rows of surface reflectance data can be complex, technically demanding, and computing resource intensive. 

AquaMatchr is a programmatic **solution** to this using R in several ways:

+ **Data Retrieval**: It includes standardized downloading methods to grab specific published AquaSat datasets from their data repositories
+ **Complex Joins**: It provides single-function handling of complex, resource-intensive tasks like data stacking, temporal and site-based joins, and corrections for satellite standardization
+ **Out-of-Memory Computation**: It efficiently handles larger-than-memory data manipulations for the tasks above, which would otherwise cause R to crash
+ **Future Plans**: In the future it will provide diagnostic functions such as time series visualization for validating site subsets and functions to assist with model training for this already model-ready data ecosystem

The source code and documentation for `AquaMatchr` are publicly available on GitHub at https://github.com/AquaSat/AquaMatchr/
