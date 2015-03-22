**[Overview](Overview.md)** > **[Editing cells](EditCell.md)** > Standard custom column editor types

See also: [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/custom-editors.html) [Non-standard custom editor types](NonStandardEditors.md)

# Introduction #

In the JEditable plugin text box is default cell editor, however, JEditable plugin that is used for editing provides other editor types.

<a href='http://www.appelsiini.net/projects/jeditable'>JQuery JEditable plugin</a> that is used as a inline cell editor in the DataTable Editable plugin, supports inline editing of various input types - text box, textarea, and select list. To initialize the editors that will be applied on the columns, parameter aoColumns needs to be set in the plugin initialization call as shown in the following example:

```
<script type="text/javascript" charset="utf-8">
$(document).ready(function () {
    $('#example').dataTable().makeEditable({
        sUpdateURL: "UpdateData.php",
        "aoColumns": [
                    	null,
                    	{
                        },
                    	{
                    		indicator: 'Saving platforms...',
                    		tooltip: 'Click to edit platforms',
                    		type: 'textarea',
                    		submit: 'Save changes'
                    	},
                    	{
                    		type: 'select',
                    		onblur: 'cancel',
                    		submit: 'Ok',
                    		loadurl: 'EngineVersionList.php',
                    		loadtype: 'GET',
                                event: 'click'
                    	},
                    	{
                    		type: 'select',
                    		onblur: 'submit',
                    		data: "{'':'Please select...', 'A':'A','B':'B','C':'C'}",
                                event: 'mouseover'
                    	}
		]
    });
})
</script>
```

This is an array of settings that will be applied on the individual table columns. This array should have the same number of columns as original table. Values set as an array elements can have following values:
  * 'null' value marking entire column as read-only (no editor will be aplied on the cells in the column,
  * {} value that set default editor to the column cells,
  * JSON object with properties that will be applied to the editors. You can find more details about the properties below:

# Column editor properties #

DataTables Editable plugin delegates cell editing functionalities to the JEditable plugin and just passes properties in the aoColumns to the editors for the particular columns. Each element of the aoColumns contains both properties that will be passed to <a href='http://www.appelsiini.net/projects/jeditable'>JQuery JEditable </a> plugin, and custom editor properties specific to the DataTables Editable plugin.

## Standard JEditable settings ##

JSON object with properties same as the properties of  <a href='http://www.appelsiini.net/projects/jeditable'>JQuery JEditable </a> plugin. Example of the usage of standard JEditable settings is shown in the following script:

```

aoColumns:[
           {
             indicator: 'Saving Dropdown',
             tooltip: 'Click to show dropdown',
             loadtext: 'loading...',
             type: 'select',
             onblur: 'cancel',
             submit: 'Ok',
             loadurl: 'EngineVersionList.php',
             loadtype: 'GET'
           },
           {
             indicator: 'Saving Text',
             tooltip: 'Click to show textbox',
             onblur: 'submit',
             event: 'mouseover'
           },
           {
             onblur: 'ignore'
           },
]
```

You can add submit button in the cell editor if you want - otherwise value will be submitted when the user press ENTER key. Leaving the cell will cancel changes. Note that if you use a text area as an input element instead of text box or select list you should use explicit submit button. In the text area, ENTER key is used for the new line, so you will not be able to post changes to the server if explicit submit button is not placed. See more details about the settings on the [JEditable](http://www.appelsiini.net/projects/jeditable) site.

See how these setting are applied on the [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/custom-editors.html).

## DataTables Editable settings ##

There are few settings specific to the DataTables Editable plugin that can be set to each individual editor. Settings are:
  * fnOnCellUpdated - function that will be called after the cell is updated,
  * sUpdateURL - string containing the URL of the page that should to update the cell, or function that will be called be called when cell is updated. If function is used, the first parameter is the value that will be sent to the server, and the second parameter is JEditable settings object,
  * oUpdateParameters - additional parameters tat will be sent to the sUpdateURL page. These parameters can be either constants or functions,
  * sSuccessResponse - response toke that will be returned from the server if cell is successfully updated. Default value is "ok". If value is set to "IGNORE" validation will be ignored.

Example of the DataTables Editable settings are shown below:

```

aoColumns:[
           {
             fnOnCellUpdated: function(sStatus, sValue, settings){
		             },
             sUpdateURL: "CustomUpdatePage.php",
             oUpdateParameters:{
                                type:"A",
                                count: function(){ 
                                          return $("tr").length;
                                },
             sSuccessResponse: "ok"
           },
           {
             sUpdateURL: function(value, settings){
                		return value;
 			},
             sSuccessResponse: "IGNORE"
           }
]                

```

See also: [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/custom-editors.html) [Non-standard custom editor types](NonStandardEditors.md)