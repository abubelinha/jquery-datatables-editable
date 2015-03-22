**[Overview](Overview.md)** > **[Editing cells](EditCell.md)** > Read-only cells
# Introduction #

Editable plugin enables you to mark some of the cells in the table as a read-only cells. Editors are not applied on the read-only cells.


# Details #

## Mark columns as read-only ##

some of the columns i your table should not be editable. As example if you have a column with links you will not want to edit links. In this case you can define that entire column is read only if you set null in the aoColumn parameters in the plug-in settings. Example is shown in the following code:
```
$('#example').dataTable()
             .makeEditable({

         "aoColumns": [
                    	null,
                    	{
                        },
                    	{
                    		type: 'textarea',
                    		submit: 'Save changes'
                    	},
                    	null,
                    	{
                    		type: 'select'
                    	}
		]
    });

```

In this code, in the aoColumn setting is defined that first and fourth columns are not editable.

## Mark cells as read-only ##
To mark cell as read-only class "read\_only" should be added to the TD tag that contains read-only text. Example is shown in the following listing:

```
 <table id="myDataTable">
        <thead>
            <tr>
                <th>Company name</th>
                <th>Address</th>
                <th>Town</th>
            </tr>
        </thead>
        <tbody>
                <tr id="17">
                    <td class="read_only">Emkay Entertainments</td>
                    <td>Nobel House, Regent Centre</td>
                    <td class="read_only">Lothian</td>
                </tr>
                <tr id="18">
                    <td>The Empire</td>
                    <td class="read_only">Milton Keynes Leisure Plaza</td>
                    <td>Buckinghamshire</td>
                </tr>
        </tbody>
</table>
```

In this example 1<sup>st</sup>, 3<sup>rd</sup>, and 5<sup>th</sup> cells in the table are marked as read-only. Therefore, clicking on these cells,  editor will not be shown.
You can use these classes to make entire rows or columns read-only - just generate "read\_only" class to the cells you want to make non-editable. You can see how read-only cells are configured on the [live demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/index.html) page.

If you don't want to use read\_only class you can change it in the editor settings as shown in the following example:

```
$('#example').dataTable()
             .makeEditable({
                sReadOnlyCellClass: "editor_disabled"
        });
```

In this example is defined that cells with class editor\_disabled will be read-only.