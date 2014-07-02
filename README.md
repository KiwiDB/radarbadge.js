RadarBadge.js
=============

# RadarBadge documentation
RadarBadge is a jQuery plugin which creates a modified radar chart meant for displaying comparitive skills, traits or any other items. Each item passed is given a numeric rating which is used to generate the chart. You see these charts in video games sometimes to display the strengths and weaknesses of different characters.

This plugin can be simple, but by passing custom options it can be used in complex ways.

> This version 1.0 of the plugin has a few caveats to keep in mind:
> * You must have an inline width and height set on the SVG tag (using the svgAttrs option). The current version will not evaulate the height and width of the parent element to determine size.
> * SVG animation support is determined by `document.implementation.hasFeature("http://www.w3.org/TR/SVG11/feature#Animation", "1.0");` and are disabled if not supported. This method is not always accurate (it reports lack of support for some browsers that do support it), and I will be exploring other methods for getting this information.
> * If you use the `rebuild` method, you must pass all custom callbacks again, even if `saveData` is enabled.

## Getting started

First, include the files for jQuery and the RadarBadge plugin

```html
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
<link href="jquery.radarbadge.min.css" rel="stylesheet">
<script type="text/javascript" src="jquery.radarbadge.min.js"></script>
```
    
The minified versions (.min.js and .min.css) will perform better, so using them is preferred. The standard non-minified files (.js and .css) are included for reference and to allow further development.
    
Once included, you need only to target elements using a jQuery selector and invoke the RadarBadge plugin.

Create an element on your site that you want to use as a container for the plugin.

```html
<div id='radarbadge'></div>
```

Then target it with the RadarBadge plugin. Be sure to add the call inside of a document.ready function if necessary!

```javascript
$(function() {
    $("#radarbadge").RadarBadge(options);
});
```
    
### Minimal setup

```javascript
$("myselector").RadarBadge(options);
```

options is an object of customizations for your chart. Only one option is needed for a meaningful chart -- "items", an array of items to display on the chart. Each is an object containing three elements -- "id" (an ID used internally by the plugin and used in classes, allowing you to target elements with CSS easily), "label" (the text to display on the chart), and "level" (the rating for this item; by default, the chart uses levels 0-5, with 5 representing the maximum).

For example:

```javascript
$("myselector").RadarBadge({
  items: [
    {"id": "item1", "label": "Item One", "level": 3},
    {"id": "item2", "label": "Item Two", "level": 5},
    {"id": "item3", "lavel": "Item Three", "level": 2}
  ]});
```

### Options

Most peoperties of the chart can be updated to better meet the look and needs of your site. Most of the style (colors, fonts, opacity, etc) are set using CSS; use your own CSS to overwrite the default. If you want to create a style from scratch, you do not need to include the CSS file from the plugin.

The following is a list of the additional options that can be set through the plugin call.

#### items (default: [])

These are the items to display on the chart. The option takes an array of objects; each object must specify an "id" (an ID used internally by the plugin and used in classes, allowing you to target elements with CSS easily), "label" (the text to display on the chart), and "level" (the rating for this item; by default, the chart uses levels 0-5, with 5 representing the maximum). You may pass include any other information you wish (you might want to do this if you are using the mouseover or click callbacks).

```javascript
$("myselector").RadarBadge({
  items: [
    {"id": "item1", "label": "Item One", "level": 3},
    {"id": "item2", "label": "Item Two", "level": 5},
    {"id": "item3", "lavel": "Item Three", "level": 2}
  ]});
```

Techicanlly it is not required that you pass an items option; however, the chart is not meaningful until you have at least three items.

#### maxLevels (default: 5)

This option sets the maximum number of levels to display within the chart. The maximum level will represent the perimeter of the circle. If an item has a level greater than the maxLevels option, the chart will leave the bounds of the circle.

```javascript
$("myselector").RadarBadge({maxLevels: 10});
```

#### svgAttrs (default: {class: "radar-badge", width: 500, height: 500})

These are attributes to assign inline to the SVG element. Any attributes you pass in the svgAttrs option will be merged with the default.

> Note that if you change the class name from "radar-badge" the plugin CSS will no longer target the chart correctly.

```javascript
$("myselector").RadarBadge({svgAttrs: {class: "radar-badge my-skills", width: 200}});
```

#### circlePortion (default: 0.25)

This option represents the portion of the size of the SVG element that will be used as the radius for the chart circle. Setting this to 0.5 will cause the chart to span the full height or width of the SVG (whichever is smaller). It is up to you to ensure that your item labels and chart fit in the SVG.

```javascript
$("myselector").RadarBadge({circlePortion: 0.5});
```

#### useCenterDot (default: true)

This option determines if a dot is drawn in the center of the chart.

```javascript
$("myselector").RadarBadge({useCenterDot: false});
```

#### centerDotSize (default: 3)

This option represents the radius of the center dot, in pixels. (useCenterDot must be set to true for this to have an effect)

```javascript
$("myselector").RadarBadge({centerDotSize: 5});
```

#### textDistance (default: 1.25)

This option is used to determine the position of item labels relative to the perimeter of the circle. The position is determined by taking a position on the circle perimeter and multiplying it by this option. (The higher this option is, the further from the circle the item labels will display.) It is up to you to ensure that your item labels and chart fit in the SVG.

```javascript
$("myselector").RadarBadge({textDistance: 1.5});
```

#### usePointDots (default: true)

This option determines if a dot is drawn at each vertex of the chart.

```javascript
$("myselector").RadarBadge({usePointDots: false});
```

#### pointDotsSize (default: 3)

This option represents the radius of the points dots, in pixels. (usePointDots must be set to true for this to have an effect)

```javascript
$("myselector").RadarBadge({pointDotsSize: 5});
```

#### autoAnimate (default: true)

If set to true, the chart will begin its display animation immediately; if set to false, the chart will remain hidden until the "animate" method is invoked through the API.

If you want to have the chart display without an animation, set this option to true and "animationDuration" to 1.

```javascript
$("myselector").RadarBadge({autoAnimate: false});
```

#### animationDuration (default: 1000)

Specifies the length of the animation in milliseconds.

If you want to have the chart display without an animation, set this option to 1.

```javascript
$("myselector").RadarBadge({animationDuration: 2000});
```

#### saveData (default: false)

If set to true, the plugin will save the options you set. You must enable this option if you wish to use the "add" or "delete" API methods. If you use the "restart" API method, setting this option to true will allow you not to pass your options again, instead needing to pass only the options you wish to change. (See the documentation for the API methods for more information)

```javascript
$("myselector").RadarBadge({saveData: true});
```

### Callbacks

Here you can pass callbacks; the function will be executed durring execution of the plugin.

#### start

This is a function to be called before any processing is done on the chart.

```javascript
function myFunc() {
  ...
}

$("myselector").RadarBadge({start: myFunc});
```
    
#### mouseover

This is a function to be called whenever the mouse is moved over the SVG element. The function receives two arguments -- the event object, and the item object that was mouseover-ed.

The pointer-to-item calculation is handled by the coToIndx function, which can be overwritten.

```javascript
function myFunc(event, index) {
  ...
}

$("myselector").RadarBadge({mouseover: myFunc});
```

#### mouseout

This is a function to be called whenever the mouse is moved out of the SVG element.

```javascript
function myFunc() {
  ...
}

$("myselector").RadarBadge({mouseout: myFunc});
```

#### click

This is a function to be called whenever the SVG element is clicked. The function receives two arguments -- the event object, and the item object that was clicked.

The pointer-to-item calculation is handled by the coToIndx function, which can be overwritten.

```javascript
function myFunc(event, index) {
  ...
}

$("myselector").RadarBadge({click: myFunc});
```

#### coToIndx

This function receives an x and y coordinate, and must return one of two things -- either the index of the item related to the passed x and y coordinate (the index of the item in the items array passed as an option to the plugin), or false if no item is found.

```javascript
function myFunc(x, y) {
  ...
}

$("myselector").RadarBadge({coToIndx: myFunc});
```

## API

The RadarBadge plugin allows you to interact with your chart after its initial creation using these API methods. The name of the method must be passed as the first parameter, followed by any options needed.

#### animate

This forces the chart to animate. If the chart has already animated when this is called, the animation will happen a second time.

There are no options to pass to this method. The length of the animation is passed during the initial creation of the chart, or when using the restart method.

```javascript
$("myselector").RadarBadge("animate");
```

#### add

> The "add" method is only available if saveData has been enabled

This method allows you to add a new item or items to the chart. The method must receive an array of item objects, each specifying at least three items -- "id" (an ID used internally by the plugin and used in classes, allowing you to target elements with CSS easily), "label" (the text to display on the chart), and "level" (the rating for this item; by default, the chart uses levels 0-5, with 5 representing the maximum).

The new items will not be displayed on the chart until the rebuild method has been invoked.

```javascript
$("myselector").RadarBadge("add", [
  {"id": "item4", "label": "Item Four", "level": 1},
  {"id": "item5", "label": "Item Five", "level": 3}
}]);
```

#### delete

> The "Delete" method is only available if saveData has been enabled

This method allows you to remove an item or items from the chart. The method must receive an array of IDs of items to be removed.

The items will not be removed from the display until the rebuild method has been invoked.

```javascript
$("myselector").RadarBadge("delete", ["item2", "item3"]);
```

#### rebuild

This method will destroy your chart and recreate it, taking a new set of options. If the add or delete methods have been used, those changes will take effect as well. This function may receive one argument, an options array; all items available as options to the initial creation of the chart are available here.

> If saveData was enabled BEFORE calling rebuild, you only need to pass any options you wish to change from their initial value. If saveData is not enabled, you will need to pass the entire set of options you wish applied to the chart. (If a new set of items are passed, they will overwrite the previous items; use the add method if you do not wish to overwrite the item list)

> IMPORTANT NOTE - whether or not saveData is enabled, you must repass any callback functions here; they will not be remembered from the initial chart creation.

```javascript
$("myselector").RadarBadge("rebuild", options);
```

#### destroy

This method destroys the SVG and other elements created by the plugin.

```javascript
$("myselector").RadarBadge("destory");
```

## Example

###### HTML
```html
<div id='radarbadge'></div>
```

###### Javascript
```javascript
var items = [
    {"id": "item1", "label": "Item One", "level": 3},
    {"id": "item2", "label": "Item Two", "level": 5},
    {"id": "item3", "label": "Item Three", "level": 2}
];
    
$("#radarbadge").RadarBadge({
    items: items,
    centerDotSize: 2,
    usePointDots: false,
    autoAnimate: false,
    saveData: true,
    click: function(e, i)
    {
        alert("You clicked " + i.label + "!");
    }
});

$("myselector").RadarBadge("animate");
```
