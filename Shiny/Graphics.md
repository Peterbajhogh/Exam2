A plot can respond to four different mouse events
- click (single click)
- dblclick (double click)
- hover (letting the mouse stays in the same place for a little while)
- brush (a rectangular selection)

_____

```
library(shiny)
library(ggplot2)
```

#### Click (single click)
```
ui <- fluidPage(
  plotOutput("plot", click = "plot_click"),
  tableOutput("data")
)
server <- function(input, output, session) {
  output$plot <- renderPlot({
    ggplot(mtcars, aes(wt, mpg)) + geom_point()
  }, res = 96)
  
  output$data <- renderTable({
    req(input$plot_click)
    #browser()
    nearPoints(mtcars, input$plot_click)
  })
}
```

#### Double Click
```
ui <- fluidPage(
  plotOutput("plot", dblclick = "plot_click"),
  tableOutput("data")
)
server <- function(input, output, session) {
  output$plot <- renderPlot({
    ggplot(mtcars, aes(wt, mpg)) + geom_point()
  }, res = 96)
  
  output$data <- renderTable({
    req(input$plot_click)
    nearPoints(mtcars, input$plot_click)
  })
}
```

#### Hover 
```
ui <- fluidPage(
  plotOutput("plot", hover = "plot_click"),
  tableOutput("data")
)
server <- function(input, output, session) {
  output$plot <- renderPlot({
    ggplot(mtcars, aes(wt, mpg)) + geom_point()
  }, res = 96)
  
  output$data <- renderTable({
    req(input$plot_click)
    nearPoints(mtcars, input$plot_click)
  })
}
```

#### Brushing 
```
ui <- fluidPage(
  plotOutput("plot", brush = "plot_brush"),
  tableOutput("data")
)
server <- function(input, output, session) {
  output$plot <- renderPlot({
    ggplot(mtcars, aes(wt, mpg)) + geom_point()
  }, res = 96)
  
  output$data <- renderTable({
    req(input$plot_brush)
    #browser()
    brushedPoints(mtcars, input$plot_brush)
  })
}
```









