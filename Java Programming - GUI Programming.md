# Java Programming - GUI Programming

[TOC]

## JavaFX Framework Structure

* `Stage` (i.e. window)
* `Scene` (A big container that carries all UI components)
* `Node` (individual UI)


## Life Cycle

JavaFX run-time does the following to run an application:

* Construct an instance of the specified Application class
* Call `init()`
  * Is **called immediately** after the Application class is loaded and constructed
  * May override to perform initialization **prior to the actual starting of the application**.
  * This method is not called on Application thread
* Call `start()`
  * The main entry point for all JavaFX applications
  * This method is called on the JavaFX Application Thread
* Call `Platform.exit()`


## UI

### GridPane

* `setAlignment(Pos)`: align all content by the given alignment
* `setHgap(double)`: set horizontal padding for each grid
* `setVgap(double)`: set vertical padding for each grid
* `add(Node, int columnIndex, int rowIndex, int colspan, int rowspan)`: Adds a child to the gridpane at the specified column,row position and spans.
* `setHalignment(Node, HPos)`(static): Sets the horizontal alignment for the child when contained by a gridpane.
  * e.g. `GridPane.setHalignment(btAdd, HPos.RIGHT);`
  * ~~This method breaks the grid positioning, nonsense!~~


### Insets

An Insets object is a representation of the borders of a container. It specifies the space that a container must leave at each of its edges. The space can be a border, a blank space, or a title.

* `Insets(int top, int left, int bottom, int right)`

### `ListView<T>`

* `getSelectionModel()`: return a multi-selection model
  * `getSelectedItem()`: get the selected item from the selection model


## Binding Properties

* enable a target object to be bound to a source object
* if source object's value changes, then target object's value also changes
* e.g. `pane.widthProperty().divide(2)`

### Unidirectional binding

* one direction binding
* If you use `bind()` to implement bidirectional binding
  * stackoverflow will raise


:::danger
```java
DoubleProperty d1 = new SimpleDoubleProperty(1);
DoubleProperty d2 = new SimpleDoubleProperty(2);
d1.bind(d2);
d2.bind(d1);
```

The above code is dangerous, because if `d1` or `d2` either one be modified, they will update each other and cause stackoverflow! 
:::

### Bidirectional binding

* two-way binding
* done by calling `DoubleProperty.bindBidirectional(Property)`