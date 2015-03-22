**[Overview](Overview.md)** > **[Editing cells](EditCell.md)** > Non-standard custom column editor types

See also: [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html) [Standard custom editor types](CustomCellEditors.md)

# Introduction #

JEditable supports the following types of inline editors Text input, Text area, and Select list. In the CustomCellEditors page you can find description how to use these editors. JEditable supports other custom input types that can be added as plugins (examples are date picker, checkbox etc). You can integrate these editors in your table.

# Adding non-standard editors #

Non-standard inline editors are added the same way as the standard ones - in the aoColumns array are set types of editors. The only prerequisite is that jeditable plugin files must be included on the page because code for the non-standard editors is not part of jquery.jeditable.js plugin. Example code for setting non-standard inline editors is shown in the following listing:

```
<script type="text/javascript" charset="utf-8">
$(document).ready(function () {
    $('#example').dataTable()
                 .makeEditable({
        sUpdateURL: "UpdateData.php",
        "aoColumns": [
                    	{
                    		type: 'datepicker'
                    	},
                    	{
                    		type: 'checkbox'
                    	},
                    	{
                    		type: 'timepicker'
                    	}
		]
    });
})
</script>
```

For each column, in the type property you should put a name of the editor that will be used in that column will be used. You can how it works in the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html).

## Inline check box editor ##

To add inline check box you will need to include jquery.jeditable.checkbox.js JavaScript file. One version can be found on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html), but you can find latest version on JEditable plug-in site. once you take this file you should add it to the page - something like:
```
<script src="media/js/jquery.jeditable.checkbox.js" type="text/javascript"></script>
```
Once you include this file, you will need to specify that for some column should be used check box as an editor. In the following example is specified that for the first column is used check box editor, while other columns are not editable:

```
$('#example')
        .dataTable()
        .makeEditable({
                       sUpdateURL: "UpdateCell.php",
                       "aoColumns": [
                                     {  type:   'checkbox',
		                        submit: 'OK',
		                        cancel: 'Cancel',
 				        checkbox: { trueValue: 'Yes',
                                                    falseValue: 'No'
                                                  }
  				     },
                                     null,
                                     null,
                                     null,
                                     null
                                    ]
                        });
```

First, you will need to set type of editor to 'checkbox'. Good practice is to add Ok and Cancel buttons by the check box, but this is optional. On the [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/checkbox.html) you can see how it works without buttons. You can define what values will be sent to the server-side when check box is checked or unchecked in the check box property (in this case will be sent values Yes and No).
You can see it in action on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html) and [checkbox](http://jquery-datatables-editable.googlecode.com/svn/trunk/checkbox.html) page.

## Inline date picker ##

Date picker is standard jquery ui dialog that can be used to enter date in the cell. To add inline date picker you will need to include jquery.jeditable.datepicker.js and standard jquery-ui.js file(to include JQuery ui date picker in the page). One version can be found on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html), but you can find latest version on JEditable plug-in site. Once you take this file, you should add it to the page - something like:
```
<script src="media/js/jquery.jeditable.datepicker.js" type="text/javascript"></script>
```
Once you include this file, you will need to specify that for some column should be used date picker as an editor. In the following example is specified that for the first column is used date picker editor, while other columns are not editable:

```
$('#example')
        .dataTable()
        .makeEditable({
                       sUpdateURL: "UpdateDate.php",
                       "aoColumns": [
                                     {  type:   'datepicker',
					sSuccessResponse: "IGNORE"
  				     },
                                     null,
                                     null,
                                     null,
                                     null
                                    ]
                        });
```

As you can see this editor do not have some specific options - you can only set type of editor. You can see it in action on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html) page.

## Inline time editor ##

Time editor enables you to enter time in the hh:mm format. To add inline time editor you will need to include jquery.jeditable.time.js file. One version can be found on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html), but you can find latest version on JEditable plug-in site. Once you take this file you should add it to the page - something like:
```
<script src="media/js/jquery.jeditable.time.js" type="text/javascript"></script>
```
Once you include this file, you will need to specify that for some column should be used time picker as an editor. In the following example is specified that for the first column is used time picker editor, while other columns are not editable:

```
$('#example')
        .dataTable()
        .makeEditable({
                       sUpdateURL: "UpdateTime.php",
                       "aoColumns": [
                                     {  type:   'time',
					submit: 'OK',
					cancel: 'Cancel'
  				     },
                                     null,
                                     null,
                                     null,
                                     null
                                    ]
                        });
```

You should set the type of editor to 'time', add buttons for submit and cancel. You can see it in action on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html) page.

## Inline file upload ##

File upload editor enables you to upload new file in the cell. To add inline upload you will need to include jquery.jeditable.ajaxUpload.js file. One version can be found on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html), but you can find latest version on JEditable plug-in site. Once you take this file you should add it to the page - something like:
```
<script src="media/js/jquery.jeditable.ajaxUpload.js" type="text/javascript"></script>
```
Once you include this file, you will need to specify that for some column should be used file upload as an editor. In the following example is specified that for the first column is used file upload editor, while other columns are not editable:

```
$('#example')
        .dataTable()
        .makeEditable({
                       sUpdateURL: "UpdateTime.php",
                       "aoColumns": [
                                     {  type:   'ajaxupload',
					submit: 'upload',
					cancel: 'Cancel'
  				     },
                                     null,
                                     null,
                                     null,
                                     null
                                    ]
                        });
```

You should set the type of editor to 'ajaxupload', add buttons for upload and cancel. You can see it in action on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html) page.

# Handling errors/success actions #

One important thing is how you can notify plug-in that server-side page has successfully updated cell.
  1. If server-side page has updated cell you will need to return the same value that is sent to the server. In this case plug-in will update cell value in the table.
  1. If server-side page has failed to update cell your will need to return ERROR <Error message text>. In this case, plug-in will show <Error message text> in alert, and cancel update. You can see error messages in action on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html) page. Try to change time and you will see server-side page returns ERROR <Error message text>, and that plug-in revert change and displays error message in alert.


See also: [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-edit-extra.html) [Standard custom editor types](CustomCellEditors.md)