<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture3.jpg)
Chapter 16
JavaF X U I Controls and Multimedia
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
A graphical user interface (G U I) makes a system user-friendly and easy to use. Creating a G U I requires creativity and knowledge of how G U I components work. Since the G U I components in Java are very flexible and versatile, you can create a wide assortment of useful user interfaces.
Previous chapters briefly introduced several G U I components. This chapter introduces the frequently used G U I components in detail.

<!-- Slide number: 3 -->
# Objectives (1 of 2)
16.1 To create graphical user interfaces with various user-interface controls (§§16.2–16.11).
16.2 To create a label with text and graphic using the Label class and explore properties in the abstract Labeled class (§16.2).
16.3 To create a button with text and graphic using the Button class and set a handler using the setOnAction method in the abstract ButtonBase class (§16.3).
16.4 To create a check box using the CheckBox class (§16.4).
16.5 To create a radio button using the RadioButton class and group radio buttons using a ToggleGroup (§16.5).
16.6 To enter data using the TextField class and password using the PasswordField class (§16.6).
16.7 To enter data in multiple lines using the TextArea class (§16.7).

<!-- Slide number: 4 -->
# Objectives (2 of 2)
16.8 To select a single item using ComboBox (§16.8).
16.9 To select a single or multiple items using ListView (§16.9).
16.10 To select a range of values using ScrollBar (§16.10).
16.11 To select a range of values using Slider and explore differences between ScrollBar and Slider (§16.11).
16.12 To develop a tic-tac-toe game (§16.12).
16.13 To view and play video and audio using the Media, MediaPlayer, and MediaView (§16.13).
16.14 To develop a case study for showing the national flag and play anthem (§16.14).

<!-- Slide number: 5 -->
# Frequently Used U I Controls

![An object shows the Frequently Used UI Controls. For long description in Notes pane, press F6.](Picture7.jpg)
Throughout this book, the prefixes l b l, b t, c h k, r b, t f, p f, t a, c b o, l v, s c b, s l d, and m p are used to name reference variables for Label, Button, CheckBox, RadioButton, TextField, PasswordField, TextArea, ComboBox, ListView, ScrollBar, Slider, and MediaPlayer.

### Notes:
It has many boxes. Box 1 shows the Node. Box 2 shows the Parent, Image View, Media View and it makes an arrow to represent Box 1. Box 3 shows the Control it represents Box 2. Box 4 shows the Labeled, Scroll Bar, Slider, Text Input Control, List View, Combo Box Base it's represent the Box 3. Now, Labeled box has 2 parts that is Button Base, Label. Text Input Control box has 2 boxes that is Text Area, Text Field. Combo Box Base has one box that is Combo Box. Then, the Button base has 3 parts that is Button, Check Box, Toggle Button. The text Field box has one part that is Password Field. At last, the Toggle Button box also has 1 box that is Radio Button.

<!-- Slide number: 6 -->
# Labeled
A label is a display area for a short text, a node, or both. It is often used to label other controls (usually text fields). Labels and buttons share many common properties. These common properties are defined in the Labeled class.

![The computer code shows the Labeled. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 2 rows. Row 1, java fx period scene period control period it was Labeled. Row 2, has 9 lines.
Line 1, minus alignment colon Object Property less than Pos greater than. Specifies the alignment of the text and node in the labeled.
Line 2, minus content Display colon. Specifies the position of the node relative to the text using the constants TOP, BOTTOM, LEFT, and RIGHT defined in ContentDisplay.
Line 3, Object Property less than Content Display greater than.
Line 4, minus graphic colon Object Property less than Node greater than. A graphic for the labeled.
Line 5, minus graphic Text Gap colon Double Property. The gap between the graphic and the text.
Line 6, minus text Fill colon Object Property less than Paint greater than. The paint used to fill the text. The paint used to fill the text.
Line 7, minus text colon String Property. A text for the labeled.
Line 8, minus underline colon Boolean Property. Whether text should be underlined.
Line 9, minus wrap Text colon Boolean Property. Whether text should be wrapped if the text exceeds the width.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 7 -->
# Label
The Label class defines labels.

![An object shows the Labels with a Graphic. A box shows the US flag, which indicates the US 50 states, a Circle, a Rectangle, an Ellipse, and a Javafx which indicates a Pane inside a label.](Picture8.jpg)

![The computer code shows the labels. It has 2 boxes. For long description in Notes pane, press F6.](Picture11.jpg)
LabelWithGraphic

### Notes:
Box 1, java fx period scene period control period it was Labeled. Box 2 has 2 rows and it represents Row 1, java fx period scene period control period Labeled. Row 2 has 3 lines. Line 1, plus Label open parenthesis close parenthesis. Line 2, plus Label open parenthesis text colon String close parenthesis. Line 3, plus Label open parenthesis text colon String comma graphic colon Node close parenthesis.

LabelWithGraphic: https://liveexample.pearsoncmg.com/html/LabelWithGraphic.html

<!-- Slide number: 8 -->
# Buttonbase and Button
A button is a control that triggers an action event when clicked. JavaF X provides regular buttons, toggle buttons, check box buttons, and radio buttons. The common features of these buttons are defined in ButtonBase and Labeled classes.

![The computer code shows the Button Base and Button. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 3 boxes. Box 1, java fx period scene period control period Labeled. Box 2, has 2 rows and it's represents Box 1. Row 1, java fx period scene period control period Button Base. Row 2, has 2 lines. Line 1, minus on Action colon Object Property less than Event Handler. Line 2, less than Action Event greater than. Box 3, has 2 rows and it represents the Box 2. Row 1, java fx period scene period control period Button. Row 2 has 3 lines. Line 1, plus Button open parenthesis close parenthesis. Line 2, plus Button open parenthesis text colon String close parenthesis. Line 3, plus Button open parenthesis text colon String comma graphic colon Node close parenthesis.

<!-- Slide number: 9 -->
# Button Example

![An object shows the Button Example. In the mid of the box, it shows the Java FX Programming and at the downward side shows the Left and Right buttons.](Picture5.jpg)
ButtonDemo

### Notes:
ButtonDemo: https://liveexample.pearsoncmg.com/html/ButtonDemo.html

<!-- Slide number: 10 -->
# CheckBox
A CheckBox is used for the user to make a selection. Like Button, CheckBox inherits all the properties such as onAction, text, graphic, alignment, graphicTextGap, textFill, contentDisplay from ButtonBase and Labeled.

![The computer code shows the Check Box. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 3 boxes. Box 1, java fx period scene period control period it was Labeled. Box 2, has 2 rows and it's represents Box 1. Row 1, java fx period scene period control period Button Base. Row 2, has 2 lines. Line 1, minus on Action colon Object Property less than Event Handler. Line 2, less than Action Event greater than. Box 3, has 3 rows and it represents Box 2. Row 1, java fx period scene period control period Check Box. Row 2, minus selected colon Boolean Property. Row 3, has 2 lines. Line 1, plus Check Box open parenthesis close parenthesis. Line 2, plus Check Box open parenthesis text colon String close parenthesis.

<!-- Slide number: 11 -->
# CheckBox Example

![An object shows the Button Example. In the mid of the box, shows the Java FX Programming, at rightward side shows the 2 check boxes that is for Bold and Italic then, at downward side shows the Left and Right buttons.](Picture6.jpg)
CheckBoxDemo

### Notes:
CheckBoxDemo: https://liveexample.pearsoncmg.com/html/CheckBoxDemo.html

<!-- Slide number: 12 -->
# RadioButton
Radio buttons, also known as option buttons, enable you to choose a single item from a group of choices. In appearance radio buttons resemble check boxes, but check boxes display a square that is either checked or blank, whereas radio buttons display a circle that is either filled (if selected) or blank (if not selected).

![The computer code shows the Radio Button. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 2 boxes. Box 1, has 3 rows. Row 1, java fx period scene period control period Toggle Button. Row 2, has 3 lines. Line 1, minus selected colon Boolean Property. Line 2, minus toggle Group colon. Line 3, Object Property less than Toggle Group greater than. Row 3, has 3 lines. Line 1, plus Toggle Button open parenthesis close parenthesis. Line 2, plus Toggle Button open parenthesis text colon String close parenthesis. Line 3, plus Toggle Button open parenthesis text colon String comma graphic colon Node close parenthesis. Box 2, has 2 rows. Row 1, java fx period scene period control period Radio Button. Row 2, has 2 lines. Line 1, plus Radio Button open parenthesis close parenthesis. Line 2, plus Radio Button open parenthesis text colon String close parenthesis.

<!-- Slide number: 13 -->
# RadioButton Example

![An object shows the Radio Button Example. For long description in Notes pane, press F6.](Picture6.jpg)
RadioButtonDemo

### Notes:
The program for Button Demo. In the mid shows the Java FX Programming. The leftward side shows the three checkboxes that is Red, Green and Blue. The rightward side also shows the 2 checkboxes that is Bold and Italic. And,in the last there are w buttons that is Left and Right.

RadioButtonDemo: https://liveexample.pearsoncmg.com/html/RadioButtonDemo.html

<!-- Slide number: 14 -->
# TextField
A text field can be used to enter or display a string. TextField is a subclass of TextInputControl.

![The computer code shows the Text Field and Text Input Control. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 2 boxes, and the Text Field is the subclass of Text Input Control. Box 1, has 2 rows. Row 1, java fx period scene period control period Text Input Control. Row 2, has 2 lines. Line 1, minus text colon String Property. Line 2, minus editable colon Boolean Property. Box 2, has 3 rows. Row 1, java fx period scene period control period Text Field. Row 2, has 4 lines. Line 1, minus alignment colon Object Property less than Pos greater than. Line 2, minus pref Column Count colon Integer Property. Line 3, minus on Action colon. Line 4, Object Property less than Event Handler less than Action Event greater than greater than. Row 3, has 2 lines. Line 1, plus Text Field open parenthesis close parenthesis. Line 2, plus Text Field open parenthesis text colon String close parenthesis.

<!-- Slide number: 15 -->
# TextField Example

![An object shows the Text Field Example. For long description in Notes pane, press F6.](Picture5.jpg)
TextFieldDemo

### Notes:
The program is for Button Demo. In this, there is a line on the top that is Enter a new message colon, and in front of the box, Programming is fun. Now, on the page shows in the mid that is Programming is fun. The left side shows the e checkboxes that is Red, Green and Blue. The right side also shows the 2 checkboxes that is Bold and Italic. The downward side shows the 2 buttons that is Left and Right.

TextFieldDemo: https://liveexample.pearsoncmg.com/html/TextFieldDemo.html

<!-- Slide number: 16 -->
# TextArea
A TextArea enables the user to enter multiple lines of text.

![The computer code shows the Text Area and Text Input Control. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 2 boxes. Box 1, has 2 rows. Row 1, java fx period scene period control period Text Input Control. Row 2, has 2 lines.
Line 1, minus text colon String Property. The text content of this control.
Line 2, minus editable colon Boolean Property. Indicates whether the text can be edited by the user.
Box 2, has 3 rows. Row 1, java fx period scene period control period Text Area. Row 2, has 3 lines.
Line 1, minus pref Column Count colon Integer Property. Specifies the preferred number of text columns.
Line 2, minus pref Row Count colon Integer Property. Specifies the preferred number of text rows.
Line 3, minus wrap Text colon Boolean Property. Specifies whether the text is wrapped to the next line.
Row 3, has 2 lines.
Line 1, plus Text Area open parenthesis close parenthesis. Creates an empty text area.
Line 2, plus Text Area open parenthesis text colon String close parenthesis. Creates a text area with the specified text.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 17 -->
# TextArea Example

![The computer code shows the Text Area Example. For long description in Notes pane, press F6.](Picture7.jpg)

![On the left side there is one national flag for Canada and the right side is a box with The Canadian national flag is write inside.](Picture11.jpg)
DescriptionPane
TextAreaDemo

### Notes:
It has 4 boxes that are interrelated with each other. Box 1, java fx period scene period layout period Border Pane. Box 2, has 3 rows. Row 1, Description Pane. Row 2, has 2 lines. Line 1, minus LBL Image Title colon Label. Line 2, minus ta Description colon Text Area. Row 3, has 2 lines. Line 1, plus set Image View open parenthesis in colon Image View close parenthesis. Line 2, plus set Description open parenthesis text colon String close parenthesis and it's represent the Bix 1 and Box 3. Box 3, shows the Text Area Demo and this represents the Box 4. Box 4, java fx period application period Application. An object shows the Text Area Demo.

DescriptionPane: https://liveexample.pearsoncmg.com/html/DescriptionPane.html
TextAreaDemo: https://liveexample.pearsoncmg.com/html/TextAreaDemo.html

<!-- Slide number: 18 -->
# ComboBox
A combo box, also known as a choice list or drop-down list, contains a list of items from which the user can choose.

![The computer code shows the Combo Box. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 2 boxes. Box 1, has 2 rows. Row 1, java fx period scene period control period Combo Box Base less than T greater than. Row 2, has 4 lines.
Line 1, minus value colon Object Property less than T greater than. The value selected in the combo box.
Line 2, minus editable colon Boolean Property. Specifies whether the combo box allows user input.
Line 3, minus on Action colon.
Line 4, Object Property less than Event Handler less than Action Event greater than. Specifies the handler for processing the action event.
Box 2, has 3 rows. Row 1, java fx period scene period control period Combo Box less than T greater than. Row 2, has 2 lines.
Line 1, minus items colon Object Property less than Observable List less than T greater than greater than. The items in the combo box popup.
Line 2, minus visible Row Count colon Integer Property. The maximum number of visible rows of the items in the combo box popup.
Row 3, has 2 lines.
Line 1, plus Combo Box open parenthesis close parenthesis. Creates an empty combo box.
Line 2, plus Combo Box open parenthesis items colon Observable List less than T greater than close parenthesis. Creates a combo box with the specified items.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 19 -->
# ComboBox Example
This example lets users view an image and a description of a country’s flag by selecting the country from a combo box.

![An object shows the Combo Box Example. For long description in Notes pane, press F6.](Picture7.jpg)
ComboBoxDemo

### Notes:
In the program shows the Combo Box Demo, on the top there is a line that is Select a country colon and in the box shows Canada. On the left side there is one national flag for Canada and the right side is a box with The Canadian national flag is write inside.

ComboBoxDemo: https://liveexample.pearsoncmg.com/html/ComboBoxDemo.html

<!-- Slide number: 20 -->
# ListView
A list view is a component that performs basically the same function as a combo box, but it enables the user to choose a single value or multiple values.

![Codes and descriptions for javaftscene.control.ListView less than symbol T greater than symbol as follows. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
Hyphen items colon ObjectProperty less than symbol ObservableList less than symbol T greater than symbol greater than symbol. The items in the list view.
Hyphen orientation colon BooleanProperty. Indicates whether the items are displayed horizontally or vertically in the list view.
Hyphen selectionModel colon ObjectProperty less than symbol MultipleSelectionModel less than symbol T greater than symbol less than symbol. Specifies how items are selected. The SelectionModel is also used to obtain the selected items.
+ListView left parenthesis right parenthesis. Creates an empty list view.
+ListView left parenthesis items colon ObservableList less than symbol T greater than symbol. Creates a list view with the specified items.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 21 -->
# Example: Using Listview
This example gives a program that lets users select countries in a list and display the flags of the selected countries in the labels.

![An object shows the Example of Using List View. For long description in Notes pane, press F6.](Picture7.jpg)
ListViewDemo

### Notes:
In the program shows the List View Demo, On the left side there is a list of countries but in these countries choose China and Germany. The right side is a box with The China national flag and The Germany national flag.

ListViewDemo: https://liveexample.pearsoncmg.com/html/ListViewDemo.html

<!-- Slide number: 22 -->
# ScrollBar
A scroll bar is a control that enables the user to select from a range of values. The scrollbar appears in two styles: horizontal and vertical.

![The computer code shows the Scroll Bar. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 3 rows. Row 1, java fx period scene period control period Scroll Bar. Row 2, has 7 lines.
Line 1, minus block Increment colon Double Property. The amount to adjust the scroll bar if the track of the bar is clicked (default colon 10).
Line 2, minus max colon Double Property. The maximum value represented by this scroll bar (default colon 100).
Line 3, minus min colon Double Property. The minimum value represented by this scroll bar (default colon 0).
Line 4, minus unit Increment colon Double Property. The amount to adjust the scroll bar when the increment left parenthesis right parenthesis and decrement left parenthesis right parenthesis methods are called (default colon 1).
Line 5, minus value colon Double Property. Current value of the scroll bar (default colon 0).
Line 6, minus visible Amount colon Double Property. The width of the scroll bar (default colon 15).
Line 7, minus orientation colon Object Property less than Orientation greater than. Specifies the orientation of the scroll bar (default colon HORIZONTAL).
Row 3, has 3 lines.
Line 1, plus Scroll Bar open parenthesis close parenthesis. Creates a default horizontal scroll bar.
Line 2, plus increment open parenthesis close parenthesis. Increments the value of the scroll bar by unitIncrement.
Line 3, plus decrement open parenthesis close parenthesis. Decrements the value of the scroll bar by unitIncrement.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 23 -->
# Scroll Bar Properties

![An object shows the Scroll Bar Properties. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It shows the Bar which indicates the Track, on the left side is a small box which indicates Left button and the line is for Minimal value. In the mid there is a oval shape which indicates the Thumb. On the right side is also a small box which indicates Right button and the line is for Maximal value.

<!-- Slide number: 24 -->
# Example: Using Scrollbars
This example uses horizontal and vertical scrollbars to control a message displayed on a panel. The horizontal scrollbar is used to move the message to the left or the right, and the vertical scrollbar to move it up and down.

![An object shows the Example for Using Scroll Bars. For long description in Notes pane, press F6.](Picture7.jpg)
ScrollBarDemo

### Notes:
The panel displayed the message and it is controlled by horizontal and vertical scrollbars. The horizontal bar is used to move the message to the left or right side. The vertical bar is used to move the message to move it up and down.

ScrollBarDemo: https://liveexample.pearsoncmg.com/html/ScrollBarDemo.html

<!-- Slide number: 25 -->
# Slider
Slider is similar to ScrollBar, but Slider has more properties and can appear in many forms.

![The computer code shows the Slider. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 3 rows. Row 1, java fx period scene period control period Slider. Row 2, has 9 lines.
Line 1, minus block Increment colon Double Property. The amount to adjust the slider if the track of the bar is clicked (default colon 10).
Line 2, minus max colon Double Property. The maximum value represented by this slider (default colon 100).
Line 3, minus min colon Double Property. The minimum value represented by this slider (default colon 0).
Line 4, minus value colon Double Property. Current value of the slider (default colon 0).
Line 5, minus orientation colon Object Property less than Orientation greater than. Specifies the orientation of the slider (default colon HORIZONTAL).
Line 6, minus major Tick Unit colon Double Property. The unit distance between major tick marks.
Line 7, minus minor Tick Count colon Integer Property. The number of minor ticks to place between two major ticks.
Line 8, minus show Tick Labels colon Boolean Property. Specifies whether the labels for tick marks are shown.
Line 9, minus show Tick Marks colon Boolean Property. Specifies whether the tick marks are shown.
Row 3, has 3 lines.
Line 1, plus Slider open parenthesis close parenthesis. Creates a default horizontal slider.
Line 2, plus Slider open parenthesis min colon double comma max colon double comma.
Line 3, value colon double close parenthesis. Creates a slider with the specified min., max., and value.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 26 -->
# Example: Using Sliders
Rewrite the preceding program using the sliders to control a message displayed on a panel instead of using scroll bars.

![An object shows the Example for Using Sliders. Rewrite the preceding program using the sliders to control a message displayed on a panel instead of using scroll bars.](Picture7.jpg)

<!-- Slide number: 27 -->
# Case Study: Bounce Ball
Listing 15.17 gives a program that displays a bouncing ball. You can add a slider to control the speed of the ball movement.

![A screenshot of Bounce Ball Control window shows a circle near top left corner. The circle is shaded in green. A slider is at the bottom with control button near left end.](Picture8.jpg)

![A screenshot of Bounce Ball Control window shows a circle near top middle. The circle is shaded in green. A slider is at the bottom with control button slightly shifted to the right.](Picture11.jpg)
SliderDemo

### Notes:
SliderDemo: https://liveexample.pearsoncmg.com/html/SliderDemo.html

<!-- Slide number: 28 -->
# Case Study: TicTacToe (1 of 2)

![An object shows the Case Study for Tic Tac Toe. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has two boxes. Box 1, shows the X won, and then the game is over. Box 2, shows the match is drawn then the game is over. The computer code shows the Case Study for Tic Tac Toe. It has two boxes. Box 1, java fx period scene period layout period Pane. Box 2, has 3 rows. Row 1, shows the Cell. Row 2, minus token colon char. Row 3, has 3 lines. Line 1, plus get Token open parenthesis close parenthesis colon char. Line 2, plus set Token open parenthesis token colon char close parenthesis colon void. Line 3, minus handle Mouse Click open parenthesis close parenthesis colon void.

<!-- Slide number: 29 -->
# Case Study: TicTacToe (2 of 2)

![The computer code shows the Case Study for Tic Tac Toe, cont. For long description in Notes pane, press F6.](Picture7.jpg)
TicTacToe

### Notes:
It has 3 boxes and they are interrelated with each other. Box 1, shows the Cell. Box 2, java fx period application period Application. Box 3, has 3 rows. Row 1, shows the Tic Tac Toe. Row 2, has 3 lines.
Line 1, minus whose Turn colon char. Indicates which player has the turn, initially X.
Line 2, minus cell colon Cell open braces close braces open braces close braces. A 3 by 3, two-dimensional array for cells.
Line 3, LbL Status colon Label. A label to display game status.
Row 3, has 3 lines.
Line 1, plus Tic Tac Toe open parenthesis close parenthesis. Constructs the TicTacToe user interface.
Line 2, plus is Full open parenthesis close parenthesis colon boolean. Returns true if all cells are filled.
Line 3, plus is Won open parenthesis token colon char close parenthesis colon boolean. Returns true if a player with the specified token has won.

TicTacToe: https://liveexample.pearsoncmg.com/html/TicTacToe.html

<!-- Slide number: 30 -->
# Media
You can use the Media class to obtain the source of the media, the MediaPlayer class to play and control the media, and the MediaView class to display the video.

![The computer code shows the Media. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 1 box. Box 1, has 3 rows. Row 1, java fx period scene period media period Media. Row 2, has 4 lines.
Line 1, minus duration colon Read Only Object Property. The durations in seconds of the source media.
Line 2, less than Duration greater than. The width in pixels of the source video.
Line 3, minus width colon Read Only Integer Property. The height in pixels of the source video.
Line 4, minus height colon Read Only Integer Property. Creates a Media from a URL source.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, plus Media open parenthesis source colon String close parenthesis.

<!-- Slide number: 31 -->
# MediaPlayer
The MediaPlayer class playes and controls the media with properties such as autoPlay, currentCount, cycleCount, mute, volume, and totalDuration.

![The computer code shows the Media Player. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 1 box. Box 1, has 3 rows. Row 1, java fx period scene period media period Media Player. Row 2, has 7 lines.
Line 1, minus auto Play colon Boolean Property. Specifies whether the playing should start automatically.
Line 2, minus current Count colon Read Only Integer Property. The number of completed playback cycles.
Line 3, minus cycle Count colon Integer Property. Specifies the number of time the media will be played.
Line 4, minus mute colon Boolean Property. Specifies whether the audio is muted.
Line 5, minus volume colon Double Property. The volume for the audio.
Line 6, minus total Duration colon.
Line 7, Read Only Object Property less than Duration greater than. The amount of time to play the media from start to finish.
Row 3, has 4 lines.
Line 1, plus Media Player open parenthesis media colon Media close parenthesis. Creates a player for a specified media.
Line 2, plus Play open parenthesis close parenthesis colon void. Plays the media.
Line 3, plus pause open parenthesis close parenthesis colon void. Pauses the media.
Line 4, plus seek open parenthesis close parenthesis colon void. Seeks the player to a new playback time.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 32 -->
# MediaView
The MediaView class is a subclass of Node that provides a view of the Media being played by a MediaPlayer. The MediaView class provides the properties for viewing the media.

![The computer code shows the Media View. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 1 box. Box 1, has 3 rows. Row 1, java fx period scene period media period Media View. Row 2, has 6 lines.
Line 1, minus x colon Double Property. Specifies the current x coordinate of the media view.
Line 2, minus y colon Double Property. Specifies the current y coordinate of the media view.
Line 3, minus media Player colon.
Line 4, Object Property less than Media Player greater than. Specifies a media player for the media view.
Line 5, minus fit Width colon Double Property. Specifies the width of the view for the media to fit.
Line 6, minus fit Height colon Double Property. Specifies the height of the view for the media to fit.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.
Row 3, has 2 lines.
Line 1, plus Media View open parenthesis close parenthesis. Creates an empty media view.
Line 2, plus Media View open parenthesis media Player colon Media Player close parenthesis. Creates a media view with the specified media player.

<!-- Slide number: 33 -->
# Example: Using Media
This example displays a video in a view. You can use the play/pause button to play or pause the video and use the rewind button to restart the video, and use the slider to control the volume of the audio.

![An object shows the Example for Using Media. It displays a video, the buttons to play or pause, the rewind button to restart and slider to control the volume.](Picture8.jpg)

![The computer code shows the Media. It has 3 boxes and they are interrelated with each other. Box 1, media colon Media. Box 2, media Player colon Media Player. Box 3, media View colon Media View.](Picture11.jpg)
MediaDemo

### Notes:
MediaDemo: https://liveexample.pearsoncmg.com/html/MediaDemo.html

<!-- Slide number: 34 -->
# Case Study: National Flags and Anthems
This case study presents a program that displays a nation’s flag and plays its anthem.

![Panel 3, shows the National Flag of US and on the downward side there is a play and pause button, one bar for Select a nation, one drop-down arrow box which shows the US.](Picture18.jpg)

![Panel 2, shows the National Flag of UK and on the downward side there is a play and pause button, one bar for Select a nation, one drop-down arrow box which shows the UK.](Picture13.jpg)

![An object shows the Case Study for National Flags and Anthems. For long description in Notes pane, press F6.](Picture11.jpg)
FlagAnthem

### Notes:
It has 3 panels. Panel 1, shows the National Flag of Denmark and on the downward side there is a play and pause button, one bar for Select a nation, one drop-down arrow box which shows the Denmark.

FlagAnthem: https://liveexample.pearsoncmg.com/html/FlagAnthem.html

<!-- Slide number: 35 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes: