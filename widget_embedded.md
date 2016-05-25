
# Embedded Widgets

## \<sp-widget />
You can embed any widget inside of your widget’s [HTML template](widget_html.md) using the `sp-widget` directive. This directive requires a complete widget model which you can get using [spUtil.get()](#get_one) on the client or [$sp.getWidget](#get_one) on the [server script](/widget_server_script.md)

Use it like this:

```html
<sp-widget widget="c.myCatItemWidget"></sp-widget>
```

Widget Model in depth
------
The widget model contains all of the client-side parts of a widget needed to create an angular directive. The HTML template, client script, and link function are loaded just as they are in the sp_widget record. The data property is the result of the widget's server script execution. Anything that you put on the data object on the server is available in the data object on the client.

Here is a detailed look at some of the fields in the widget model: 

| Property name | Type | Description |
| ------------- | ---- | ----------- |
| client_script | string | The widget's client script field |
| css | string | The compiled css output from the widget's sass field |
| data | object | The data object containing all of the keys and values added to it in the widget's server script |
| dependencies | array | A collection of javascript libraries to load before the widget executes |
| options | object | The options used to initialize the widget |
| template | string | The widget's HTML Template field |



<a name="get_one" href="#get_one"></a>   

spUtil.get() - Get a widget model via client script
------

```javascript
spUtil.get("widget-sc-cat-item", {sys_id: "your_catalog_item_sys_id"}).then(function(response) {
	c.catalogItemWidget = response;
});
```
**Parameters**

- (_string_) widget\_id  
   Can be a widget_id or widget sys_id.
- (_object_) data  
   An object to post to the widget's server script. Refer to this object as **input** in your server script.

**Callback**  

The callback function is called when the widget model is ready. The response object contains the full widget model.


$sp.getWidget() - Get a widget model via server script
------

```javascript
data.catalogItemWidget = $sp.getWidget("widget-sc-cat-item", {sys_id: "your_catalog_item_sys_id"});
```
**Parameters**  

- (*string*) widget\_id  
   Can be a widget_id or widget sys_id.  
- (*object*) options  
   An object to pass to the widget's server script. Refer to this object as **options** in your server script.


Examples
------

#### <a name="embedding_with_client_script" href="#embedding_with_client_script">#</a> Embedding a widget using client script

This example will show you how to create a new widget and test page, and how to embed the cool clock widget in a bootstrap panel.

1. First go to the widget editor: /sp_config?id=widget_editor

2. Click "Create a new widget" and use the following values  
   **Widget Name:** Embedded clock  
   **Widget Id:** embedded_clock  
   **Create a test page:** True  
   **Page Id:** embedded_clock  
   
   ![New widget dialog](./assets/widget_embedded/example_clock_1.png)
3. Click Submit

Now, copy the following code blocks into your new embedded clock widget:


HTML Template

```html
<div class="my-clock">
	<div class="panel panel-default ">
		<div class="panel-heading">Time in San Diego</div>
		<div class="panel-body">
			<sp-widget widget="c.myClockWidget"></sp-widget>
		</div>
	</div>
</div>
```
<br />
Client Script

```javascript
function(spUtil) {
	var c = this;
	spUtil.get("widget-cool-clock", null).then(function(response){
			c.myClockWidget = response;
	});
}
```
<br />
CSS  

```css
.my-clock {
	margin: 25px;
	width: 300px;
	height: 300px;
}
```
<br />
Result  <br />
In a new browser window go to: /$sp.do?id=embedded_clock  

![Embedded clock](./assets/widget_embedded/example_clock_2.png)

#### <a name="embedding_with_server_script" href="#embedding_with_server_script">#</a> An example using server script