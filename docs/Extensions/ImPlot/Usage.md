# Table of Contents
- [Table of Contents](#table-of-contents)
- [Basic Usage](#basic-usage)
- [ImPlot specific functions](#implot-specific-functions)
  - [Setup Axes](#setup-axes)
  - [Limitting the Plot Area](#limitting-the-plot-area)
  - [Drawing Cursors](#drawing-cursors)


# Basic Usage
The basic sequence of using ImPlot in ImGui consists of the following steps:

**BeginPlot:** First, you need to create a plot. This is done with the ImPlot::BeginPlot function. This function takes several parameters, including the title of the plot, the axis titles, and the size of the plot.

```cpp
if (ImPlot::BeginPlot("My Plot", "X-Axis", "Y-Axis")) {
    // ...
}
```
**Plot Data:** Within the plot, you can plot data. There are many functions to plot different types of data, such as ImPlot::PlotLine, ImPlot::PlotScatter, ImPlot::PlotBars, etc. These functions typically take arrays of x and y values and the number of points as parameters.

```cpp
double xs[] = { 1, 2, 3, 4, 5 };
double ys[] = { 1, 4, 9, 16, 25 };
ImPlot::PlotLine("My Data", xs, ys, 5);
```

Interactive Elements: You can also add interactive elements to your plot, like drag lines or drag points. These functions, like ImPlot::DragLineX, typically take a pointer to a value as a parameter. When the user moves the interactive element, the value is updated.

```cpp
double line_pos = 0.5;
if (ImPlot::DragLineX(&line_pos)) {
    // The user has moved the line, line_pos has been updated
}
```

**EndPlot:** Finally, you need to end the plot with ImPlot::EndPlot. This closes the plot and displays it on the screen.

```cpp
ImPlot::EndPlot();
```




# ImPlot specific functions 

> [!WARNING]
>Please note that all plotting functions must be called between ImPlot::BeginPlot and ImPlot::EndPlot. If you try to call plotting functions outside of this block, nothing will be drawn or your program may crash.

## Setup Axes
The ```ImPlot::SetupAxes``` function is used to set up the axes of the plot. This function takes several parameters, including the minimum and maximum values of the x and y axes, the number of ticks, and the format of the ticks.

## Limitting the Plot Area

## Drawing Cursors 

