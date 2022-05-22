---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Tree Crown Benchmark"
summary: "A benchmark dataset for assessing crown detection and delineation methods for canopy trees in the United States"
authors: []
tags: ["dataset"]
categories: []
date: 2021-10-25T15:04:31-04:00
show_date: false
profile: false

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.

image:
  focal_point: Smart
  preview_only: false
  alt_text: "2 by 4 grid of images of tree crown annotations showing tree crowns for two sites (one on each row) on different remote sensing products (each column) including RGB, RGB - Stretch, LiDAR canopy height, and hyperspectral. Annotations are generally well aligned with impressions of trees in remote sensing."

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
links:
- name: Project Website
  url: https://idtrees.org/
  icon_pack: fa
  icon: home
- name: Data
  url: https://doi.org/10.5281/zenodo.3723356
  icon_pack: fa
  icon: database
- name: Paper
  url:  https://doi.org/10.1371/journal.pcbi.1009180
  icon_pack: fa
  icon: file-lines

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

Broad scale remote sensing promises to build forest inventories at unprecedented scales. A crucial step in this process is to associate sensor data into individual crowns. While dozens of crown detection algorithms have been proposed, their performance is typically not compared based on standard data or evaluation metrics. There is a need for a benchmark dataset to minimize differences in reported results as well as support evaluation of algorithms across a broad range of forest types. Combining RGB, LiDAR and hyperspectral sensor data from the USA National Ecological Observatory Network’s Airborne Observation Platform with multiple types of evaluation data, we created a benchmark dataset to assess crown detection and delineation methods for canopy trees covering dominant forest types in the United States. This benchmark dataset includes an R package to standardize evaluation metrics and simplify comparisons between methods. The benchmark dataset contains over 6,000 image-annotated crowns, 400 field-annotated crowns, and 3,000 canopy stem points from a wide range of forest types. In addition, we include over 10,000 training crowns for optional use. We discuss the different evaluation data sources and assess the accuracy of the image-annotated crowns by comparing annotations among multiple annotators as well as overlapping field-annotated crowns. We provide an example submission and score for an open-source algorithm that can serve as a baseline for future methods. [Abstract from paper]