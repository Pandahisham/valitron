Valitron 0.2 beta
========

jQuery validation plugin for [Laravel](http://www.laravel.com/), with Twitter bootstrap markup support.

Valitron name credits goes to [daylerees](http://daylerees.com/) :)

## Some news
- Support error translation!
- Reworked object witch should be returned by validation rule.
- Added for replacing certain parts of messages, such as :max for max rule and so.

## What can it do?

- Obviously validate user input data using different rules.
- Apply validation while user typing.
- Automaticaly mark invalid fields with "error" class
- Can grab validation rules declared in html data-validation element.

## What can I do?

- Add new validation rules.
- Configure invalid field marking.
- Declare error and success validation callbacks.
- Change default options and default config.

### All callbacks gets array of objects witch defines element, rule, parameters and message
Each array object contains following parameters:
- rule - a rule applied
- parameters - parameters for rules
- message - the message
- type - type to use for translation

# Some examples
Text input with some rules declared for validation:
```html
<input id="test" type="text" data-validation="required|min:2|max:30">
```
A JavaScript part for validation, hook a live validation, 
data-validation rules will be applied after 500ms cooldown then user stops typing.
```javascript
$("#test").valitron('live');
```
Need to cache successfull of failed validation? Just pass some callbacks
```javascript
$("#test").valitron('live', {
	error : function(errors, success) {	// you get an array of validation messages, errors and success
		for ( var msg in messages )
		{
			console.log(
				msg.rule,		// A rule applied
				msg.parameters,	// Rule parameters
				msg.message,	// Validation rule message, either translated or default one
			);
		}
		// this refers to input DOM element
		$(this).addClass("error");
	},
	success : function(success, error)
	{
		$(this).removeClass("error");
	},
	timeout: 1000 // timout to trigger validation in ms
}
});
```
If you return anything from callbacks that evolutes to true, globalError or globalSuccess will be called too

### Need to validate on button click?
```html
<input id="test" type="text" data-validation="required|min:2|max:30">
<button id="do_check" type="button">Validate!</button>
```
```javascript
$("#do_check").on('click', function()
{
	$("#test").valitron('validate');
})
```
## Validate and live validation support such options
- rules: [] - rules to check, default "|" delimiter, ":" parameters, ex: max:5|min:2
- language : 'en' - default language to use for errors, somehow should be loaded :)
- success : null - passes jQuery element and message, function(message) {}, this refers to jquery object
- error : null - passes jQuery element and message, function(messagae) {}, this refers to jquery object
- beforeValidate : null - executes before validation, returned value will be used for testing!
- afterValidate : null - executes after validation
- valid : false - indicates that vield is valid and doesnt contain any errors
- timeout : 500 - Timeout for live validation
- timer: null - Timer reference
- errors: {} - holds last validation errors( 0.2 beta )
- messeges : {} - custom error messages, attribute_rule notation for specific error for attribute, and only ruleName for rule error ( 0.2 beta )

## Whant to change default options or config?
```javascript
$.valitron('options', {
	// supported options goes here
})
```
Changes to global config
```javascript
$.valitron('config', {
	globalSuccess : function(){} // global success, this refers to jquery object, there is actual code inside functions ;)
	globalError : function(){} 	// global error, this refers to jquery object
	ruleDelimiter : "|" 		// Delimiter used to separate rules.
	ruleMethodDelimiter : ":"	// Rule and its parameter delimiter
	ruleParamDelimiter: ","		// Rule parameters delimiter
	ruleDataElement: 'validation'// html data element name witch holds validation rules
})
```
### Valitron support all jquery selectors
All selected elements will get valitron instances, hooked callbacks will be applied on each.
```html
Login:<input class="test" type="text" data-validation="required|min:2|max:30"><br>
Password:<input class="test" type="password" data-validation="required|min:15">
<button id="do_check" type="button">Take me to heaven!</button>
```
```javascript
$(".test").valitron("live", {
	error: function(messeges) {
		// both input fields will have error callbacks
	}
})
```
## Replacers