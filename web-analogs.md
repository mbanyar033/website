---
layout: page
title: HTML/CSS Analogs in Flutter
permalink: /web-analogs/
---

<link rel="stylesheet" href="/css/two_column.css">

* TOC Placeholder
{:toc}

<div class="begin-examples"></div>
This page is for users who are familiar with the HTML and CSS syntax for
arranging components of an application's UI. It maps HTML/CSS code snippets to
their Flutter/Dart code equivalents.

The examples assume:
* The HTML document starts with a modern HTML DOCTYPE, and the CSS box model
for all HTML elements is set to [border-box](https://css-tricks.com/box-sizing/),
for consistency with the Flutter model.
   {% prettify css %}<!DOCTYPE html>

   {
     box-sizing: border-box;
   }
{% endprettify %}
* In Flutter, the default styling of the "Lorem ipsum" text is defined by the
`bold24Roboto` variable as follows, to keep the syntax simple:
  {% prettify dart %}
  TextStyle bold24Roboto = new TextStyle(
      color: Colors.white,
      fontSize: 24.0,
      fontWeight: FontWeight.w900,
  );
{% endprettify %}

## Performing Basic Layout Operations

The following examples show how to perform the most common UI layout tasks.

### Styling and Aligning Text
Font style, size, and other text attributes that CSS handles with the font and
color properties are individual properties of a [TextStyle](https://docs.flutter.io/flutter/painting/TextStyle-class.html)
child of a [Text](https://docs.flutter.io/flutter/widgets/Text-class.html) widget.

In both HTML and Flutter, by default child elements or widgets are anchored at
the top left.

<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
    Lorem ipsum
</div>

.greybox {
      background-color: #e0e0e0; /* grey 300 */
      width: 320px;
      height: 240px;
[[highlight]]      font: 900 24px Georgia;[[/highlight]]
    }
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
  var container = new Container( // grey box
    child: new Text(
      "Lorem ipsum",
      style: [[highlight]]new TextStyle(
        fontSize: 24.0
        fontWeight: FontWeight.w900,
        fontFamily: "Georgia",
      ),[[/highlight]]
    ),
    width: 320.0,
    height: 240.0,
    color: Colors.grey[300],
  );
{% endprettify %}
</div>

### Setting Background Color
In Flutter, you set background color in a [Container](https://docs.flutter.io/flutter/widgets/Container-class.html)’s
```decoration``` property.

The CSS examples use the hex color equivalents to the Material color palette.
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  Lorem ipsum
</div>

.greybox {
[[highlight]]      background-color: #e0e0e0; [[/highlight]] /* grey 300 */
      width: 320px;
      height: 240px;
      font: 900 24px Roboto;
    }
{% endprettify %}
</div>

<div class="righthighlight">
{% prettify dart %}
  var container = new Container( // grey box
    child: new Text(
      "Lorem ipsum",
      style: bold24Roboto,
    ),
    width: 320.0,
    height: 240.0,
[[highlight]]    color: Colors.grey[300],[[/highlight]]
  );
{% endprettify %}
</div>

### Centering Components
A [Center](https://docs.flutter.io/flutter/widgets/Center-class.html) widget
centers its child both horizontally and vertically.

To accomplish a similar effect in CSS, the parent element uses either a flex
or table-cell display behavior. The examples on this page show the flex
behavior.

<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  Lorem ipsum
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
[[highlight]]  display: flex;
  align-items: center;
  justify-content: center; [[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: [[highlight]] new Center(
    child: [[/highlight]] new Text(
      "Lorem ipsum",
      style: bold24Roboto,
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Setting Container Width

To specify the width of a [Container](https://docs.flutter.io/flutter/widgets/Container-class.html)
widget you set its ```width``` property. This is a fixed width, unlike the
CSS max-width property which adjusts container width up to a maximum value. To
mimic that effect in Flutter, use the ```constraints``` property of the Container.
Create a new [BoxConstraints](https://docs.flutter.io/flutter/rendering/BoxConstraints-class.html)
widget with a ```minWidth``` or ```maxWidth```.

For nested Containers, if the parent’s width is less than the child’s width,
the child Container sizes itself to match the parent.
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
[[highlight]]  width: 320px; [[/highlight]]
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]    width: 100%;
  max-width: 240px; [[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: new Text(
        "Lorem ipsum",
        style: bold24Roboto,
      ),
      decoration: new BoxDecoration(
        color: Colors.red[400],
      ),
      padding: new EdgeInsets.all(16.0),
[[highlight]]      width: 240.0, [[/highlight]]//max-width is 240.0
    ),
  ),
[[highlight]]  width: 320.0, [[/highlight]]
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

## Manipulating Position and Size
The following examples show how to perform more complex operations on widget
position, size, and background.

### Setting Absolute Position

By default, widgets are positioned relative to their parent.

To specify an absolute position for a widget as x-y coordinates, nest it in a
[Positioned](https://docs.flutter.io/flutter/widgets/Positioned-class.html)
widget that is in turn nested in a [Stack](https://docs.flutter.io/flutter/widgets/Stack-class.html)
widget.
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
[[highlight]]  position: relative; [[/highlight]]
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  position: absolute;
  top: 24px;
  left: 24px; [[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
[[highlight]]  child: new Stack(
    children: [
      new Positioned( // red box
        child: [[/highlight]] new Container(
          child: new Text(
            "Lorem ipsum",
            style: bold24Roboto,
          ),
          decoration: new BoxDecoration(
            color: Colors.red[400],
          ),
          padding: new EdgeInsets.all(16.0),
        ),
[[highlight]]        left: 24.0,
        top: 24.0,
      ),
    ],
  ), [[/highlight]]
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Rotating Components

To rotate a widget, nest it in a [Transform](https://docs.flutter.io/flutter/widgets/Transform-class.html)
widget. Use the Transform widget’s ```alignment``` and ```origin``` properties to
specify the transform origin (fulcrum) in relative and absolute terms, respectively.

For a simple 2D rotation, the widget is rotated on the Z axis using radians.
(degrees × π / 180)
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  transform: rotate(15deg); [[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
{% prettify dart %}
var container = new Container( // gray box
  child: new Center(
    child: [[highlight]] new Transform(
      child: [[/highlight]] new Container( // red box
        child: new Text(
          "Lorem ipsum",
          style: bold24Roboto,
          textAlign: TextAlign.center,
        ),
        decoration: new BoxDecoration(
          color: Colors.red[400],
        ),
        padding: new EdgeInsets.all(16.0),
      ),
[[highlight]]      alignment: FractionalOffset.center,
      transform: new Matrix4.identity()
        ..rotateZ(15 * 3.1415927 / 180),
    ), [[/highlight]]
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Scaling Components

To scale a widget up or down, nest it in a [Transform](https://docs.flutter.io/flutter/widgets/Transform-class.html)
widget. Use the Transform widget’s ```alignment``` and ```origin``` properties to specify
the transform origin (fulcrum) in relative and absolute terms, respectively.

For a simple scaling operation along the x-axis, create a new
[Matrix4](https://docs.flutter.io/flutter/vector_math_64/Matrix4-class.html)
identity object and use its scale() method to specify the scaling factor.

When you scale a parent widget, all its child widgets are scaled accordingly.
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  transform: scale(1.5); [[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
{% prettify dart %}
var container = new Container( // gray box
  child: new Center(
    child: [[highlight]] new Transform(
      child: [[/highlight]] new Container( // red box
        child: new Text(
          "Lorem ipsum",
          style: bold24Roboto,
          textAlign: TextAlign.center,
        ),
        decoration: new BoxDecoration(
          color: Colors.red[400],
        ),
        padding: new EdgeInsets.all(16.0),
      ),
[[highlight]]      alignment: FractionalOffset.center,
      transform: new Matrix4.identity()
        ..scale(1.5),
     ), [[/highlight]]
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Applying a Linear Gradient

To apply a linear gradient to a widget's background, nest it in a
[Container](https://docs.flutter.io/flutter/widgets/Container-class.html) widget.
Then use the Container widget’s ```decoration``` property to create a [BoxDecoration](https://docs.flutter.io/flutter/painting/BoxDecoration-class.html)
object, and use BoxDecoration's ```gradient``` property to transform the background
fill.

The gradient “angle” is based on the FractionalOffset (x, y) values:

* If the beginning and ending x values are equal, the gradient is vertical
(0° | 180°).
* If the beginning and ending y values are equal, the gradient is horizontal
(90° | 270°).

#### Vertical Gradient
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  padding: 16px;
  color: #ffffff;
[[highlight]]  background: linear-gradient(180deg, #ef5350, rgba(0, 0, 0, 0) 80%); [[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: new Text(
        "Lorem ipsum",
        style: bold24Roboto,
      ),
[[highlight]]      decoration: new BoxDecoration(
        gradient: new LinearGradient(
          begin: const FractionalOffset(0.5, 0.0),
          end: const FractionalOffset(0.5, 0.8),
          colors: <Color>[
            const Color(0xffef5350),
            const Color(0x00ef5350)
          ],
        ),
      ), [[/highlight]]
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

#### Horizontal Gradient
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  padding: 16px;
  color: #ffffff;
[[highlight]]  background: linear-gradient(90deg, #ef5350, rgba(0, 0, 0, 0) 80%); [[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: new Text(
        "Lorem ipsum",
        style: bold24Roboto,
      ),
[[highlight]]      decoration: new BoxDecoration(
        gradient: new LinearGradient(
          begin: const FractionalOffset(0.0, 0.5),
          end: const FractionalOffset(0.8, 0.5),
          colors: <Color>[
            const Color(0xffef5350),
            const Color(0x00ef5350)
          ],
        ),
      ), [[/highlight]]
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

## Manipulating Shapes
The following examples show how to make and customize shapes.

### Rounding Corners
To round the corners of a rectangular shape, use the ```borderRadius``` property of a
[BoxDecoration](https://docs.flutter.io/flutter/painting/BoxDecoration-class.html)
object. Create a new [BorderRadius](https://docs.flutter.io/flutter/painting/BorderRadius-class.html)
object that specifies the radii for rounding each corner.
<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* gray 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  border-radius: 8px; [[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red circle
      child: new Text(
        "Lorem ipsum",
        style: bold24Roboto,
      ),
      decoration: new BoxDecoration(
        color: Colors.red[400],
[[highlight]]        borderRadius: new BorderRadius.all(
          const Radius.circular(8.0),
        ), [[/highlight]]
      ),
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Adding Box Shadows
In CSS you can specify shadow offset and blur in shorthand, using the box-shadow
property. This example shows two box shadows, with properties:
*  ```xOffset: 0px, yOffset: 2px, blur: 4px, color: black @80% alpha```
*  ```xOffset: 0px, yOffset: 06x, blur: 20px, color: black @50% alpha```.

In Flutter, each property and value is specified separately. Use the ```boxShadow```
property of BoxDecoration to create a list of [BoxShadow](https://docs.flutter.io/flutter/painting/BoxShadow-class.html)
widgets. You can define one or multiple BoxShadow widgets, which can be stacked
to customize the shadow depth, color, etc.


<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.8),
              0 6px 20px rgba(0, 0, 0, 0.5);[[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: new Text(
        "Lorem ipsum",
        style: bold24Roboto,
      ),
      decoration: new BoxDecoration(
        color: Colors.red[400],
[[highlight]]        boxShadow: <BoxShadow>[
          new BoxShadow (
            color: const Color(0xcc000000),
            offset: new Offset(0.0, 2.0),
            blurRadius: 4.0,
          ),
          new BoxShadow (
            color: const Color(0x80000000),
            offset: new Offset(0.0, 6.0),
            blurRadius: 20.0,
          ),
        ], [[/highlight]]
      ),
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  decoration: new BoxDecoration(
    color: Colors.grey[300],
  ),
  margin: new EdgeInsets.only(bottom: 16.0),
);
{% endprettify %}
</div>

### Making Circles and Ellipses
Making a circle in CSS requires a workaround of applying a border-radius of
50% to all four sides of a rectangle.

While this approach is supported with the ```borderRadius``` property of
[BoxDecoration](https://docs.flutter.io/flutter/painting/BoxDecoration-class.html),
Flutter provides a ```shape``` property with [BoxShape enum](https://docs.flutter.io/flutter/painting/BoxShape-class.html)
for this purpose.

<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redcircle">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* gray 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redcircle {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  text-align: center;
  width: 160px;
  height: 160px;
  border-radius: 50%; [[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red circle
      child: new Text(
        "Lorem ipsum",
        style: bold24Roboto,
[[highlight]]        textAlign: TextAlign.center, [[/highlight]]
      ),
      decoration: new BoxDecoration(
        color: Colors.red[400],
[[highlight]]        shape: BoxShape.circle, [[/highlight]]
      ),
      padding: new EdgeInsets.all(16.0),
[[highlight]]      width: 160.0,
      height: 160.0, [[/highlight]]
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

## Manipulating Text
The following examples show how to specify fonts and other text attributes. They
also show how to transform text strings, customize spacing, and create excerpts.


### Adjusting Text Spacing

In CSS you specify the amount of white space between each letter or word by
giving a length value for the letter-spacing and word-spacing properties,
respectively. The amount of space can be in px, pt, cm, em, etc.

In Flutter, you specify white space as logical pixels (negative values are allowed)
for the ```letterSpacing``` and wordSpacing ```properties``` of a
[TextStyle](https://docs.flutter.io/flutter/painting/TextStyle-class.html)
child of a Text widget.


<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  letter-spacing: 4px; [[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: new Text(
        "Lorem ipsum",
        style: new TextStyle(
          color: Colors.white,
          fontSize: 24.0,
          fontWeight: FontWeight.w900,
[[highlight]]          letterSpacing: 4.0, [[/highlight]]
        ),
      ),
      decoration: new BoxDecoration(
        color: Colors.red[400],
      ),
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Transforming Text
In HTML/CSS, you perform simple case transformations using the text-transform
property.

In Flutter, you transform the contents of a Text widget using the methods and
operators of the [String class](https://docs.flutter.io/flutter/dart-core/String-class.html)
in the dart:core library.
<div class="lefthighlight">
{% prettify css%}
<div class="greybox">
  <div class="redbox">
[[highlight]]    Lorem ipsum [[/highlight]]
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  text-transform: uppercase; [[/highlight]]
}
{% endprettify %}
</div>

<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: new Text(
[[highlight]]        "Lorem ipsum".toUpperCase(), [[/highlight]]
        style: bold24Roboto,
      ),
      decoration: new BoxDecoration(
        color: Colors.red[400],
      ),
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Making Inline Formatting Changes

A [Text](https://docs.flutter.io/flutter/widgets/Text-class.html) widget lets
you display text with the same formatting characteristics. To
display text that uses multiple styles (in this example, a single word with
emphasis), use a [RichText](https://docs.flutter.io/flutter/widgets/RichText-class.html)
widget instead. Its ```text``` property can specify one or more
[TextSpan](https://docs.flutter.io/flutter/painting/TextSpan-class.html) widgets
that can be individually styled.

In the following example, "Lorem" is in a TextSpan widget with the default
(inherited) text styling, and "ipsum" is in a separate TextSpan with custom
styling.


<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
[[highlight]]    Lorem <em>ipsum</em> [[/highlight]]
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
[[highlight]]  font: 900 24px Roboto; [[/highlight]]
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
}
[[highlight]] .redbox em {
  font: 300 48px Roboto;
  font-style: italic;
} [[/highlight]]
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: [[highlight]] new RichText(
        text: new TextSpan(
          style: bold24Roboto,
          children: <TextSpan>[
            new TextSpan(text: "Lorem "),
            new TextSpan(
              text: "ipsum",
              style: new TextStyle(
                fontWeight: FontWeight.w300,
                fontStyle: FontStyle.italic,
                fontSize: 48.0,
              ),
            ),
          ],
        ),
      ), [[/highlight]]
      decoration: new BoxDecoration(
        backgroundColor: Colors.red[400],
      ),
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>

### Creating Text Excerpts

An excerpt displays the initial line(s) of text in a paragraph, and handles the
overflow text, often using an ellipsis. In HTML/CSS an excerpt can be no longer
than one line. Truncating after multiple lines requires some JavaScript code.

In Flutter, use the ```maxLines``` property of a [Text](https://docs.flutter.io/flutter/widgets/Text-class.html)
widget to specify the number of lines to include in the excerpt, and the
```overflow``` property for handling overflow text.

<div class="lefthighlight">
{% prettify css %}
<div class="greybox">
  <div class="redbox">
    Lorem ipsum dolor sit amet, consec etur
  </div>
</div>

.greybox {
  background-color: #e0e0e0; /* grey 300 */
  width: 320px;
  height: 240px;
  font: 900 24px Roboto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.redbox {
  background-color: #ef5350; /* red 400 */
  padding: 16px;
  color: #ffffff;
[[highlight]]  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap; [[/highlight]]
}
{% endprettify %}
</div>
<div class="righthighlight">
{% prettify dart %}
var container = new Container( // grey box
  child: new Center(
    child: new Container( // red box
      child: new Text(
        "Lorem ipsum dolor sit amet, consec etur",
        style: bold24Roboto,
[[highlight]]        overflow: TextOverflow.ellipsis,
        maxLines: 1, [[/highlight]]
      ),
      decoration: new BoxDecoration(
        backgroundColor: Colors.red[400],
      ),
      padding: new EdgeInsets.all(16.0),
    ),
  ),
  width: 320.0,
  height: 240.0,
  color: Colors.grey[300],
);
{% endprettify %}
</div>
<div class="end-examples"></div>
