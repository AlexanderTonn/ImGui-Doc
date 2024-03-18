# Table of Contents

- [Table of Contents](#table-of-contents)
- [Window](#window)
  - [Begin / End](#begin--end)
  - [Window Flags](#window-flags)
    - [Usecase: Fit ImGui Widgets to window size](#usecase-fit-imgui-widgets-to-window-size)
- [Widgets](#widgets)
  - [Labels](#labels)
    - [Hiding labels](#hiding-labels)
  - [Buttons](#buttons)
  - [Radio Buttons](#radio-buttons)
  - [Combobox](#combobox)
  - [Menubar](#menubar)
    - [Menu Begin / End](#menu-begin--end)
    - [Menu Item](#menu-item)
  - [PopUp Windows](#popup-windows)
- [Arrangement](#arrangement)
  - [Spacing()](#spacing)
  - [Dummy()](#dummy)
  - [Separator Text](#separator-text)
  - [Separator](#separator)
  - [Columns](#columns)
- [Create new ImGui Windows](#create-new-imgui-windows)
  - [Begin / End Child](#begin--end-child)

> [!CAUTION]
> Please note that this documentation is a created while using the ImGui library. The documentation is based on the official documentation and the experience of the author. If you have any suggestions or improvements, please feel free to contribute to the documentation.
> Please also consider that the given code snippets are not complete and may not work as expected. They only show the usage of the functions and classes.

---

# Window



## Begin / End

The `ImGui::Begin` and `ImGui::End` functions are used to create and end a new window, respectively. The `ImGui::Begin` function takes a label and an optional set of flags as parameters, and returns a boolean value that indicates whether the window is currently open. The `ImGui::End` function takes no parameters and simply ends the window.

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

## Window Flags
If you create a Window with ImGui::Begin() you can pass a set of flags to the function. The flags are used to customize the appearance and behavior of the window. The flags are defined in the `ImGuiWindowFlags` enum, and can be combined using the bitwise OR operator (`|`).

### Usecase: Fit ImGui Widgets to window size
The following example shows how you can realize that the ImGui Widgets are filling out the window size. This is useful if you want to create a fullscreen window with ImGui.

```cpp
// declaration
    constexpr static auto window_flags = ImGuiWindowFlags_NoDecoration |
                                         ImGuiWindowFlags_NoMove |
                                         ImGuiWindowFlags_MenuBar ;
    constexpr static auto window_size = ImVec2(1280.0F, 720.0F);
    constexpr static auto window_pos = ImVec2(0.0F, 0.0F);

// usage
    // ... 
    ImGui::SetNextWindowPos(ImVec2(0, 0));
    ImGui::SetNextWindowSize(window_size);
    if (ImGui::Begin("My Window", nullptr, window_flags))
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
> Basically the data type of the label is a `const char*`. It's recommendable to use `std::string_view` for the label, because this is a better c++ approach. You also can save labels in a struct to make it easier to manage the labels (e.g. translations).
>
> - Don't forget to include the header file `<string_view>`

### Hiding labels

If you want to hide the label of a widget, you can add `###` to the label. For example, `ImGui::Button("###Button 1", ImVec2(100, 50));` will create a button without a label.

[up](#table-of-contents)

---

## Buttons

The `ImGui::Button` function returns true when the button was clicked, and false otherwise. Therefore, you can use an if statement to check if the button was clicked, and then perform a corresponding action.

The `ImGui::Button` function also takes an optional second parameter that sets the size of the button. This parameter is an `ImVec2` that specifies the width and height of the button in pixels. If you omit this parameter, the size of the button is automatically calculated based on the text and the current style.

```cpp
if (ImGui::Button("Big Button", ImVec2(200, 50))) {
    // The button was clicked, perform an action
}
```

[up](#table-of-contents)

---

## Radio Buttons
To create a group of three RadioButtons with ImGui, you need a variable to store the selected RadioButton index, and an array or list of labels for each RadioButton. Here's an example of how you can do this:

```cpp
struct ConfigData {
    int iTypeSelection = 0;
};

struct Labels {
    std::array<std::string> deviceTypes = {"Device 1", "Device 2"};
};

int main() {
    ConfigData configData;
    Labels labels;

    // Creates a radio button for "Device 1" and stores the selection in configData.iTypeSelection
    ImGui::RadioButton(labels.deviceTypes[0].c_str(), &configData.iTypeSelection, 1);
    // Sets the cursor to the same line for the next widget
    ImGui::SameLine();
    // Creates a radio button for "Device 2" and stores the selection in configData.iTypeSelection
    ImGui::RadioButton(labels.deviceTypes[1].c_str(), &configData.iTypeSelection, 2);

    return 0;
}
```

In this code, a loop is used to draw each RadioButton. The third parameter of the RadioButton function is the index of this RadioButton. When the user clicks on a RadioButton, selectedRadioButton is set to the index of this RadioButton. You can then use selectedRadioButton to determine which RadioButton was selected.


[up](#table-of-contents)

---

## Combobox

To create an ImGui Combobox, you need the following components:

* **BeginCombo:** This function starts a new combobox. It takes two parameters: a unique identifier and the currently selected text.
* **Selectable:** This function creates a selectable option within the combobox. It takes two parameters: the text to display and a boolean value indicating whether the option is currently selected or not.
* **SetItemDefaultFocus:** This function sets the focus on the currently selected item. It is usually used after an option has been selected.
* **EndCombo:** This function ends the combobox. It should always be called after you are done adding options to the combobox.

```cpp
template <size_t size>
auto drawComboBox(std::string sCbLabel, std::array<std::string, size> sItems, bool xLabelOn) -> int
{
    static size_t iCurrentItem = 0; 

    if(!xLabelOn)
        sCbLabel = "###" + sCbLabel;

    if(ImGui::BeginCombo(sCbLabel.c_str(), sItems[iCurrentItem].c_str()))
    {
        for (size_t i = 0; i < sItems.size(); i++)
        {
            bool isSelected = (iCurrentItem == i);
            if (ImGui::Selectable(sItems[i].c_str(), isSelected))
            {
                iCurrentItem = i;
            }
            if (isSelected)
            {
                ImGui::SetItemDefaultFocus();
            }
        }
        ImGui::EndCombo();
    }
    return static_cast<int>(iCurrentItem);
}
```

This is a template function drawComboBox in the BoilerConfig class. It takes three parameters: a string sCbLabel for the label of the combo box, an array sItems of strings for the items in the combo box, and a boolean xLabelOn to determine whether to prepend "###" to the label.

The function begins by declaring a static variable iCurrentItem to keep track of the currently selected item in the combo box.

If xLabelOn is false, "###" is prepended to sCbLabel. This is a feature of ImGui where "###" is used to create a unique ID for the widget, but it's not displayed.

The function then starts a combo box with ImGui::BeginCombo. The label of the combo box is sCbLabel and the preview value is the current item.

Inside the combo box, it iterates over each item in sItems. For each item, it checks if it's the currently selected item. If the item is selected in the GUI, iCurrentItem is updated to the current index. If the item is the currently selected one, it sets the default focus to this item using ImGui::SetItemDefaultFocus.

After iterating over all items, it ends the combo box with ImGui::EndCombo.

Finally, the function returns the index of the currently selected item as an integer.

[up](#table-of-contents)

---

## Menubar

The menubar is a special widget in ImGui that is used to create a menu bar at the top of the window. The menu bar can contain multiple menus, each of which can contain multiple items. The items in the menu bar can be used to create dropdown menus, which can contain submenus and items.

### Menu Begin / End

The `ImGui::BeginMenuBar` and `ImGui::EndMenuBar` functions are used to create and end a menu bar, respectively. The `ImGui::BeginMenuBar` function takes no parameters and simply creates a menu bar at the top of the window. The `ImGui::EndMenuBar` function takes no parameters and simply ends the menu bar.

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
> If you configured `ImGuiWindowFlags_NoMenuBar` for your window, the menu bar will not be displayed.

### Menu Item

The `ImGui::MenuItem` function is used to create an item in a menu. It takes a label and an optional boolean parameter that indicates whether the item is selected. If the item is selected, it will be highlighted in the menu.

[up](#table-of-contents)

---

## PopUp Windows

The `ImGui::OpenPopup` function is used to open a popup window. It takes a label as a parameter, which is used to identify the popup window. The `ImGui::BeginPopup` and `ImGui::EndPopup` functions are used to create and end a popup window, respectively. The `ImGui::BeginPopup` function takes a label as a parameter, which must match the label passed to `ImGui::OpenPopup`. The `ImGui::EndPopup` function takes no parameters and simply ends the popup window.

Simple Sample for a Popup Window:

```cpp
    ImGui::OpenPopup(sName.data());

    if(ImGui::BeginPopupModal(sName.data(),nullptr, ImGuiWindowFlags_AlwaysAutoResize))
    {
        ImGui::Text("%s", sQuestion.data());
        ImGui::Spacing();

        // OK Button
        if(ImGui::Button(labels.buttons[6].data()))
        {
            ImGui::CloseCurrentPopup();
            return true;
        }
        // Cancel Button
        else if(ImGui::Button(labels.buttons[7].data()))
        {
            ImGui::CloseCurrentPopup();
            return false;
        }
    }


```

# Arrangement

There are several ways to arrange widgets in ImGui, including columns, tables, and custom layouts. Each of these methods has its own advantages and disadvantages, and the best method to use will depend on the specific requirements of your application.

## Spacing()

The `ImGui::Spacing()` function can be used to add a fixed amount of space between widgets. This can be useful when you want to create a gap between widgets, or when you want to align widgets with other elements in the window.

## Dummy()

The `ImGui::Dummy()` is a alternativ to the `ImGui::Spacing()`. It allows to pass an ImVec2 to create a fixed amount of space between widgets.

## Separator Text
The `ImGui::SeparatorText()` function is used to create a horizontal line with text in the middle. It takes a label as a parameter, which is used to display the text in the middle of the line.

## Separator 
The `ImGui::Separator()` function is used to create a horizontal line. It takes no parameters and simply creates a horizontal line.

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
