<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture8.jpg)
Chapter 14
JavaFX Basics
Copyright © 2024 Pearson Education, Inc. All Rights Reserved

![Pearson Logo](PicturePlaceholder21.jpg)

### Notes:
If this PowerPoint presentation contains mathematical equations, you may need to check that your computer has the following installed:
1) MathType Plugin
2) Math Player (free versions available)
3) NVDA Reader (free versions available)

Slides in this presentation contain hyperlinks. JAWS users should be able to get a list of links by using INSERT+F7

<!-- Slide number: 2 -->
# Motivations
JavaF X is a new framework for developing Java G U I programs. The JavaF X A P I is an excellent example of how the object-oriented principle is applied. This chapter serves two purposes. First, it presents the basics of JavaF X programming. Second, it uses JavaF X to demonstrate O O P. Specifically, this chapter introduces the framework of JavaF X and discusses JavaF X G U I components and their relationships.

### Notes:

<!-- Slide number: 3 -->
# Objectives (1 of 2)
14.1 To distinguish between JavaF X, Swing, and A W T (§14.2).
14.2 To write a simple JavaF X program and understand the relationship among stages, scenes, and nodes (§14.3).
14.3 To create user interfaces using panes, U I controls, and shapes (§14.4).
14.4 To use binding properties to synchronize property values (§14.5).
14.5 To use the common properties style and rotate for nodes (§14.6).
14.6 To create colors using the Color class (§14.7).
14.7 To create fonts using the Font class (§14.8).

### Notes:

<!-- Slide number: 4 -->
# Objectives (2 of 2)
14.8 To create images using the Image class and to create image views using the ImageView class (§14.9).
14.9 To layout nodes using Pane, StackPane, FlowPane, GridPane, BorderPane, H Box, and VBox (§14.10).
14.10 To display text using the Text class and create shapes using Line, Circle, Rectangle, Ellipse, Arc, Polygon, and Polyline (§14.11).
14.11 To develop the reusable G U I components ClockPane for displaying an analog clock (§14.12).

<!-- Slide number: 5 -->
# JavaF X versus Swing and A WT
Swing and A W T are replaced by the JavaF X platform for developing rich Internet applications.
When Java was introduced, the G U I classes were bundled in a library known as the Abstract Windows Toolkit (A W T). A W T is fine for developing simple graphical user interfaces, but not for developing comprehensive G U I projects. In addition, A W T is prone to platform-specific bugs. The A W T user-interface components were replaced by a more robust, versatile, and flexible library known as Swing components. Swing components are painted directly on canvases using Java code. Swing components depend less on the target platform and use less of the native G U I resource. With the release of Java 8, Swing is replaced by a completely new G U I platform known as JavaF X.

### Notes:

<!-- Slide number: 6 -->
# Basic Structure of Java F X
Application
Override the start(Stage) method
Stage, Scene, and Nodes

![An object shows the Basic Structure of Java FX. It has 1 box which shows the Scene, 1 outer layer which shows the Stage, and there is also a small box that shows the Button.](Picture6.jpg)
MyJavaFX
MultipleStageDemo

### Notes:
MyJavaFX: https://liveexample.pearsoncmg.com/html/MyJavaFX.html
MultipleStageDemo: https://liveexample.pearsoncmg.com/html/MultipleStageDemo.html

<!-- Slide number: 7 -->
# Panes, U I Controls, and Shapes

![An object (a) shows the Panes, Of Controls, and Shapes. For long description in Notes pane, press F6.](Picture5.jpg)
ButtonInPane

### Notes:
It has 1 box which shows the Parent (Pane, Control). The outer layer shows the Scene. The second outer layer shows the Stage, and there is also a two small box which shows the Nodes. An object (b) also shows the Panes, Of Controls, and Shapes. It has 14 boxes, and there are interrelated with each other. Boxes are Stage, Scene, Node, their parts are Shape, Image View, Control, then Parent and their parts are Control, Pane but Pane also has FlowPane, GridPane, Border Pane, HBox, VBox, and Stack Pane.

ButtonInPane: https://liveexample.pearsoncmg.com/html/ButtonInPane.html

<!-- Slide number: 8 -->
# Display a Shape
This example displays a circle in the center of the pane.

![An object shows the Display a Shape. It has 2 graphs. The left side graph shows the Java Coordinate System. It has points (0,0) from x axis and y axis. The right side graph shows the Conventional Coordinate System. It has points (0,0) from x axis and y axis.](Picture6.jpg)
ShowCircle

### Notes:
ShowCircle: https://liveexample.pearsoncmg.com/html/ShowCircle.html

<!-- Slide number: 9 -->
# Binding Properties
JavaF X introduces a new concept called binding property that enables a target object to be bound to a source object. If the value in the source object changes, the target property is also changed automatically. The target object is simply called a binding object or a binding property.
ShowCircleCentered

### Notes:
ShowCircleCentered: https://liveexample.pearsoncmg.com/html/ShowCircleCentered.html

<!-- Slide number: 10 -->
# Binding Property: Getter, Setter, and Property Getter

![The computer code shows the Binding Property. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 2 boxes that is (a) for X is a binding property and (b) for Centre X Is binding property. Box a has 10 lines. Line 1, public class Some Class Name open braces. Line 2, private Property Type x semicolon. Line 3, slash forward address address Value getter method address slash forward. Line 4, public property Value Type get X open parenthesis close parenthesis open braces period period period close braces. Line 5, slash forward address address Value setter method address slash forward. Line 6, public void set X open parenthesis property Values Type value close parenthesis open braces period period period close braces. Line 7, slash forward address address Property getter method address slash forward. Line 8, public Property Type. Line 9, x Property open parenthesis close parenthesis open braces period period period close braces. Line 10, close braces. Box b, has 9 lines. Line 1, public class Circle open braces. Line 2, private Double Property center X semicolon. Line 3, slash forward address address Value getter method address slash forward. Line 4, public double get Centre X open parenthesis close parenthesis open braces period period period close braces. Line 5, slash forward address address Value setter method address slash forward. Line 6, public void set Centre X open parenthesis double value close parenthesis open braces period period period close braces. Line 7, slash forward address address Property getter method address slash forward. Line 8, public Double Property center X Property open parenthesis close parenthesis open braces period period period close braces. Line 9, close braces.

<!-- Slide number: 11 -->
# Uni/Bidirectional Binding
BindingDemo
BidirectionalBindingDemo

### Notes:
BindingDemo: https://liveexample.pearsoncmg.com/html/BindingDemo.html
BidirectionalBindingDemo: https://liveexample.pearsoncmg.com/html/BidirectionalBindingDemo.html

<!-- Slide number: 12 -->
# Common Properties and Methods for Nodes
style: set a JavaF X C S S style
rotate: Rotate a node
NodeStyleRotateDemo

### Notes:
NodeStyleRotateDemo: https://liveexample.pearsoncmg.com/html/NodeStyleRotateDemo.html

<!-- Slide number: 13 -->
# The Color Class

![The computer code shows the Color Class. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 3 rows. Row 1, shows the java fx period scene period paint period Color. Row 2, has 4 lines.
Line 1, minus red colon double.
Line 2, minus green colon double.
Line 3, minus blue colon double.
Line 4, minus opacity colon double. Row 3, has 12 lines.
Line 1, plus Color open parenthesis r colon double comma g colon double comma b colon. The red value of this Color (between 0.0 and 1.0).
Line 2, double comma opacity colon double close braces. The green value of this Color (between 0.0 and 1.0).
Line 3, plus brighter open parenthesis close parenthesis colon Color. The blue value of this Color (between 0.0 and 1.0).
Line 4, plus darker open parenthesis close parenthesis colon Color. The opacity of this Color (between 0.0 and 1.0).
Line 5, plus color open parenthesis r colon double comma g colon double comma b colon. Creates a Color with the specified red, green, blue, and opacity values.
Line 6, double close parenthesis colon Color. Creates a Col or that is a brighter version of this Color.
Line 7, plus color open parenthesis r colon double comma g colon double comma b colon. Creates a Color that is a darker version of this Color.
Line 8, double comma opacity colon double close parenthesis colon Color. Creates an opaque Col or with the specified red, green, and blue values.
Line 9, plus rgb open parenthesis r colon int comma g colon int comma b colon int comma Color. Creates a Color with the specified red, green, and blue values in the range from 0 to 255.
Line 10, plus rgb open parenthesis r colon int comma g colon int comma b colon int comma opacity colon double close parenthesis colon Color. Creates a Color with the specified red, green, and blue values in the range from 0 to 255 and a given opacity.
The first line of codes is labeled, getter methods for property values are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 14 -->
# The Font Class

![The computer code shows the Font Class. For long description in Notes pane, press F6.](Picture6.jpg)
FontDemo

### Notes:
It has 3 rows. Row 1, java fx period scene period text period Font. Row 2, has 3 lines.
Line 1, minus size colon double. The size of this font.
Line 2, minus name colon String. The name of this font.
Line 3, minus family colon String. The family of this font.
Row 3, has 11 lines.
Line 1, plus Font open parenthesis size colon double close parenthesis. Creates a Font with the specified size.
Line 2, plus Font open parenthesis name colon String comma size colon double close parenthesis. Creates a Font with the specified full font name and size.
Line 3, plus Font open parenthesis name colon String comma size colon double close parenthesis. Creates a Font with the specified name and size.
Line 4, plus Font open parenthesis name colon String comma w colon Font Weight comma size colon double close parenthesis. Creates a Font with the specified name, weight, and size.
Line 5, plus Font open parenthesis name colon String comma w colon Font Weight comma p colon Font Posture comma size colon double close parenthesis. Creates a Font with the specified name, weight, posture, and size.
Line 6, plus get Families open parenthesis close parenthesis colon List less than String greater than. Returns a list of font family names.
Line 7, plus get Font Nanes open parenthesis close parenthesis colon List less than String greater than. Returns a list of full font names including family and weight.
The first line of codes is labeled, the getter methods for property values are provided in the class, but omitted in the UML diagram for brevity.

FontDemo: https://liveexample.pearsoncmg.com/html/FontDemo.html

<!-- Slide number: 15 -->
# The Image Class

![The computer code shows the Image Class. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
It has 3 rows. Row 1, java fx period scene period image period Image. Row 2, has 4 lines.
Line 1, minus error colon Read Only Boolean Property. Indicates whether the image is loaded correctly?
Line 2, minus height colon Read Only Boolean Property. The height of the image.
Line 3, minus width colon Read Only Boolean Property. The width of the image.
Line 4, minus progress colon Read Only Boolean Property. The approximate percentage of image's loading that is completed.
The first line of code is labeled, the getter methods for property values are provided in the class, but omitted in the UML diagram for brevity.
Row 3, plus Image open parenthesis file name Or URL colon String close parenthesis. Creates an Image with contents loaded from a file or a URL.

<!-- Slide number: 16 -->
# The ImageView Class

![The computer code shows the Image View Class. For long description in Notes pane, press F6.](Picture5.jpg)
ShowImage

### Notes:
 It has 3 rows. Row 1, java fx period scene period Image period Image View. Row 2, has 5 lines.
Line 1, minus fit Height colon Double Property. The height of the bounding box within which the image is resized to fit.
Line 2, minus fit Width colon Double Property. The width of the bounding box within which the image is resized to fit.
Line 3, minus x colon Double Property. The x coordinate of the ImageView origin.
Line 4, minus y colon Double Property. The y coordinate of the ImageView origin.
Line 5, minus image colon Object Property less than Image greater than. The image to be displayed in the image view.
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 3 lines.
Line 1, plus Image View open parenthesis close parenthesis. Creates an ImageView.
Line 2, plus Image View open parenthesis image colon Image close parenthesis. Creates an ImageVi ew with the specified image.
Line 3, plus Image View open parenthesis file name Or URL colon String close parenthesis. Creates an ImageVi ew with image loaded from the specified file or URL.

ShowImage: https://liveexample.pearsoncmg.com/html/ShowImage.html

<!-- Slide number: 17 -->
# Layout Panes
Java F X provides many types of panes for organizing nodes in a container.
| Class | Description |
| --- | --- |
| Pane | Base class for layout panes. It contains the getChildren() method forreturning a list of nodes in the pane. |
| StackPane | Places the nodes on top of each other in the center of the pane. |
| FlowPane | Places the nodes row-by-row horizontally or column-by-column vertically. |
| GridPane | Places the nodes in the cells in a two-dimensional grid. |
| BorderPane | Places the nodes in the top. right, bottom, left, and center regions. |
| H Box | Places the nodes in a single row. |
| V box | Places the nodes in a single column. |

### Notes:

<!-- Slide number: 18 -->
# FlowPane

![The computer code shows the Flow Pane. For long description in Notes pane, press F6.](Picture6.jpg)
MultipleStageDemo

### Notes:
It has 3 rows. Row 1, java fx period scene period layout period Flow Pane. Row 2, has 4 lines.
Line 1, minus alignment colon Object Property less than Pos greater than. The overall alignment of the content in this pane (default, Pos. LEFT).
Line 2, minus orientation colon Object Property less than Orientation greater than. The orientation in this pane (default, Orientation. HORIZONTAL).
Line 3, minus hgap colon Double Property. The horizontal gap between the nodes (default, 0).
Line 4, minus vgap colon Double Property. The vertical gap between the nodes (default, 0).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 4 lines.
Line 1, plus Flow Pane open parenthesis close parenthesis. Creates a default FlowPane.
Line 2, plus Flow Pane open parenthesis hgap colon double comma vgap colon double close parenthesis. Creates a FlowPane with a specified horizontal and vertical gap.
Line 3, plus Flow Pane open parenthesis Orientation colon Object Property less than Orientation greater than. Creates a FlowPane with a specified orientation.
Line 4, plus Flow Pane open parenthesis Orientation colon Object Property less than Orientation greater than comma hgap colon double comma vgap colon double. Creates a FlowPane with a specified orientation, horizontal gap and vertical gap.

MultipleStageDemo: https://liveexample.pearsoncmg.com/html/ShowFlowPane.html

<!-- Slide number: 19 -->
# GridPane

![The computer code shows the Grid Pane. For long description in Notes pane, press F6.](Picture6.jpg)
ShowGridPane

### Notes:
 It has rows. Row 1, java fx period scene period layout period Grid Pane. Row 2, has 4 lines.
Line 1, minus alignment colon Object Property less than Pos greater than. The overall alignment of the content in this pane (default, Pos. LEFT).
Line 2, minus grid Lines Visible colon Boolean Property. Is the grid line visible? (default, false)
Line 3, minus hgap colon Double Property. The horizontal gap between the nodes (default, 0).
Line 4, minus vgap colon Double Property. The vertical gap between the nodes (default, 0).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 10 lines.
Line 1, plus Grid Pane open parenthesis close parenthesis. Creates a GridPane. Creates a GridPane.
Line 2, plus add open parenthesis child colon Node comma column Index colon int comma row Index colon int close parenthesis colon void. Adds a node to the specified column and row.
Line 3, plus add Column open parenthesis column Index colon int comma children colon Node ellipsis close parenthesis colon void. Adds multiple nodes to the specified column.
Line 4, plus add Row open parenthesis row Index colon int comma children colon Node ellipsis close parenthesis colon void. Adds multiple nodes to the specified row.
Line 5, plus get Column Index open parenthesis child colon Node close parenthesis colon int. Returns the column index for the specified node.
Line 6, plus set Column Index open parenthesis child colon Node comma column Index colon int close parenthesis colon void. Sets a node to a new column. This method repositions the node.
Line 7, plus get Row Index open parenthesis child colon Node close parenthesis colon int. Returns the row index for the specified node.
Line 8, plus set Row Index open parenthesis child colon Node comma row Index colon int close parenthesis colon void. Sets a node to a new row. This method repositions the node.
Line 9, plus set H alignment open parenthesis child colon Node comma value colon HPos close parenthesis colon void. Sets the horizontal alignment for the child in the cell.
Line 10, plus set V alignment open parenthesis child colon Node comma value colon VPos close parenthesis colon void. Sets the vertical alignment for the child in the cell.

ShowGridPane: https://liveexample.pearsoncmg.com/html/ShowGridPane.html

<!-- Slide number: 20 -->
# BorderPane

![The computer code shows the Border Pane. For long description in Notes pane, press F6.](Picture6.jpg)
ShowBorderPane

### Notes:
It has 3 rows. Row 1, java fx period scene period layout period Border Pane. Row 2, has 5 lines.
Line 1, minus top colon Object Property less than Node greater than. The node placed in the top region (default, nul 1).
Line 2, minus right colon Object Property less than Node greater than. The node placed in the right region (default, null).
Line 3, minus bottom colon Object Property less than Node greater than. The node placed in the bottom region (default, null).
Line 4, minus left colon Object Property less than Node greater than. The node placed in the left region (default, null).
Line 5, minus center colon Object Property less than Node greater than. The node placed in the center region (default, null).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 2 lines.
Line 1, plus Border Pane open parenthesis close parenthesis. Creates a BorderPane.
Line 2, plus set Alignment open parenthesis child colon Node comma pos colon Pos close parenthesis. Sets the alignment of the node in the BorderPane.

ShowBorderPane: https://liveexample.pearsoncmg.com/html/ShowBorderPane.html

<!-- Slide number: 21 -->
# H Box

![The computer code shows the HBox. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
 It has 3 rows. Row 1, java fx period scene period layout period HBox. Row 2, has 3 lines.
Line 1, minus alignment colon Object Property less than Pos greater than. The overall alignment of the children in the box (default, Pos. TOP_LEFT).
Line 2, minus fill Height colon Boolean Property. Is resizable children fill the full height of the box (default, true).
Line 3, minus spacing colon Double Property. The horizontal gap between two nodes (default, 0).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 3 lines.
Line 1, plus H Box open parenthesis close parenthesis. Creates a default HBox.
Line 2, plus H Box open parenthesis spacing colon double close parenthesis. Creates an HBox with the specified horizontal gap between nodes.
Line 3, plus set Margin open parenthesis node colon Node comma value colon Insets close parenthesis colon void. Sets the margin for the node in the pane.

<!-- Slide number: 22 -->
# V Box

![The computer code shows the VBox. For long description in Notes pane, press F6.](Picture6.jpg)
ShowHBoxVBox

### Notes:
 It has 3 rows. Row 1, java fx period scene period layout period VBox. Row 2, has 3 lines.
Line 1, minus alignment colon Object Property less than Pos greater than. The overall alignment of the children in the box (default, Pos. TOP_LEFT).
Line 2, minus fill Width colon Boolean Property. Is resizable children fill the full width of the box (default, true).
Line 3, minus spacing colon Double Property. The vertical gap between two nodes (default, 0).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 3 lines.
Line 1, plus V Box open parenthesis close parenthesis. Creates a default VBox.
Line 2, plus V Box open parenthesis spacing colon double close parenthesis. Creates a VBox with the specified horizontal gap between nodes.
Line 3, plus set Margin open parenthesis node colon Node comma value colon Insets close parenthesis colon void. Sets the margin for the node in the pane.

ShowHBoxVBox: https://liveexample.pearsoncmg.com/html/ShowHBoxVBox.html

<!-- Slide number: 23 -->
# Shapes
JavaF X provides many shape classes for drawing texts, lines, circles, rectangles, ellipses, arcs, polygons, and polylines.

![An object shows the Shapes. It has 2 boxes but second box is divided into 8 boxes and they are interrelated with each other. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
 Box 1, shows the Node. Box 2, shows the Shape, it makes an arrow forward to represent the Box 1 and it's divided into 8 boxes that is Text, Line, Rectangle, Circle, Ellipse, Arc, Polygon, Polyline.

<!-- Slide number: 24 -->
# Text

![The computer code shows the Text. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 3 rows. Row 1, java fx period scene period text period Text. Row 2, has 6 lines.
Line 1, minus text colon String Property. Defines the text to be displayed.
Line 2, minus x colon Double Property. Defines the x coordinate of text (default 0).
Line 3, minus y colon Double Property. Defines the y coordinate of text (default 0).
Line 4, minus underline colon Boolean Property. Defines if each line has an underline below it (default false).
Line 5, minus strike through colon Boolean Property. Defines if each line has a line through it (default false).
Line 6, minus font colon Object Property less than Font greater than. Defines the font for the text.
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 3 lines.
Line 1, plus Text open parenthesis close parenthesis. Creates an empty Text.
Line 2, plus Text open parenthesis text colon String close parenthesis. Creates a Text with the specified text.
Line 3, plus Text open parenthesis x colon double comma y colon double comma text colon String close parenthesis. Creates a Text with the specified x, y coordinates and text.

<!-- Slide number: 25 -->
# Text Example

![An object shows the Text Example. For long description in Notes pane, press F6.](Picture6.jpg)
ShowText

### Notes:
It has 2 boxes that is (a) for Text (x, y, text) and (b) for Three Text objects are displayed. In box (a), the upward side 2 corners shows the (0,0), (get Width (),0), then in the mid shows the (x,y) indicates text is displayed and the downward side 2 corners shows the (0, get Height()), (get Width (), get Height ()). In box (b), shows the 5 lines that is Programing is fun, Programming is fun, Display text, Programing is fun and Display text but last two have been cut.

ShowText: https://liveexample.pearsoncmg.com/html/ShowText.html

<!-- Slide number: 26 -->
# Line

![The computer code shows the Line. For long description in Notes pane, press F6.](Picture10.jpg)

![An object shows the Line. It has one box. In box, the upward side 2 corners shows the (0,0), (get Width (),0), then in the mid shows the (startX, startY), (endX, endY) and the downward side 2 corners shows the (0, get Height()), (get Width(), get Height ()).](Picture12.jpg)
ShowLine

### Notes:
It has 3 rows. Row 1, java fx period scene period shape period Line. Row 2, has 4 lines.
Line 1, minus start X colon Double Property. The x coordinate of the start point.
Line 2, minus start Y colon Double Property. The y coordinate of the start point.
Line 3, minus end X colon Double Property. The x coordinate of the end point.
Line 4, minus end Y colon Double Property. The y coordinate of the end point.
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class but omitted in the UML diagram for brevity.
Row 3, has 2 lines.
Line 1, plus Line open parenthesis close parenthesis. Creates an empty Line.
Line 2, plus Line open parenthesis start X colon double comma start Y colon double comma end X colon double comma end Y colon double close parenthesis. Creates a Line with the specified starting and ending points.

ShowLine: https://liveexample.pearsoncmg.com/html/ShowLine.html

<!-- Slide number: 27 -->
# Rectangle

![The computer code shows the Rectangle. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 3 rows. Row 1, java fx period scene period shape period Rectangle. Row 2, has 6 lines.
Line 1, minus X colon Double Property. The x coordinate of the upper left corner of the rectangle (default 0).
Line 2, minus Y colon Double Property. The y coordinate of the upper left corner of the rectangle (default 0).
Line 3, minus width colon Double Property. The width of the rectangle (default, 0).
Line 4, minus height colon Double Property. The height of the rectangle (default, 0).
Line 5, minus arc width colon Double Property. The arcWidth of the rectangle (default, 0). arcWidth is the horizontal diameter of the arcs at the corner (see Figure 14.31a).
Line 6, minus arc Height colon Double Property. The arcHeight of the rectangle (default, 0). arcHeight is the vertical diameter of the arcs at the corner (see Figure 14.31a).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 2 lines.
Line 1, plus Rectangle open parenthesis close parenthesis. Creates an empty Rectangle.
Line 2, plus Rectangle open parenthesis X colon double comma Y colon double comma width colon double comma height colon double close parenthesis. Creates a Rectangle with the specified upper left corner point, width, and height.

<!-- Slide number: 28 -->
# Rectangle Example

![An object shows the Rectangle Example. For long description in Notes pane, press F6.](Picture6.jpg)
ShowRectangle

### Notes:
It has one box (a) that is Rectangle (x, y, w, h) but corners are rounded in shape. The upward left hand side corner shows the (x ah/2, y aw/2). The downward line of the box shows the width. The rightward line of the box shows the height.

ShowRectangle: https://liveexample.pearsoncmg.com/html/ShowRectangle.html

<!-- Slide number: 29 -->
# Circle

![The computer code shows the Circle. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 3 rows. Row 1, java fx period scene period shape period Circle. Row 2, has 3 lines.
Line 1, minus center X colon Double Property. The x coordinate of the center of the circle (default 0).
Line 2, minus center Y colon Double Property. The y coordinate of the center of the circle (default 0).
Line 3, minus radius colon Double Property. The radius of the circle (default, 0).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 3 lines.
Line 1, plus Circle open parenthesis close parenthesis. Creates an empty Circle.
Line 2, plus Circle open parenthesis X colon double comma Y colon double close parenthesis. Creates a Circle with the specified center.
Line 3, plus Circle open parenthesis x colon double comma y colon double comma radius colon double close parenthesis. Creates a Circle with the specified center and radius.

<!-- Slide number: 30 -->
# Ellipse

![The computer code shows the Ellipse. For long description in Notes pane, press F6.](Picture7.jpg)

![An object shows the shape of Ellipse. It's make a lines at the centre. The vertical line shows the radius Y. The horizontal line shows the radius X. The mid point shows the (center X, center Y).](Picture10.jpg)
ShowEllipse

### Notes:
It has 3 rows. Row 1, java fx period scene period shape period Ellipse. Row 2, has 4 lines.
Line 1, minus center X colon Double Property. The x coordinate of the center of the ellipse (default 0).
Line 2, minus center Y colon Double Property. The y coordinate of the center of the ellipse (default 0).
Line 3, minus radius X colon Double Property. The horizontal radius of the ellipse (default, 0).
Line 4, minus radius Y colon Double Property. The vertical radius of the ellipse (default, 0).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 3 lines.
Line 1, plus Ellipse open parenthesis close parenthesis. Creates an empty Ellipse.
Line 2, plus Ellipse open parenthesis X colon double comma Y colon double close parenthesis. Creates an Ellipse with the specified center.
Line 3, plus Ellipse open parenthesis x colon double comma y colon double comma radius X colon double comma radius Y colon double close parenthesis. Creates an Ellipse with the specified center and radiuses.

ShowEllipse: https://liveexample.pearsoncmg.com/html/ShowEllipse.html

<!-- Slide number: 31 -->
# Arc

![The computer code shows the Arc.For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
t has 3 rows. Row 1, java fx period scene period shape period Arc. Row 2, has 7 lines.
Line 1, minus center X colon Double Property. The x coordinate of the center of the ellipse (default 0).
Line 2, minus center Y colon Double Property. The y coordinate of the center of the ellipse (default 0).
Line 3, minus radius X colon Double Property. The horizontal radius of the ellipse (default, 0).
Line 4, minus radius Y colon Double Property. The vertical radius of the ellipse (default, 0).
Line 5, minus start Angle colon Double Property. The start angle of the arc in degrees.
Line 6, minus length colon Double Property. The angular extent of the arc in degrees.
Line 7, minus type colon Object Property less than Arc Type greater than. The closure type of the arc (ArcType.OPEN, ArcType.CHORD, ArcType.ROUND).
The first line of codes is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 2 lines.
Line 1, plus Arc open parenthesis close parenthesis. Creates an empty Arc.
Line 2, plus Arc open parenthesis X colon double comma Y colon double comma radius X colon double comma radius Y colon double comma start Angle colon double comma length colon double close parenthesis. Creates an Arc with the specified arguments.

<!-- Slide number: 32 -->
# Arc Examples

![A diagram shows an ellipse with center labeled, (centerX, centerY). Positive x axis shows 0 degree. For long description in Notes pane, press F6.](Picture7.jpg)

![A diagram shows two arc examples. For long description in Notes pane, press F6.](Picture10.jpg)
ShowArc

### Notes:
A line from center to the ellipse makes an angle labeled, start angle. The line is labeled, length. The radius of the ellipse along x axis is labeled, radius and that along y axis is labeled, radius Y.

(a) Negative starting angle, negative 30 degrees and negative spanning angle, negative 20 degrees. Two angles inside an ellipse. The angle from positive x axis to a line below it in clockwise direction is labeled, negative 30 degrees and an angle from this 30 degree line to further below in clockwise direction is labeled, negative 20 degree.
(b) Negative starting angle, negative 50 degrees and positive spanning angle, 20 degrees. Two angles inside an ellipse. The angle from positive x axis to a line below it in clockwise direction is labeled, negative 50 degrees and an angle from this 50 degree line to further below in counterclockwise direction is labeled, 20 degree.

ShowArc: https://liveexample.pearsoncmg.com/html/ShowArc.html

<!-- Slide number: 33 -->
# Polygon and Polyline

![An object shows the Polygon and Polyline. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 2 boxes (a) for Polygon and (b) for Polyline. Box (a), shows the shape of polygon and the upward side 2 corners shows the (x[0], y[0]), (x[0], y[0]), then in the mid shows the (x[3], y[3]) and the downward side 2 corners shows the (x[4], y[4]), (x[2], y[2]). Box (b), shows the shape of polyline and the upward side 2 corners shows the (x[0], y[0]), (x[1], y[1]), then in the mid shows the (x[3], y[3]) and the downward side 2 corners shows the (x[4], y[4]), (x[2], y[2]).

<!-- Slide number: 34 -->
# Polygon

![The computer code shows the Polygon. For long description in Notes pane, press F6.](Picture6.jpg)
ShowPolygon

### Notes:
It has 2 rows. Row 1, java fx period scene period shape period Polygon. Row 2, has 3 lines.
Line 1, plus Polygon open parenthesis close parenthesis. Creates an empty polygon.
Line 2, plus Polygon open parenthesis double period period period points close parenthesis. Creates a polygon with the given points.
Line 3, plus get Points open parenthesis close parenthesis Observable List less than Double greater than. Returns a list of double values as x and y coordinates of the points.
The first line of code is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

ShowPolygon: https://liveexample.pearsoncmg.com/html/ShowPolygon.html

<!-- Slide number: 35 -->
# Case Study: The ClockPane Class
This case study develops a class that displays a clock on a pane.

![The computer code shows the Case Study for the Clock Pane Class. For long description in Notes pane, press F6.](Picture7.jpg)
ClockPane

### Notes:
It has 2 boxes. Box 1, java fx period scene period layout period Panel. Box 2, has 3 rows and makes an arrow upward to represent the Box 1. Row 1, shows the Clock Pane. Row 2, has 3 lines.
Line 1, minus hour colon int. The hour in the clock.
Line 2, minus minute colon int. The minute in the clock.
Line 3, minus second colon int. The second in the clock.
The codes in row 2 are labeled, the getter and setter methods for these data fields are provided in the class but omitted in the UML diagram for brevity.
Row 3, has 5 lines.
Line 1, plus Clock Pane open parenthesis close parenthesis. Constructs a default clock for the current time.
Line 2, plus Clock Pane open parenthesis hour colon int comma minute colon int comma second colon double close parenthesis. Constructs a clock with the specified time.
Line 3, plus set Current Time open parenthesis close parenthesis colon void. Sets hour, minute, and second for current time.
Line 4, plus set Width open parenthesis width colon double close parenthesis colon void. Sets clock pane's width and repaint the clock.
Line 5, plus set Height Time open parenthesis height colon double close parenthesis colon void. Sets clock pane's height and repaint the clock.

ClockPane: https://liveexample.pearsoncmg.com/html/ClockPane.html

<!-- Slide number: 36 -->
# Use the ClockPane Class
DisplayClock

### Notes:
DisplayClock: https://liveexample.pearsoncmg.com/html/DisplayClock.html

<!-- Slide number: 37 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes: