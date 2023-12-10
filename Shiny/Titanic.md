```
library(shiny)
library(PerformanceAnalytics)
library(ggplot2)

df <- read.csv("titanic_case.csv")

variable_lst = c("Survived","Pclass", "Male", "Age", "SibSp", "Parch", "Fare")
categorical_lst = c("Survived","Pclass", "Male", "SibSp", "Parch")
continues_lst = c( "Age", "Fare", "SurvivalP")

ui <- navbarPage(
  "Analyzing Titanic",
  theme = bslib::bs_theme(bootswatch = "united"),
  
  tabPanel("Correlations", 
           sidebarLayout(
             sidebarPanel(
               selectInput("variable", "Select Variable", variable_lst, multiple = TRUE)
             ),
             mainPanel(
               plotOutput("correlationmatrix")
             )
             )
           ),
  
  tabPanel("Plots", 
           sidebarLayout(
             sidebarPanel(
               selectInput("x", "Select X Variable", continues_lst),
               selectInput("y", "Select Y Variable", rev(continues_lst)),
               selectInput("color", "Select Color Variable", categorical_lst),
               selectInput("facet", "Select Facet Variable", categorical_lst)
             ),
             mainPanel(
               plotOutput("mainplot")
             )
           )
           ),
  
  tabPanel("View Data",
    dataTableOutput("dataset")
  )
)


server <- function(input, output, session) {
  
  correlate <- reactive({
    req(length(input$variable) > 1)
    corrdata <- df[, input$variable]
  })
  
  output$correlationmatrix <- renderPlot(chart.Correlation(correlate(), histogram=TRUE, pch=19))
  
  output$mainplot <- renderPlot(ggplot(data = df,           
                                   mapping = aes(
                                     x = .data[[input$x]],
                                     y = .data[[input$y]],
                                     color = .data[[input$color]]
                                     )) + 
                              geom_point() + 
                              facet_wrap(.data[[input$facet]] ~ .) 
                              )
  
  output$dataset <- renderDataTable(df, 
                                    options = list(pageLength = 10, 
                                                   autoWidth = TRUE,
                                                   columnDefs = list(list( targets = 3, width = '400px')),
                                                   scrollX = TRUE)
                                    )
  
}

shinyApp(ui, server)
```
