+++
title = "Individual tree-crown detection in RGB imagery using semi-supervised deep learning neural networks"
date = "2019-01-16"

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Ben G Weinstein", "Sergio Marconi", "Stephanie Bohlman", 
"Alina Zare", "Ethan White"]

# Publication type.
# Legend:
# 0 = Uncategorized
# 1 = Conference proceedings
# 2 = Journal
# 3 = Work in progress
# 4 = Technical report
# 5 = Book
# 6 = Book chapter
publication_types = ["2"]

# Publication name and optional abbreviated version.
publication = "Remote Sensing"
publication_short = "Remote Sens"
# Abstract and optional shortened version.
abstract = "Remote sensing can transform the speed, scale, and cost of biodiversity and forestry surveys. Data acquisition currently outpaces the ability to identify individual organisms in high resolution imagery. We outline an approach for identifying tree-crowns in RGB imagery while using a semi-supervised deep learning detection network. Individual crown delineation has been a long-standing challenge in remote sensing and available algorithms produce mixed results. We show that deep learning models can leverage existing Light Detection and Ranging (LIDAR)-based unsupervised delineation to generate trees that are used for training an initial RGB crown detection model. Despite limitations in the original unsupervised detection approach, this noisy training data may contain information from which the neural network can learn initial tree features. We then refine the initial model using a small number of higher-quality hand-annotated RGB images. We validate our proposed approach while using an open-canopy site in the National Ecological Observation Network. Our results show that a model using 434,551 self-generated trees with the addition of 2848 hand-annotated trees yields accurate predictions in natural landscapes. Using an intersection-over-union threshold of 0.5, the full model had an average tree crown recall of 0.69, with a precision of 0.61 for the visually-annotated data. The model had an average tree detection rate of 0.82 for the field collected stems. The addition of a small number of hand-annotated trees improved the performance over the initial self-supervised model. This semi-supervised deep learning approach demonstrates that remote sensing can overcome a lack of labeled training data by generating noisy data for initial training using unsupervised methods and retraining the resulting models with high quality labeled data."

# Featured image thumbnail (optional)
image_preview = ""

# Is this a selected publication? (true/false)
selected = false

# Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter the filename (excluding '.md') of your project file in `content/project/`.
projects = ["remote-sensing"]

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
#url_custom = [{name = "HTML", url = "https://doi.org/10.7717/peerj.5843"}]

# Links (optional).
url_custom = [{name = "HTML", url = "https://doi.org/10.3390/rs11111309"}]

# Does the content use math formatting?
math = false

# Does the content use source code highlighting?
highlight = false

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = "headers/bubbles-wide.jpg"
caption = "My caption :smile:"

+++


