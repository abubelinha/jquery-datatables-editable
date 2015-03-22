**[Overview](Overview.md)** > **[Editing cells](EditCell.md)** > Settings
# Introduction #

There are several options/settings that can be applied in order to customize editor behavior.

## Standard options ##

The following options can be used to customize update functionality:
  * sUpdateURL - URL of the page that will be used to update cell value on the server side, or function that will be called when cell is updated. If function is used, the first parameter is the value that will be sent to the server, and the second parameter is JEditable settings object,
  * sReadOnlyCellClass - class that will mark read-only cells. On these cells will not be applied editors.
  * oUpdateParameters - additional parameters that will be sent in the Ajax call to the sUpdateURL (see [demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-parameters.html)),
  * ajaxoptions - set of Ajax options that will be applied to the Ajax call (see [JQuery site](http://api.jquery.com/jQuery.ajax/) for details). This setting is useful if you want to change HTTP protocol from POST to GET.

Example of the customization is shown in the following example:

```

$('#example').dataTable()
             .makeEditable({
		//sUpdateURL: "UpdateData.php",
                sUpdateURL: function(value, settings){
                                return value;
                },
            	sReadOnlyCellClass: "read_only"
            	oUpdateParameters: { 
			"status": "1",
			"number": function(){ return 11; }
                },
            	ajaxoptions:{ type: 'GET' }
	});

```

## Events Handlers ##

There are two event handler function that can be attached to the editors:
  * fnOnEditing(jInput, oEditableSettings, sOriginalText, id) - function that will be called before edit request is sent. Parameter jInput is a JQuery wrapper for the input that is used as a JEditable inline editor, oEditableSettings is settings set to the JEditable plugin, sOriginalText is an original text, and id is id of the row. If function returns false, edit action will be canceled (useful for the custom validation).
  * fnOnEdited(status) - function that will be called when edit action is finished. Parameter status is a string "success" or "failure".

```

$('#example').dataTable()
             .makeEditable({
            	fnOnEditing: function(jInput, oEditableSettings, sOriginalText, id)
		{ 	
		  alert("Updating cell with value " + input.val() + ". Sending request to " + oEditableSettings.target);
		  return true;
		},
            	fnOnEdited: function(status)
		{ 	
		  alert("Edit action finished. Status - " + status);
		}
	});

```

See example on the [demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/events.html).

## Changing the inline edit events ##

Instead of the default double click event for inline editing you can use any other JQuery event. In the following example is shown how you can set click event to initiate inline editing:

```
$('#example').dataTable()
             .makeEditable({
									
		oEditableSettings: { event: 'click' }
			});

```
See example on the [demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/custom-edit-events.html).