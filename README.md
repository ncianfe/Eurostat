# Eurostat

---
title: "Eurostat Home Office"
author: "Nicola Cianferoni (ABGG)"
date: "16/2/2021"
output:
  html_document:
    df_print: paged
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = FALSE)

library(tidyverse)
library(labelled)
library(sjmisc)
library(haven)
library(ggplot2)
library(arsenal)
library(forcats)
library(eurostat)

```

## Eurostat

```{r Eurostat, include = FALSE}

# Get Eurostat data listing
toc <- get_eurostat_toc()

# Check the first items
head(toc)

# info about passengers
id <- search_eurostat("working from home")
View(id)

# lfsa_ehomp: Employed persons working from home as a percentage of the total employment, by sex, age and professional status (%)

dat <- get_eurostat(id = "lfsa_ehomp", time_format = "num")
str(dat)

datl <- label_eurostat(dat)
head(datl)

frq(datl$geo)


dat_ho <- subset(datl, geo %in% c("European Union - 28 countries (2013-2020)", "Italy", "Austria", "France", "Germany (until 1990 former territory of the FRG)", "Switzerland") & time %in% 1990:2020)

dat_ho$geo <- fct_recode(dat_ho$geo, "EU28" = "European Union - 28 countries (2013-2020)", "Germany" = "Germany (until 1990 former territory of the FRG)")

dat_ho$geo <- fct_relevel(dat_ho$geo, "Austria", "Germany", "France", "Italy", "Switzerland", "EU28")

```

# Traduction


```{r traduction, include = FALSE}


dat_ho_fr <- dat_ho

dat_ho_fr <- dat_ho_fr %>% rename(Pays = geo)

dat_ho_fr$Pays <- fct_recode(dat_ho_fr$Pays, "Autriche" = "Austria", "Allemagne" = "Germany", "Italie" = "Italy", "Suisse" = "Switzerland")
dat_ho_fr$Pays <- fct_relevel(dat_ho_fr$Pays, "Autriche", "Allemagne", "France", "Italie", "Suisse", "EU28")

dat_ho_fr$frequenc <- fct_recode(dat_ho_fr$frequenc, "Jamais" = "Never", "Habituellement" = "Sometimes", "Toujours" = "Usually")

frq(dat_ho_fr$frequenc)


```


```{r ggplot2, fig.height=25, fig.width=30}

dat_ho_fr %>% 
  filter(sex == "Total", age == "From 50 to 64 years", wstatus == "Employees") %>% 
  ggplot() +
  aes(x = time, y = values, colour = Pays) +
  geom_line(size=3) +
  ggtitle("Personnes en emploi travaillant à domicile") +
  xlab("") +
  ylab("% (total de l'emploi entre 15 et 64 ans)") +
  labs(caption="Source: Eurostat, variable lfsa_ehomp") +
  facet_wrap(~ frequenc, dir = "h") +
  theme_bw(base_size = 50)

```


