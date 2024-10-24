# leaflet demo

The `htmlwidgets` package allows you to embed interactive leaflet maps in html-based quarto documents.

### Interactive Leaflet Plots

```
leaflet() |>
  addTiles() |> setView(-111.05, 45.67, zoom = 17)
```

```
leaflet() |>
  addTiles() |>
  addPopups(-111.048, 45.66845, 'Here is our classroom')

```

### Using a dataset

```
#| message: FALSE
set.seed(1019024)
df <- tibble(Long = runif(10, max = -111, min = -111.5), 
             Lat = runif(10, min = 45.5, max = 46),
             size = runif(10, 5, 20),
             color = sample(colors(), 10))
leaflet(df) |>
  addTiles() |> 
  addCircles(lng = ~Long, lat = ~Lat, color = 'maroon')


```

### Map Choropleths

```
library(maps)
mapStates <- map("state", fill = TRUE, plot = FALSE)
leaflet(data = mapStates) %>%
  addTiles() %>%
  addPolygons(fillColor = topo.colors(10, alpha = NULL), stroke = FALSE)
```



```
mt <- map("county",'montana', fill = TRUE, plot = FALSE)
leaflet(mt) %>%
  addTiles() %>%
  addPolygons(fillColor = viridis::rocket(10), stroke = FALSE)
```

### Choropleth from RStudio help files

```

# From https://leafletjs.com/examples/choropleth/us-states.js
states <- sf::read_sf("https://rstudio.github.io/leaflet/json/us-states.geojson")

bins <- c(0, 10, 20, 50, 100, 200, 500, 1000, Inf)
pal <- colorBin("YlOrRd", domain = states$density, bins = bins)

labels <- sprintf(
  "<strong>%s</strong><br/>%g people / mi<sup>2</sup>",
  states$name, states$density
) %>% lapply(htmltools::HTML)

leaflet(states) %>%
  setView(-96, 37.8, 4) %>%
  # addProviderTiles("MapBox", options = providerTileOptions(
  #   id = "mapbox.light",
  #   accessToken = Sys.getenv('MAPBOX_ACCESS_TOKEN'))) %>%
  addPolygons(
    fillColor = ~pal(density),
    weight = 2,
    opacity = 1,
    color = "white",
    dashArray = "3",
    fillOpacity = 0.7,
    highlightOptions = highlightOptions(
      weight = 5,
      color = "#666",
      dashArray = "",
      fillOpacity = 0.7,
      bringToFront = TRUE),
    label = labels,
    labelOptions = labelOptions(
      style = list("font-weight" = "normal", padding = "3px 8px"),
      textsize = "15px",
      direction = "auto")) %>%
  addLegend(pal = pal, values = ~density, opacity = 0.7, title = NULL,
    position = "bottomright")
```

### Demo Questions

```
seattle <- read_csv('http://math.montana.edu/ahoegh/teaching/stat408/datasets/Seattle_911_062016.csv')
```
1. Create a leaflet map with the locations of shoplifting. Use the addMarker function().


2.Create a palette that maps crime type to colors



3. Explore the `markerClusterOptions()` feature in leaflet



4. Leaflet can also be used together with shiny [https://rstudio.github.io/leaflet/articles/shiny.html](https://rstudio.github.io/leaflet/articles/shiny.html). Open a shiny app and explore embedding leaflet.
