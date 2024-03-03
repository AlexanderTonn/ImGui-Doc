# Table of Contents


# Adjust the ImPlot dynamically to Gui Window Size
```cpp

    ImVec2 dynPlotSize;
    dynPlotSize.y = ImGui::GetWindowHeight() - ImGui::GetCursorPos().y;
    dynPlotSize.x = ImGui::GetWindowWidth() - ImGui::GetCursorPos().x;

    if (ImPlot::BeginPlot("###Plot", dynPlotSize, (ImPlotFlags_NoTitle | ImPlotFlags_Crosshairs )  ))
    {
        // ... Your data and plot here... 

        ImPlot::EndPlot();
    }


```

This code snippet is creating an ImPlot widget with its size dynamically adjusted to the size of the current ImGui window.

First, an ```ImVec2``` object named ```dynPlotSize``` is created. ```ImVec2``` is a simple structure from the ImGui library that represents a 2D vector, typically used to store positions or sizes. In this case, it's used to store the size of the plot.

The ```y``` and ```x``` components of ```dynPlotSize``` are then set to the height and width of the current ImGui window, respectively. This is done using the ```ImGui::GetWindowHeight()``` and ```ImGui::GetWindowWidth()``` functions. These functions return the height and width of the current ImGui window, respectively, allowing the plot to resize dynamically with the window.
