---
title: Introduction to Meta-Analysis with R
summary: In this session, I will briefly introduce how to conduct a meta-analysis using R and meta and metafor packages.
authors: [Mohammadreza Amiri]
tags: []
categories: []
date: "2021-10-26T12:00:00Z"
slides:
  theme: white
  highlight_style: dracula
---

```{r packages, include=FALSE}
# install.package("tidyverse", "meta", "metafor", "openxlsx")
library(tidyverse)
library(meta)
library(metafor)
library(openxlsx)
```

# Forest Plot Using meta Package

```{r, fig.align='center', fig.height=5, fig.width=10, fig.cap="Forest Plot Using meta Package"}
nfr <- read.xlsx(xlsxFile = "https://github.com/rexaamiri/MetaAnalysis/blob/main/NFR_v_Control_final.xlsx")

nfrModel <- metacont(data=nfr, 
                     n.e = ss1, 
                     mean.e = m1, 
                     sd.e = sd1, 
                     n.c = ss2, 
                     mean.c = m2, 
                     sd.c = sd2,
                     studlab = Study.Label, 
                     sm = "SMD")

forest(x = nfrModel, comb.random = F, digits.mean = 1, digits.sd = 1)
```

---

# Funnel Plot Using metafor Package

A [funnel plot](https://en.wikipedia.org/wiki/funnel%20plot "https://en.wikipedia.org/wiki/funnel plot") shows the observed effect sizes or outcomes on the x-axis against a precision measure of the observed effect sizes on the y-axis. In metafor package, the recommended choice for the y-axis is the standard error (in decreasing order) which is in line with @Sterne2001. If there is no publication bias or heterogeneity among studies, the expectation is to see all points to fall within the pseudo-confidence shape funnel, i.e., the triangle shape funnel for the case of standard errors on the y-axis.

```{r funnel-plot, fig.align='center', fig.cap="Common Funnel Plots"}
funnel(res, main="Standard Error")
funnel(res, yaxis="seinv", main="Inverse Standard Error")
```

---

# Funnel Plot Using meta Package

```{r funnel-plot-meta, fig.align='center', fig.cap="Common Funnel Plots Using meta Package"}
nfr <- read.xlsx(xlsxFile = "NFR_v_Control_final.xlsx")

nfrModel <- metacont(data=nfr, 
                     n.e = ss1, 
                     mean.e = m1, 
                     sd.e = sd1, 
                     n.c = ss2, 
                     mean.c = m2, 
                     sd.c = sd2,
                     studlab = Study.Label, 
                     sm = "SMD")

meta::funnel.meta(nfrModel, 
                  contour.levels = c(0.9, 0.95, 0.99), yaxis = "invvar")
```
---
