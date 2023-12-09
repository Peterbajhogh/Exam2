We can use the useShinyFeedback() in the ui and call one of the feedback functions in the
server

The feedback functions are:
- feedback()
- feedbackWarning()
- feedbackDanger()
- feedbackSuccess()
  
The functions all have three key arguments:
- inputId - the id of the input where the feedback should be placed.
- show - a logical determining whether or not to show the feedback.
- text - the text to display.
_____

#### Feedback Warning 
```
ui <- fluidPage(
  shinyFeedback::useShinyFeedback(),
  numericInput("n", "n", value = 10),
  textOutput("half")
)

server <- function(input, output, session) {
  half <- reactive({
    even <- input$n %% 2 == 0
    shinyFeedback::feedbackWarning("n", !even, "Please select an even number")
    req(even)
    input$n / 2 
  })
  
  output$half <- renderText(half())
}
```

#### Notification - Timed 
```
ui <- fluidPage(
  actionButton("goodnight", "Good night")
)

server <- function(input, output, session) {
  observeEvent(input$goodnight, {
    showNotification("So long")
    Sys.sleep(1)
    showNotification("Farewell", type = "message")
    Sys.sleep(1)
    showNotification("Auf Wiedersehen", type = "warning")
    Sys.sleep(1)
    showNotification("Adieu", type = "error")
  })
}
```

