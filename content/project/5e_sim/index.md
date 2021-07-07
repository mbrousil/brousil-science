---
title: Dungeons and Dragons Simulation with Shiny
summary: I wrote a short Shiny app to simulate some game mechanics in Dungeons and Dragons in order to visualize and quantify how probability affects game outcomes.
tags:
- Shiny
date: "2020-07-02"
weight: 4

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Example distribution of damage in a simulated 5e encounter.
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

During grad school I was playing in a [Pathfinder](https://en.wikipedia.org/wiki/Pathfinder_Roleplaying_Game) game with some friends and found myself wondering whether a new item my character had received was something I should use or *sell*. The new sword's stats were similar enough to the one my character was currently using that it wasn't clear to me which one was better. I was still fairly new to R at the time, so I used this as an opportunity to test out `for` loops, `if` statements, and functions in more depth.

I ended up writing a script that accepted inputs that a typical Pathfinder or Dungeons and Dragons (D&D) player would be actively using: Statistics about their character, the opposing character (or monster!), the number of dice they roll when they attack, and so on.

In the short term I found out that the new weapon was better, but this example has served as an ongoing learning experience for me outside of gameplay. I eventually updated the script when I started playing with the new rules of Dungeons and Dragons 5th edition (5e), and then again when I wanted to dig into building Shiny apps with R.

I now have two versions of this as a Shiny app on shinyapps.io:
 + My standard version built for a character of the barbarian class in D&D 5th Edition. Test it [here](http://cactusoxbird.shinyapps.io/dd-shiny-sim).
 + A rewrite of the standard version using the `purrr` package to change how simulation runs are handled. This is slower than the original version of the app, but I found it to be a useful case for understanding how the `map()` family can be put to use for simulation. Test it [here](https://cactusoxbird.shinyapps.io/dd-shiny-sim-purrr/).

Both versions of the app provide a couple of pieces of output for the inquisitive D&D player:
  + A histogram of the total damage done by the character for each of the simulation runs
  + Lines on the histogram and printed text showing the mean and median damage done over the course of the simulated runs
  + A tab containing a table of hit statistics: All of the amounts of damage their character did over the simulation runs, the raw number of times each amount of damage was done, and the percentage of the time that amount of damage was done

These outputs were helpful as a Pathfinder/D&D player, and as I reviewed them I ended up learning more than that my character's new sword was an improvement. For example, what I had not realized while throwing dice down on the table was *just how frequently* a character misses. Using the current base settings for the D&D 5e barbarian simulation shows that a character should expect to miss ~30% of the time they take a swing. This might be dismaying, but also could be a bit of a confidence boost for anyone who feels like they have bad luck a lot.

You can find the scripts for this app on GitHub [here](https://github.com/mbrousil/5e-shiny-sim).
