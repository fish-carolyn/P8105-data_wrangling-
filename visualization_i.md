visualization_i: ggplot basics
================
2023-09-28

Data for plotting today…

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark, NY", 
      USW00022534 = "Molokai, HI",
      USS0023B17S = "Waterhole, WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

Making a plot!

``` r
weather_df |> 
  filter(name == "CentralPark, NY") |> 
ggplot(aes(x = tmin, y = tmax))+
  geom_point(color= "pink")+ 
  theme_linedraw()
```

![](visualization_i_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
ggp_nyc_weather= weather_df |> 
  filter(name == "CentralPark, NY") |> 
ggplot(aes(x = tmin, y = tmax))+
  geom_point(color= "pink")+ 
  theme_linedraw()
```

Fancy plot

``` r
weather_df |> 
  ggplot( aes(x= tmin, y=tmax, color= name))+
  geom_point()+ 
  geom_smooth()+
  theme_linedraw()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_i_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
#adding color to the top applies the color aesthetic to all items in plot 

weather_df |> 
  ggplot( aes(x = tmin, y = tmax))+
  geom_point(aes(color= name), alpha = 0.3)+ 
  geom_smooth(se=FALSE)+
  theme_linedraw()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).
    ## Removed 17 rows containing missing values (`geom_point()`).

![](visualization_i_files/figure-gfm/unnamed-chunk-3-2.png)<!-- -->

``` r
#adding color to the geom_point applies color to the points only 
```

Plot with facets

``` r
weather_df |> 
  ggplot(aes(x= tmin, y= tmax, color=name))+
  geom_point(alpha=0.3)+
  geom_smooth()+
  theme_linedraw()+
  facet_grid(.~name)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_i_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#defaults to alphabetical/ numerical order of group 
```

A different plot, with more than just temperature.

``` r
weather_df |> 
  ggplot(aes(x= date, y= tmax, color= name))+
  geom_point(aes(size= prcp), alpha=0.3)+
  geom_smooth()+
  facet_grid(.~name)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values (`geom_point()`).

![](visualization_i_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Assigning colors

``` r
weather_df |> 
  filter(name== "CentralPark, NY") |> 
  ggplot(aes(x= date, y= tmax))+
  geom_point(color="orange")
```

![](visualization_i_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
#aesthetic mapping means that you are taking variables and assigning them to elements of the plot. So if you want anything to be dictated by the data (like by name, group, etc.) you need to put in aes(). If you want to assign independent of a variable, add color to the object you want that color. 

# He does not love assigning color by hand. 
```
