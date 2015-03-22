**[Overview](Overview.md)** > **[Editing cells](EditCell.md)** > Cell editor validation

See also: [Demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-validation.html)

# Introduction #

JQuery DataTables Editable plugin enables developers to quick and easy set the validations on the individual cell editors. DataTable Editable plugin supports validation for inline editing.

## Validation via CSS classes ##

The simplest way to validate inline edited cells is to associate some validation class to cell editor. In the aoColumns settings we can define what CSS class should be applied to the particular cells during editing, as it is shown in the following example:
```
$('#example').dataTable().makeEditable({
    sUpdateURL: "UpdateData.php",
    "aoColumns": [
                    null,
                    {
                    },
                    {
                    	cssclass: "required"
                    },
                    {
                    	cssclass: "email"
                    },
                    {
			cssclass: "date"
                    }
		]
});
```

These classes are used for validation of input values before cell is updated. Validation classes that can be used are the same classes that are used in <a href='http://docs.jquery.com/Plugins/Validation'>JQuery validation plugin</a> so please refer to the documentation of his plugin for any customization.

On the [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-validation.html) can be seen how inline validation works.
## Advanced validation using validation rules/messages ##

For more advanced validation options you can specify validation settings for each column via oValidationOptions property. Example is shown below:

```
$('#example').dataTable().makeEditable({
    sUpdateURL: "UpdateData.php",
    "aoColumns": [
                    null,
                    null,
                    {
                    	
                    },
                    {
                    	oValidationOptions : 
                                   { 
                                       rules:{ 
                                               value: {
                                                       required: true,
                                                       creditcard: true
                                                      }
                                       },
				       messages: { 
                                               value: {
                                                       creditcard: "Invalid credit card" } 
                                       }
                                   }
                    },
                    {
                    	oValidationOptions : 
                                   { 
                                       rules:{ 
                                               value: {
                                                       minlength: 5
                                                      }
                                       },
				       messages: { 
                                               value: {
                                                       minlength: "Enter at least 5 characters" 
                                                       } 
                                       }
                                   }
                    }
		]
});
```

In this example is defined that fourth column should be required valid credit card, and that fifth column should be at least five character long. Also, this way you can define custom error messages that will be shown when validation rule is not satisfied.
See more details about validation properties in <a href='http://docs.jquery.com/Plugins/Validation'>JQuery validation plugin</a>.

On the [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-validation.html) can be seen how inline validation works.

See also: [Demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/inline-validation.html)