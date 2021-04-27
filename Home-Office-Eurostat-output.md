Eurostat Home Office
================
Nicola Cianferoni
27/4/2021

``` r
dat_ho_fr %>% 
  filter(sex == "Total", age == "From 50 to 64 years", wstatus == "Employees") %>% 
  ggplot() +
  aes(x = time, y = values, colour = Pays) +
  geom_line(size=1) +
  ggtitle("Personnes en emploi travaillant à domicile") +
  xlab("") +
  ylab("% (total de l'emploi entre 15 et 64 ans)") +
  labs(caption="Source: Eurostat, variable lfsa_ehomp") +
  facet_wrap(~ frequenc, dir = "h") +
  theme_bw(base_size = 10)
```

![](Home-Office-Eurostat-output_files/figure-gfm/ggplot2-1.png)<!-- -->
