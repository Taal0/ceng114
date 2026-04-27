<!-- Slide number: 1 -->
# Introduction to Java Programming and Data Structures
Thirteenth Edition

![Front Cover: Introduction to Java Programming and Data Structures Thirteenth Edition by Liang.](Picture3.jpg)
Chapter 15
Event-Driven Programming and Animations
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
Suppose you want to write a G U I program that lets the user enter a loan amount, annual interest rate, and number of years and click the Compute Payment button to obtain the monthly payment and total payment. How do you accomplish the task? You have to use event-driven programming to write the code to respond to the button-clicking event.

![An object shows the program of the Loan Calculator. For long description in Notes pane, press F6.](Picture6.jpg)
LoanCalculator

### Notes:
It has 5 lines. Line 1 shows the Annual Interest Rate colon and in the Box shows the 4.5. Line 2 shows the Number of Year's Colons, and the Box shows the 4. Line 3, shows the Laon Amount colon and in the Box shows the 5000. Line 4, shows the Monthly Payment colon and in the Box shows $114.02. Line 5, shows the Total Payment colon and in the Box shows the$5472.84. In the end, the Calculator shows.

LoanCalculator: https://liveexample.pearsoncmg.com/html/LoanCalculator.html

<!-- Slide number: 3 -->
# Objectives (1 of 2)
15.1 To get a taste of event-driven programming (§15.1).
15.2 To describe events, event sources, and event classes (§15.2).
15.3 To define handler classes, register handler objects with the source object, and write the code to handle events (§15.3).
15.4 To define handler classes using inner classes (§15.4).
15.5 To define handler classes using anonymous inner classes (§15.5).
15.6 To simplify event handling using lambda expressions (§15.6).

<!-- Slide number: 4 -->
# Objectives (2 of 2)
15.7 To develop a G U I application for a loan calculator (§15.7).
15.8 To write programs to deal with MouseEvents (§15.8).
15.9 To write programs to deal with KeyEvents (§15.9).
15.10 To create listeners for processing a value change in an observable object (§15.10).
15.11 To use the Animation, PathTransition, FadeTransition, and Timeline classes to develop animations (§15.11).
15.12 To develop an animation for simulating a bouncing ball (§15.12).
15.13 To draw, color, and resize a U S map (§15.13).

<!-- Slide number: 5 -->
# Procedural versus Event-Driven Programming
Procedural programming is executed in procedural order.
In event-driven programming, code is executed upon activation of events.

<!-- Slide number: 6 -->
# Taste of Event-Driven Programming
The example displays a button in the frame. A message is displayed on the console when a button is clicked.

![An object shows the Taste of Event-Driven Programming. It has 2 boxes. Box 1, shows the Command Prompt-java Handle Event. For long description in Notes pane, press F6.](Picture8.jpg)

![Box shows the Handle Event. It shows the 2 small boxes that are for OK and Cancel.](Picture11.jpg)
HandleEvent

### Notes:
It has 4 lines. Line 1, C colon slash backward book greater than java Handle Event. Line 2, OK Button clicked. Line 3, Cancel button clicked. Line 4, OK Button clicked.

HandleEvent: https://liveexample.pearsoncmg.com/html/HandleEvent.html

<!-- Slide number: 7 -->
# Handling G U I Events
Source object (e.g., button)
Listener object contains a method for processing the event.

![An object shows the Handling GUI Events. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
It has 3 boxes. Box 1 shows the Button, which indicates the Event source object, and it represents Box 2. Box 2 shows the Event, which indicates the Event object, and it represents Box 3. Box 3 shows the handler, which indicates the Event Handler Object.

<!-- Slide number: 8 -->
# Trace Execution (1 of 3)

![The computer code shows Trace Execution. For long description in Notes pane, press F6.](Picture5.jpg)

### Notes:
The computer code contains 18 lines. Line 1, public class HandleEvent extends Application open braces. Line 2, public void start open parenthesis Stage primaryStage close parenthesis open braces Line 3, This line shows the Start from the main method to create a window and display it Line 4, period period period Line 5, OKHandlerClass handler1 equals to new OKHandlerClass open parenthesis close parenthesis semicolon Line 6, btOK period set on action open parenthesis handler1 close parenthesis semicolon Line 7, cancelHandler Class handler2 equals to new CancelHandlerClass open parenthesis close parenthesis semicolon. Line 8, btCancel period setOnAction open parenthesis handler2 close parenthesis semicolon Line 9, period period period Line 10, primaryStage period show open parenthesis close parenthesis semi colon forward slash forward slash Display the stage Line 11, close braces Line 12, close braces Line 13, class OKHandlerClass implements EventHandler less than ActionEvent greater than open braces Line 14, @Override Line 15, public void handle open parenthesis ActionEvent e close parenthesis open braces Line 16, System period out period println open parenthesis, OK Button clicked, close parenthesis semi colon Line 17, close braces Line 18, close braces.

<!-- Slide number: 9 -->
# Trace Execution (2 of 3)

![The computer code shows Trace Execution. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
The computer code contains 17 lines. Line 1, public class HandleEvent extends Application open braces Line 2, public void start open parenthesis Stage primaryStage close parenthesis open braces Line 3, Period period period Line 4, OKHandlerClass handler1 equals to new OKHandlerClass open parenthesis close parenthesis semicolon Line 5, btOK period set on action open parenthesis handler1 close parenthesis semicolon Line 6, CancelHandlerClass handler2 equals to new ancelHandlerClass open parenthesis close parenthesis semicolon Line 7, btCancel period set on action open parenthesis handler2 close parenthesis semicolon Line 8, period period period Line 9, primaryStage period show open parenthesis close parenthesis semicolon forward slash forward slash Display the stage Line 10, close braces Line 11, close braces Line 12, class OKHandlerClass implements EventHandler less than ActionEvent greater than open braces Line 13, at symbol Override Line 14, public void handle open parenthesis ActionEvent e close parenthesis open braces Line 15, System period out period println open parenthesis, OK Button clicked, close parenthesis semicolon Line 16, close braces Line 17, close braces.

<!-- Slide number: 10 -->
# Trace Execution (3 of 3)

![The computer code shows Trace Execution. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
The computer code contains 17 lines. Line 1, public class HandleEvent extends Application open braces Line 2, public void start open parenthesis Stage primaryStage close parenthesis open braces Line 3, Period period period Line 4, OKHandlerClass handler1 equals to new OKHandlerClass open parenthesis close parenthesis semicolon Line 5, btOK period set on action open parenthesis handler1 close parenthesis semi colon Line 6, CancelHandlerClass handler2 equals to new CancelHandlerClass open parenthesis close parenthesis semicolon Line 7, btCancel period set on action open parenthesis handler2 close parenthesis semicolon Line 8, period period period Line 9, primaryStage period show open parenthesis close parenthesis semicolon forward slash forward slash Display the stage Line 10, close braces Line 11, close braces Line 12, class OKHandlerClass implements EventHandler less than ActionEvent greater than open braces Line 13, at symbol Override Line 14, public void handle open parenthesis ActionEvent e close parenthesis open braces This line shows The JVM invokes the listener's handle method Line 15, System period out period println open parenthesis "OK button clicked" close parenthesis semicolon Line 16, close braces Line 17, close braces.

<!-- Slide number: 11 -->
# Events
An event can be defined as a type of signal to the program that something has happened.
The event is generated by external user actions such as mouse movements, mouse clicks, or keystrokes.

<!-- Slide number: 12 -->
# Event Classes

![An object shows the Event Classes. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has 2 boxes, and they are divided into many parts. Box 1, shows the Event Object. Box 2 shows the Event, which represents the Box 1 and it's divided into 3 parts: Action Event, Input Event, Window Event. But, the Input Event is also divided into 2 parts are Mouse Event and Key Event.

<!-- Slide number: 13 -->
# Event Information
An event object contains whatever properties are pertinent to the event. You can identify the source object of the event using the getSource() instance method in the EventObject class. The subclasses of EventObject deal with special types of events, such as button actions, window events, mouse movements, and keystrokes. Table 15.1 lists external user actions, source objects, and event types generated.

<!-- Slide number: 14 -->
# Selected User Actions and Handlers
| User Action | Source Object | Event Type Fired | Event Registration Method |
| --- | --- | --- | --- |
| Click a button | Button | ActionEvent | setOnAction(EventHandler<ActionEvent>) |
| Press Enter in a text field | TextField | ActionEvent | setOnAction(EventHandler<ActionEvent>) |
| Check or uncheck | RadioButton | ActionEvent | setOnAction(EventHandler<ActionEvent>) |
| Check or uncheck | CheckBox | ActionEvent | setOnAction(EventHandler<ActionEvent>) |
| Select a new; item | ComboBox | ActionEvent | setOnAction(EventHandler<ActionEvent>) |
| Mouse pressed | Node, Scene | MouseEvent | setOnMousePressed(EventHandler<MouseEvent>) |
| Mouse released | Blank | Blank | setOnMouseReleased(EventHandler<MouseEvent>) |
| Mouse clicked | Blank | Blank | setOnMouseClicked(EventHandler<MouseEvent>) |
| Mouse entered | Blank | Blank | setOnMouseEntered(EventHandler<MouseEvent>) |
| Mouse exited | Blank | Blank | setOnMouseExited(EventHandler<MouseEvent>) |
| Mouse moved | Blank | Blank | setOnMouseMoved(EventHandler<MouseEvent>) |
| Mouse dragged | Blank | Blank | setOnMouseDragged(EventHandler<MouseEvent>) |
| Key pressed: | Node, Scene | KeyEvent | setOnKeyPressed(EventHandler<KeyEvent>) |
| Key released | Blank | Blank | setOnKeyReleased(EventHandler<KeyEvent>) |
| Key typed | Blank | Blank | setOnKeyTyped(EventHandler<KeyEvent>) |

<!-- Slide number: 15 -->
# The Delegation Model

![An object shows the Delegation Model. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
It has one Circle and 6 boxes. A Circle shows the User Action, which represents Box 1, and it indicates Trigger an event. Box 1, has 2 rows. Row 1, source colon Source Class. Row 2, plus set on X Event Type open parenthesis listener close parenthesis, indicates the (2) Register by invoking, source colon set on X Event Type open parenthesis listener close parenthesis colon, and representing the Box 3. Box 2, has 2 rows. Row 1, has 2 lines. Line 1, open braces open braces interface close braces close braces. Line 2, Event Handler less than T extends Event greater than. Row 2, plus handle open parenthesis event colon T close parenthesis. Box 3, listener colon Listener Class indicating Alyssa object is an instance of a listener interface, representing the Box 2 and Box 1. Box 4, has 2 rows. Row 1, has 2 lines. Line 1, open braces open braces interface close braces close braces. Line 2, Event Handler less than Action Event greater than. Row 2, plus handle open parenthesis event colon Action Event close parenthesis. Box 5, listener colon Custom Listener Class b indicates an action event listener is an instance of Event Handler less than Action Event greater than and it represents the Box 4 and Box 6. Box 6, which shows the a, A genetic source object with a generic event T and (b) A button source object with an Action Event. It has 2 rows. Row 1, source colon java fx period scene period control period Button. Row 2, plus set On Action open parenthesis listener close parenthesis. This Box also shows the (2) for Register by invoking the source period set On Action open parenthesis listener close parenthesis semicolon.

<!-- Slide number: 16 -->
# The Delegation Model: Example
Button btOK = new Button("OK");
OKHandlerClass handler = new OKHandlerClass();
btOK.setOnAction(handler);

<!-- Slide number: 17 -->
# Example: First Version for Controlcircle (No Listeners)
Now let us consider to write a program that uses two buttons to control the size of a circle.

![An object shows the Example of First Version for Control Circle (no listeners). The program shows the shape of the Circle and shows the 2 buttons for Enlarge and Shrink.](Picture6.jpg)
ControlCircleWithoutEventHandling

### Notes:
ControlCircleWithoutEventHandling: https://liveexample.pearsoncmg.com/html/ControlCircleWithoutEventHandling.html

<!-- Slide number: 18 -->
# Example: Second Version for Controlcircle (With Listener for Enlarge)
Now let us consider to write a program that uses two buttons to control the size of a circle.

![The first program shows the circle size is Shrink, and the second program shows the circle size is Enlarge.](Picture11.jpg)

![An object shows the Example of the Second Version for Control Circle (with the listener for Enlarge).](Picture8.jpg)
ControlCircle

### Notes:
ControlCircle: https://liveexample.pearsoncmg.com/html/ControlCircle.html

<!-- Slide number: 19 -->
# Inner Class Listeners
A listener class is designed specifically to create a listener object for a G U I component (e.g., a button). It will not be shared by other applications. So, it is appropriate to define the listener class inside the frame class as an inner class.

<!-- Slide number: 20 -->
# Inner Classes (1 of 4)
Inner class: A class is a member of another class.
Advantages: In some applications, you can use an inner class to make programs simple.
An inner class can reference the data and methods defined in the outer class in which it nests, so you do not need to pass the reference of the outer class to the constructor of the inner class.
ShowInnerClass

### Notes:
ShowInnerClass: https://liveexample.pearsoncmg.com/html/ShowInnerClass.html

<!-- Slide number: 21 -->
# Inner Classes (2 of 4)

![The computer code shows Inner Classes comma cont period The computer code is divided into 3 parts period Part A contains 6 lines. For long description in Notes pane, press F6.](Picture6.jpg)

### Notes:
Line 1, public class test open parenthesis. Line 2, period period period. Line 3, close parenthesis. Line 4, public class A open parenthesis. Line 5 period period period. Line 6, close parenthesis. Part B contains 7 lines. Line 1, public class test open parenthesis. Line 2, period period period. Line 3, slash forward forward slash. Line 4, public class A open parenthesis. Line 5, period period period. Line 6, close parenthesis. Line 7, close parenthesis. Part C contains 18 lines. Line 1, forward slash forward slash outer class period java colon inner class demo. Line 2, public class outer class open parenthesis. Line 3, private int data semicolon. Line 4, forward slash address address a method in the outer class address forward slash. Line 5, public void m open parenthesis close parenthesis open braces. Line 6, slash forward slash do something. Line 7, close parenthesis. Line 8, forward slash forward slash an inner class. Line 9, class inner class open braces. Line 10, forward slash address address a method in the inner class address forward slash. Line 11, public void mi open parenthesis close parenthesis open braces. Line 12, forward slash forward slash directly reference data and method. Line 13, forward slash forward slash defined in its outer class. Line 14, data plus plus semicolon. Line 15, m open parenthesis close parenthesis. Semicolon. Line 16, close parenthesis. Line 17, close parenthesis. Line 18, close parenthesis.

<!-- Slide number: 22 -->
# Inner Classes (3 of 4)
Inner classes can make programs simple and concise.
An inner class supports the work of its containing outer class and is compiled into a class named OuterClassName$InnerClassName.class. For example, the inner class InnerClass in OuterClass is compiled into OuterClass$InnerClass.class.

<!-- Slide number: 23 -->
# Inner Classes (4 of 4)
An inner class can be declared public, protected, or private subject to the same visibility rules applied to a member of the class.
An inner class can be declared static. A static inner class can be accessed using the outer class name. A static inner class cannot access nonstatic members of the outer class

<!-- Slide number: 24 -->
# Anonymous Inner Classes (1 of 3)
An anonymous inner class must always extend a superclass or implement an interface, but it cannot have an explicit extends or implements clause.
An anonymous inner class must implement all the abstract methods in the superclass or in the interface.
An anonymous inner class always uses the no-arg constructor from its superclass to create an instance. If an anonymous inner class implements an interface, the constructor is Object().
An anonymous inner class is compiled into a class named OuterClassName$n.class. For example, if the outer class Test has two anonymous inner classes, these two classes are compiled into Test$1.class and Test$2.class.

<!-- Slide number: 25 -->
# Anonymous Inner Classes (2 of 3)
Inner class listeners can be shortened using anonymous inner classes. An anonymous inner class is an inner class without a name. It combines declaring an inner class and creating an instance of the class in one step. An anonymous inner class is declared as follows:
new SuperClassName/InterfaceName() {
// Implement or override methods in superclass or interface
// Other methods if necessary
}

<!-- Slide number: 26 -->
# Anonymous Inner Classes (3 of 3)

![The computer code shows the Anonymous Inner Classes open parenthesis cont dot close parenthesis. For long description in Notes pane, press F6.](Picture7.jpg)

![A screenshot of Anonymous Handler Demo window shows buttons for New, Open, Save, and Print.](Picture10.jpg)
AnonymousHandlerDemo

### Notes:
The computer code is divided into 2 parts. Part A shows the inner class enlarge listener. The computer code contains 11 lines. Line 1, public void start open parenthesis stage primary stage close parenthesis open braces. Line 2, omitted. Line 3, btEnlargeHandler.setonaction open parenthesis. Line 4, new EnLarger handler open parenthesis close parenthesis close parenthesis semi colon. Line 5, close parenthesis. Line 6, class EnlargerHandler. Line 7, implements EventHandler Less than action event greater than open braces. Line 8, public void handle open parenthesis ActionEvent e close parenthesis open braces. Line 9, circle pane period enlarge open parenthesis close parenthesis semicolon. Line 10, close braces. Line 11, close braces. Part B shows Anonymous inner class. The computer code contains 10 lines. Line 1, public void start open parenthesis stage primary stage close parenthesis open braces. Line 2, omitted. Line 3, btEnlargeHandler.setonaction open parenthesis. Line 4, new class enlarge handler is being cutted. Line 5, implements EventHandler Less than action event greater than open braces. Line 6, public void handle open parenthesis ActionEvent e close parenthesis open braces. Line 7, circle pane period enlarge open parenthesis close parenthesis semicolon. Line 8, close braces. Line 9, close braces close braces semi colon. Line 10, close braces.

AnonymousHandlerDemo: https://liveexample.pearsoncmg.com/html/AnonymousHandlerDemo.html

<!-- Slide number: 27 -->
# Simplifying Event Handing Using Lambda Expressions
Lambda expression is a new feature in Java 8. Lambda expressions can be viewed as an anonymous method with a concise syntax. For example, the following code in (a) can be greatly simplified using a lambda expression in (b) in three lines.

![The computer code shows Simplifying Event Handing Using Lambda Expressions period. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The computer code is divided into two parts period Part A shows Anonymous inner class event handler. This code contains 8 lines. Line 1, btEnlarge period set on action open parenthesis. Line 2, new EventHandler less than ActionEvent greater than open parenthesis close parenthesis open braces. Line 3, at symbolOverride Line 4, public void handle open parenthesis ActionEvent e close parenthesis open braces Line 5, forward slash forward slash Code for processing event e Line 6, close braces Line 7, close braces Line 8, close braces close parenthesis semi colon. Part B shows Lambda expression event handler The computer code contains 3 lines. Line1, btEnlarge period set on action open parenthesis e - greater than open braces Line 2, forward slash forward slash Code for processing event e. Line 3, close braces close parenthesis semicolon.

<!-- Slide number: 28 -->
# Basic Syntax for a Lambda Expression
The basic syntax for a lambda expression is either
(type1 param1, type2 param2, ...) -> expression
or
(type1 param1, type2 param2, ...) -> { statements; }
The data type for a parameter may be explicitly declared or implicitly inferred by the compiler. The parentheses can be omitted if there is only one parameter without an explicit data type.

<!-- Slide number: 29 -->
# Single Abstract Method Interface (S A M)
The statements in the lambda expression is all for that method. If it contains multiple methods, the compiler will not be able to compile the lambda expression. So, for the compiler to understand lambda expressions, the interface must contain exactly one abstract method. Such an interface is known as a functional interface, or a Single Abstract Method (S A M) interface.
AnonymousHandlerDemo

### Notes:
AnonymousHandlerDemo: https://liveexample.pearsoncmg.com/html/AnonymousHandlerDemo.html

<!-- Slide number: 30 -->
# Problem: Loan Calculator
LoanCalculator

### Notes:
LoanCalculator: https://liveexample.pearsoncmg.com/html/LoanCalculator.html

<!-- Slide number: 31 -->
# The MouseEvent Class

![The computer code shows the mouse event class. For long description in Notes pane, press F6.](Picture6.jpg)
MouseEventDemo

### Notes:
The computer code contains 12 lines
Line 1, plus get button open parenthesis close parenthesis colon MouseButton. Indicates which mouse button has been clicked.
Line 2, plus getClickcount open parenthesis close parenthesis colon int. Returns the number of mouse clicks associated with this event.
Line 3, getX open parenthesis close parenthesis colon double. Returns the x coordinate of the mouse point in the event source node.
Line 4, plus getY open parenthesis close parenthesis colon double. Returns the y coordinate of the mouse point in the event source node.
Line 5, plus getSceneX open parenthesis close parenthesis colon double. Returns the x coordinate of the mouse point in the scene.
Line 6, plus getSceneY open parenthesis close parenthesis colon double. Returns the y coordinate of the mouse point in the scene.
Line 7, getScreenX open parenthesis close parenthesis colon double. Returns the x coordinate of the mouse point in the screen.
Line 8, getScreenY open parenthesis close parenthesis colon double. Returns the y coordinate of the mouse point in the screen.
Line 9, plus isAltDown open parenthesis close parenthesis colon Boolean. Returns true if the Al t key is pressed on this event.
Line 10, plus isControlDown open parenthesis close parenthesis colon Boolean. Returns true if the Control key is pressed on this event.
Line 11, isMetaDown open parenthesis close parenthesis colon Boolean. Returns true if the mouse Meta button is pressed on this event.
Line 12, plus isShiftDown open parenthesis close parenthesis colon Boolean. Returns true if the Shift key is pressed on this event.

MouseEventDemo: https://liveexample.pearsoncmg.com/html/MouseEventDemo.html

<!-- Slide number: 32 -->
# The KeyEvent Class

![The computer code shows The Keyevent Class. For long description in Notes pane, press F6.](Picture6.jpg)
KeyEventDemo

### Notes:
The computer code contains 7 lines.
Line 1, plus get Character open parenthesis close parenthesis colon String. Returns the character associated with the key in this event.
Line 2, plus getCode open parenthesis close parenthesis colon keyCode. Returns the key code associated with the key in this event.
Line 3, plus get text open parenthesis close parenthesis colon String. Returns a string describing the key code.
Line 4, plus isAltDown open parenthesis close parenthesis colon Boolean. Returns true if the Alt key is pressed on this event.
Line 5, plus isControlDown open parenthesis close parenthesis colon Boolean. Returns true if the Control key is pressed on this event.
Line 6, isMetaDown open parenthesis close parenthesis colon Boolean. Returns true if the mouse Meta button is pressed on this event.
Line 7, plus isShiftDown open parenthesis close parenthesis colon Boolean. Returns true if the Shift key is pressed on this event.

KeyEventDemo: https://liveexample.pearsoncmg.com/html/KeyEventDemo.html

<!-- Slide number: 33 -->
# The KeyCode Constants
| Constant | Description |
| --- | --- |
| Home | The Home key |
| End | The End key |
| Page\_up | The Page Up key |
| Pace\_down | The Page Down key |
| Up | The up-arrow key |
| Down | The down-arrow key |
| Left | The left-arrow key |
| Right | The right-arrow key |
| Escape | The Esc key |
| Tab | The Tab key |
| Constant | Description |
| --- | --- |
| Control | The Control key |
| Shift | The Shift key |
| Back\_space | The Backspace key |
| Caps | The Caps Lock key |
| Num\_lock | The Num Lock key |
| Enter | The Enter key |
| Undefined | The keyCode unknown |
| F1 to F12 | The function keys from F1 to F12 |
| 0 to 9 | The number keys from 0 to 9 |
| A to Z | The letter keys from A to Z |

<!-- Slide number: 34 -->
# Example: Control Circle With Mouse and Key
ControlCircleWithMouseAndKey

### Notes:
ControlCircleWithMouseAndKey: https://liveexample.pearsoncmg.com/html/ControlCircleWithMouseAndKey.html

<!-- Slide number: 35 -->
# Listeners for Observable Objects
You can add a listener to process a value change in an observable object.
An instance of Observable is known as an observable object, which contains the addListener(InvalidationListener listener) method for adding a listener. Once the value is changed in the property, a listener is notified. The listener class should implement the InvalidationListener interface, which uses the invalidated(Observable o) method to handle the property value change. Every binding property is an instance of Observable.
ObservablePropertyDemo
DisplayResizableClock

### Notes:
ObservablePropertyDemo: https://liveexample.pearsoncmg.com/html/ObservablePropertyDemo.html
DisplayResizableClock: https://liveexample.pearsoncmg.com/html/DisplayResizableClock.html

<!-- Slide number: 36 -->
# Animation
JavaF X provides the Animation class with the core functionality for all animations.

![The computer code shows Animation. For long description in Notes pane, press F6.](Picture7.jpg)

### Notes:
The computer code contains 8 lines.
Line 1, minus autoReverse colon BooleanProperty. Defines whether the animation reverses direction on alternating cycles.
Line 2, minus cycleCount colon IntegerProperty. Defines the number of cycles in this animation.
Line 3, minus rate colon DoubleProperty. Defines the speed and direction for this animation.
Line 4, minus status colon ReadOnlyObjectProperty. Read only property to indicate the status of the animation.
Line 5, less than Animation.Status greater than.
Line 6, plus pause open parenthesis close parenthesis colon void. Pauses the animation.
Line 7, plus play open parenthesis close parenthesis colon void. Plays the animation from the current position.
Line 8, plus stop open parenthesis close parenthesis colon void. Stops the animation and resets the animation.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

<!-- Slide number: 37 -->
# PathTransition

![The computer code shows PathTransition. For long description in Notes pane, press F6.](Picture6.jpg)
PathTransitionDemo
FlagRisingAnimation

### Notes:
The computer code contains 10 lines.
Line 1, minus duration colon ObjectProperty less than Duration greater than. The duration of this transition.
Line 2, minus node colon ObjectProperty less than Node greater than.
Line 3, minus orientation colon ObjectProperty. The target node of this transition.
Line 4, less than PathTransition.OrientationType greater than. The orientation of the node along the path.
Line 5, minus path colon Objecttypeless than Shape greater than. The shape whose outline is used as a path to animate the node move.
Line 6, plus Pathtransition open parenthesis close parenthesis. Creates an empty PathTransition.
Line 7, plus PathTransition open parenthesis duration colon Duration. Creates a PathTransition with the specified duration and path.
Line 8, Path colon Shape close parenthesis.
Line 9, plus PathTransition open parenthesis duration colon Duration. Creates a PathTransition with the specified duration and path.
Line 10, Path colon Shape, node colon Node close parenthesis. Creates a PathTransition with the specified duration, path, and node.
The first line is labeled, the getter and setter methods for property 1 values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

PathTransitionDemo: https://liveexample.pearsoncmg.com/html/PathTransitionDemo.html
FlagRisingAnimation: https://liveexample.pearsoncmg.com/html/FlagRisingAnimation.html

<!-- Slide number: 38 -->
# FadeTransition
The FadeTransition class animates the change of the opacity in a node over a given time.

![The computer code shows FadeTransition. For long description in Notes pane, press F6.](Picture6.jpg)
FadeTransitionDemo

### Notes:
The computer code contains 8 lines.
Line 1, minus duration colon ObjectProperty less than Duration greater than. The duration of this transition.
Line 2, minus node colon ObjectProperty less than Node greater than. The target node of this transition.
Line 3, minus fromValue colon DoubleProperty. The start opacity for this animation.
Line 4, minus toValue colon DoubleProperty. The stop opacity for this animation.
Line 5, minus byValue colon DoubleProperty. The incremental value on the opacity for this animation.
Line 6, plus FadeTransition open parenthesis close parenthesis. Creates an empty FadeTransition.
Line 7, plus FadeTransition open parenthesis duration colon Duration close parenthesis. Creates a FadeTransition with the specified duration.
Line 8, plus FadeTransition open parenthesis duration colon Duration, node colon Node close parenthesis. Creates a FadeTransition with the specified duration and node.
The first line is labeled, the getter and setter methods for property values and a getter for property itself are provided in the class, but omitted in the UML diagram for brevity.

FadeTransitionDemo: https://liveexample.pearsoncmg.com/html/FadeTransitionDemo.html

<!-- Slide number: 39 -->
# Timeline
PathTransition and FadeTransition define specialized animations. The Timeline class can be used to program any animation using one or more KeyFrames. Each KeyFrame is executed sequentially at a specified time interval. Timeline inherits from Animation.
TimelineDemo
Run

### Notes:
TimelineDemo: https://liveexample.pearsoncmg.com/html/TimelineDemo.html

<!-- Slide number: 40 -->
# Clock Animation

![An object shows the Clock Animation. The program shows the clock and in the clock represents the hour hand, minute hand and second hand.](Picture6.jpg)
ClockAnimation

### Notes:
ClockAnimation: https://liveexample.pearsoncmg.com/html/ClockAnimation.html

<!-- Slide number: 41 -->
# Case Study: Bouncing Ball

![A screenshot of Bounce Ball Control window shows a circle in the middle. The circle is shaded in green.](Picture19.jpg)

![A screenshot of Bounce Ball Control window shows a circle near bottom right corner. The circle is shaded in green.](Picture21.jpg)

![A screenshot of Bounce Ball Control window shows a circle near top left corner. The circle is shaded in green.](Picture13.jpg)

![A diagram shows BallPane leading to javafx.scene.layout.Pane and BounceBallControl leading to javafx.application.Application. For long description in Notes pane, press F6.](Picture23.jpg)
BallPane
BounceBallControl

### Notes:
A horizontal line connects BallPane to BounceBallControl with 1 near each end. There is a shaded rhombus attached to BounceBallControl on the left. The codes for BallPane are as follows.
Hyphen x colon double
Hyphen y colon double
Hyphen dx colon double
Hyphen dy colon double
Hyphen radius colon double
Hyphen circle colon Circle
Hyphen animation colon Timeline
+BallPane left parenthesis right parenthesis
+play left parenthesis right parenthesis colon void
+pause left parenthesis right parenthesis colon void
+increaseSpeed left parenthesis right parenthesis colon void
+decreaseSpeed left parenthesis right parenthesis colon void
+rateProperty left parenthesis right parenthesis colon DoubleProperty
+moveBall left parenthesis right parenthesis colon void.

BallPane: https://liveexample.pearsoncmg.com/html/BallPane.html
BounceBallControl: https://liveexample.pearsoncmg.com/html/BounceBallControl.html

<!-- Slide number: 42 -->
# Copyright
This work is protected by United States copyright laws and is provided solely for the use of instructors in teaching their courses and assessing student learning. Dissemination or sale of any part of this work (including on the World Wide Web) will destroy the integrity of the work and is not permitted. The work and materials from it should never be made available to students except by instructors using the accompanying text in their classes. All recipients of this work are expected to abide by these restrictions and to honor the intended pedagogical purposes and the needs of other instructors who rely on these materials.

![Warning](Graphic6.jpg)

### Notes: