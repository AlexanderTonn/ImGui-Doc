# Table of Contents
- [Table of Contents](#table-of-contents)
- [Begin / End](#begin--end)
- [Widgets](#widgets)
  - [Labels](#labels)
    - [Hiding labels](#hiding-labels)
  - [Buttons](#buttons)
  - [Menubar](#menubar)
    - [Menu Begin / End](#menu-begin--end)
    - [Menu Item](#menu-item)
- [Arrangement](#arrangement)
  - [Spacing()](#spacing)
  - [Dummy()](#dummy)
  - [Columns](#columns)
- [Create new ImGui Windows](#create-new-imgui-windows)
  - [Begin / End Child](#begin--end-child)

> [!CAUTION]
> Please note that this documentation is a created while using the ImGui library. The documentation is based on the official documentation and the experience of the author. If you have any suggestions or improvements, please feel free to contribute to the documentation.
> Please also consider that the given code snippets are not complete and may not work as expected. They only show the usage of the functions and classes.

---

# Begin / End 
The ```ImGui::Begin``` and ```ImGui::End``` functions are used to create and end a new window, respectively. The ```ImGui::Begin``` function takes a label and an optional set of flags as parameters, and returns a boolean value that indicates whether the window is currently open. The ```ImGui::End``` function takes no parameters and simply ends the window.

The Begin and End functions are essential functions in ImGui also for creating widgets.

```cpp
if (ImGui::Begin("My Window"))
{
    // Here you can add code to draw widgets into the window.
    ImGui::End();
}

```


[up](#table-of-contents)

---

# Widgets 
Widgets are the building blocks of an ImGui interface. They are used to create buttons, sliders, text boxes, and other interactive elements that allow the user to interact with the application. Each widget has its own set of parameters that can be used to customize its appearance and behavior.

## Labels 
ImGui uses labels to manage and store the state of widgets between frames. Each widget in ImGui has an associated state (e.g., whether a button is pressed, the current value of a slider, the position and size of a window, etc.), and this state is identified by the widget's label.

If two widgets have the same label, ImGui cannot correctly manage their state, as it cannot distinguish which state belongs to which widget. This can lead to unexpected behaviors, such as pressing one button affecting the behavior of another button, sliders sharing their values, etc.

Therefore, it's important that each widget has a unique label. If you want to create multiple instances of the same widget (e.g., multiple buttons with the same text), you can append a unique suffix to the label to ensure each widget has a unique label. For example, you could use the labels "Button 1", "Button 2", "Button 3", etc., to create multiple buttons.

> [!TIP]
Basically the data type of the label is a ```const char*```. It's recommendable to use ```std::string_view``` for the label, because this is a better c++ approach. You also can save labels in a struct to make it easier to manage the labels (e.g. translations). 
> * Don't forget to include the header file ```<string_view>```

### Hiding labels 
If you want to hide the label of a widget, you can add ```###``` to the label. For example, ```ImGui::Button("###Button 1", ImVec2(100, 50));``` will create a button without a label.

[up](#table-of-contents)

---

## Buttons
The ```ImGui::Button``` function returns true when the button was clicked, and false otherwise. Therefore, you can use an if statement to check if the button was clicked, and then perform a corresponding action.

The ```ImGui::Button``` function also takes an optional second parameter that sets the size of the button. This parameter is an ```ImVec2``` that specifies the width and height of the button in pixels. If you omit this parameter, the size of the button is automatically calculated based on the text and the current style.

```cpp
if (ImGui::Button("Big Button", ImVec2(200, 50))) {
    // The button was clicked, perform an action
}
```

[up](#table-of-contents)

---

## Menubar
The menubar is a special widget in ImGui that is used to create a menu bar at the top of the window. The menu bar can contain multiple menus, each of which can contain multiple items. The items in the menu bar can be used to create dropdown menus, which can contain submenus and items.

### Menu Begin / End
The ```ImGui::BeginMenuBar``` and ```ImGui::EndMenuBar``` functions are used to create and end a menu bar, respectively. The ```ImGui::BeginMenuBar``` function takes no parameters and simply creates a menu bar at the top of the window. The ```ImGui::EndMenuBar``` function takes no parameters and simply ends the menu bar.

```cpp

if (ImGui::BeginMenuBar())
{
    if (ImGui::BeginMenu("File"))
    {
        if (ImGui::MenuItem("Open", "Ctrl+O")) { /* Do something */ }
        if (ImGui::MenuItem("Save", "Ctrl+S")) { /* Do something */ }
        if (ImGui::MenuItem("Close", "Ctrl+W")) { /* Do something */ }
        ImGui::EndMenu();
    }
    if (ImGui::BeginMenu("Edit"))
    {
        if (ImGui::MenuItem("Cut", "Ctrl+X")) { /* Do something */ }
        if (ImGui::MenuItem("Copy", "Ctrl+C")) { /* Do something */ }
        if (ImGui::MenuItem("Paste", "Ctrl+V")) { /* Do something */ }
        ImGui::EndMenu();
    }
    ImGui::EndMenuBar();
}
```

> [!NOTE]
> If you configured  ```ImGuiWindowFlags_NoMenuBar``` for your window, the menu bar will not be displayed.

### Menu Item
The ```ImGui::MenuItem``` function is used to create an item in a menu. It takes a label and an optional boolean parameter that indicates whether the item is selected. If the item is selected, it will be highlighted in the menu.


[up](#table-of-contents)

---

# Arrangement
There are several ways to arrange widgets in ImGui, including columns, tables, and custom layouts. Each of these methods has its own advantages and disadvantages, and the best method to use will depend on the specific requirements of your application.

## Spacing() 
The ```ImGui::Spacing()``` function can be used to add a fixed amount of space between widgets. This can be useful when you want to create a gap between widgets, or when you want to align widgets with other elements in the window.

## Dummy() 
The ```ImGui::Dummy()``` is a alternativ to the ```ImGui::Spacing()```. It allows to pass an ImVec2 to create a fixed amount of space between widgets.
## Columns

[up](#table-of-contents)

---

# Create new ImGui Windows
## Begin / End Child
BeginChild and EndChild are functions in the ImGui library that are used to create a child window or area within another window. They are useful when you want to create areas within your GUI that have their own scrollbars or background.

The BeginChild function starts a new child area and must always be paired with a call to EndChild. If you call BeginChild but do not call EndChild, you will get an error message, like the one you have seen.

**A typical use of BeginChild and EndChild might look like this:**

```cpp
ImGui::Begin("My Window");
ImGui::BeginChild("My Child Window");

// Here you can add code to draw widgets into the child window.

ImGui::EndChild();
ImGui::End();
```

If you are getting the error message that BeginChild and EndChild do not match, it means that you have a BeginChild somewhere in your code without a matching EndChild. You should check your code and make sure that every BeginChild is paired with an EndChild.

[up](#table-of-contents)

---