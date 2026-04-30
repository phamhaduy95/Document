
draw mimic 
- Documents
- Work Area
dynamic graphic

window management

scripting
UI graphic interface
feature:
basic control:



user can drag and drop item from the list of object below
Interactive object plan
- `Button`
- `Checkbox`
- `RadioButton`
- `Combobox`
- `Editbox`
- `Slider`
- `Spinner`
- `Listbox`

area select


Trending Control
Alarm and Event Summary
Time bar control
Update Control


Tab order
user can specify the order of highlight or item focused when pressing Tab key repeatedly 



Data
Live data: data that is displayed as it is received or after a short time lag.
History data: data record from the past.
History replay data: replay particular incident or show current and past data for comparison purpose.

Property sheet
- able co customize control styling and register event handler
	support we have object called input.
	from property sheet window, we can set input size (width, height), border-color


find how `draw.io` save or hydrate its shape as JSON data
how can we model shape object data ?

`maxGraph` is the main engine behind well-known online drawling tool `draw.io`

it support almost every features that are necessary for completed drawing app such as
- native redo/undo 
- grouping shape

`maxGraph` provides dedicated classes (`SvgCanvas2D` and `ImageExport`) that mathematically redraw your graph's exact state into a clean, standalone XML/SVG document.

import custom SVG image 
To build a professional diagramming tool where shapes act natively—meaning users can change their colors, resize them without distorting line thickness, and snap connecting arrows to specific points on the shape—you cannot just drop raw SVGs onto the canvas.

You must convert the SVG into **Stencil XML**.

require a step to convert SVG into Stencil XML format

file-level synchronization.

By hijacking the coordinates and passing the `cell` data to React/Vue, you can use modern UI libraries (like Material UI, Tailwind UI, or Radix) to render a beautiful, animated context menu that perfectly matches the rest of your application's design system

