```
library(shiny)
library(ggplot2)
```


#### Single Page Layout

<img width="412" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/9b401309-15de-40f3-9124-9158e4204b9b">

```
fluidPage(
titlePanel(
# app title/description
),
sidebarLayout(
sidebarPanel(
# inputs
),
mainPanel(
# outputs
)
)
)
```
#### Single Page Layout (with Rows)

<img width="432" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/68b70cdc-2446-46c0-9b33-96cb0a8c41c2">


```
fluidPage(
fluidRow(
column(4,
...
),
column(8,
...
)
),
fluidRow(
column(6,
...
),
column(6,
...
)
)
)
```

#### Multipage Layout with tabs 

<img width="613" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/0ef73d1b-778b-49ec-90a4-17e1ebd12877">

```
ui <- fluidPage(
  tabsetPanel(
    tabPanel("Import data", 
             fileInput("file", "Data", buttonLabel = "Upload..."),
             textInput("delim", "Delimiter (leave blank to guess)", ""),
             numericInput("skip", "Rows to skip", 0, min = 0),
             numericInput("rows", "Rows to preview", 10, min = 1)
    ),
    tabPanel("Set parameters"),
    tabPanel("Visualise results")
  )
)
```

#### Multipage Layout with headlines 

<img width="531" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/9cbe1d2a-7554-4103-b7ba-b721de3f64a0">

```
ui <- fluidPage(
  navlistPanel(
    id = "tabset",
    "Heading 1",
    tabPanel("panel 1", "Panel one contents"),
    "Heading 2",
    tabPanel("panel 2", "Panel two contents"),
    tabPanel("panel 3", "Panel three contents")
  )
)

server <- function(input, output, session) {
}
```

#### Multipage Layout using Navbar 

<img width="659" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/5139aba2-292d-4263-bd5d-a2580ead387f">

```
ui <- navbarPage(
  "Page title",   
  tabPanel("panel 1", "one"),
  tabPanel("panel 2", "two"),
  tabPanel("panel 3", "three"),
  navbarMenu("subpanels", 
             tabPanel("panel 4a", "four-a"),
             tabPanel("panel 4b", "four-b"),
             tabPanel("panel 4c", "four-c")
  )
)


server <- function(input, output, session) {
}
```









