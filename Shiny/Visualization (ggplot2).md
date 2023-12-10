### Grammar of Graphics

The ggplot2 package allows you to create data visualizations based on the grammar of
graphics which is made up of 7 layers.
1. Data— The data you are interested in visualizing
2. Aesthetics— The scales you will map the data, which is your x and y-axis etc.
3. Geometry— This is where you define the kind of shape you want your visualization to
have.
4. Facets— This is the process of splitting your plot into various subplots to get a clearer
view.
5. Statistics— In this layer, you define statistical summaries or trends in the data.
6. Coordinates— This is where you describe your plotting space.
7. Themes— This is where you can customize non-data elements in your data such as
font, graph fill and outline, and so on.

ggplot() function is where you pass in the data and aesthetics
```
ggplot(data = diamonds,
mapping = aes(
x = cut,
y = price
))
```

Description: ggplot() initializes a ggplot object. It can be used to declare the input data frame for a
graphic and to specify the set of plot aesthetics intended to be common throughout all
subsequent layers unless specifically overridden.

**Geometry Functions**
1. Bar-plots (geom_bar)
2. Box-plots (geom_boxplot)
3. Scatter-plots (geom_point)
4. Line graph (geom_line)
5. Histogram (geom_bin)
_______
### 1. Bar-plots (geom_bar)

<img width="530" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/b138d720-4ff6-438e-a019-c85346eb7c5b">


```
ggplot(data = diamonds,
  mapping = aes(
    x = cut
  )) +
 geom_bar()
```


<img width="528" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/a49ed133-b1a9-4de7-9356-5756171759c9">

```
ggplot(data = diamonds,
  mapping = aes(
  x = cut,
  fill = cut
  )) +
geom_bar()
```


<img width="529" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/d79e1e85-e854-4ae7-8559-f284f71d476c">

```
ggplot(data = diamonds,
mapping = aes(
y = clarity,
fill = clarity
)) +
geom_bar()
```

**BAR-PLOTS (STACKED)**

<img width="526" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/5ae78cd4-891c-4ab1-b3fe-ccac35bbadc7">

```
ggplot(data = diamonds,
mapping = aes(
x = cut,
fill = color)) +
geom_bar()
```

**BAR-PLOTS (GROUPED)**

<img width="526" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/cce00d4a-e9fa-450f-81b6-a102e99b1f62">

```
ggplot(data = diamonds,
mapping = aes(
x = cut,
fill = color)) +
geom_bar(position = "dodge")
```

### 2. Box-plots (geom_boxplot)

<img width="524" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/80f3b3b3-4cf7-4420-8d97-9e2c9dfbc67b">

```
ggplot(data = diamonds,
  mapping = aes(
  x = cut,
  fill = color)) +
geom_bar()
```

<img width="527" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/be732971-684d-4b93-bf6f-2389997a117a">

```
  ggplot(data = diamonds,
    mapping = aes(
    x = cut,
    y = price,
    fill = cut)) +
geom_boxplot()
```

### 3. Scatter-plots (geom_point)

<img width="529" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/faa11b04-424b-4bf7-b5da-ecd194b40464">

```
ggplot(data = diamonds,
  mapping = aes(
  x = price,
  y = carat)) +
geom_point()
```

### 4. Line graph (geom_line)

<img width="527" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/438adb3a-9950-4329-aa76-89de9b6d3f67">

```
ggplot(data = economics,
  mapping = aes(
  x = date,
  y = pop)) +
geom_line()
```

### 5. Histogram (geom_bin)

<img width="527" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/c276c416-31ed-45c0-bdb6-14c77504e79b">

```
ggplot(data = diamonds,
        mapping = aes(x = price)) +
geom_histogram(bins = 50)
```



