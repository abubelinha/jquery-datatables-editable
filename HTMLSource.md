**[Overview](Overview.md)** > Client-side HTML source
# Introduction #

Web page where the DataTables Editable plugin is used must be build according to the plugin requirements. On this page is described what need to be placed on the client web page in order to run DataTables Editable plugin correctly.

# Details #

In order to successfully enhance table, following tasks need to be done on the client-side:
  * Include necessary JavaScript libraries
  * Properly format HTML table that will be enhanced
  * Initialize table

## Included JavaScript libraries ##

DataTables Editable plugin integrates several libraries/plugins while enhancing the HTML table. Following JavaScript libraries are required:
  * JQuery library v1.4.4. containing standard classes used by DataTables plugin
  * JQuery UI library v1.8.7. – containing classes for handling dialogs
  * JQuery DataTables plugin v1.7.5. including optional DataTables CSS style-sheets used for applying default styles on the page
  * JQuery JEditable plugin v1.6.2. – required for inline cell editing
  * JQuery validation plugin v1.7. for implementation of client-side validation

The libraries listed above should be downloaded and placed in the local site. Following HTML code include these libraries under assumption that they are placed in the /Scripts/ folder:

```

        <script src="/Scripts/jquery-1.4.4.min.js" type="text/javascript"></script>
        <script src="/Scripts/jquery.DataTables.min.js" type="text/javascript"></script>
        <script src="/Scripts/jquery.jeditable.js" type="text/javascript"></script>
        <script src="/Scripts/jquery-ui.js" type="text/javascript"></script>
        <script src="/Scripts/jquery.validate.js" type="text/javascript"></script>
        <script src="/Scripts/jquery.DataTables.editable.js" type="text/javascript"></script>

```

## HTML table format ##

HTML table that is enhanced must be plain HTML table with the following requirements:
  * Table must have an id
  * Table must have standard THEAD and TBODY sections
  * Each TR tag in the body of the table must have an id of the record that is listed in the table row.

Example of the valid table is:

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
                    <td>Emkay Entertainments</td>
                    <td>Nobel House, Regent Centre</td>
                    <td>Lothian</td>
                </tr>
                <tr id="18">
                    <td>The Empire</td>
                    <td>Milton Keynes Leisure Plaza</td>
                    <td>Buckinghamshire</td>
                </tr>
                <tr id="19">
                    <td>Asadul Ltd</td>
                    <td>Hophouse</td>
                    <td>Essex</td>
                </tr>
                <tr id="21">
                    <td>Ashley Mark Publishing Company</td>
                    <td>1-2 Vance Court</td>
                    <td>Tyne &amp; Wear</td>
                </tr>
	</tbody>
</table>
```

## Initialization code ##

JQuery DataTables Editable plugin is initialized using the following code:

```
        <script language="JavaScript" type="text/javascript">
            $(document).ready(function () {
                $('#myDataTable').dataTable().makeEditable();
            });
        </script>
```

myDataTable is an id of the HTML table that should be enhanced. First, DataTables plugin is applied to add common table functionalities, and then this table is made editable using the plugin created in this project.

## Add and delete buttons ##

Two buttons that will be used for add and delete actions must be added to the page so DataTables Editable plugin can find them and attach the event handlers that will perform add and delete actions. Example of the buttons that should be added is:

```
<button id="btnAddNewRow">Add</button>
<button id="btnDeleteRow">Delete</button>
```

It is important that these buttons have the exact ids as displayed in the example above. DataTables Editable plugin will try to find them by exact id.

If you do not want to add buttons, you can just place any div in the page with a class add\_delete\_toolbar, as it is shown below:

```

<div class="add_delete_toolbar" />

```
When plugin detects that buttons does not exist but there is an element with a class "add\_delete\_toolbar", it will auto-generate and inject the "add" and "delete" buttons in this element.


## Add new record form ##

To add new record completely custom blank form needs to be added to a page. Example of the form is:


```

<form id="formAddNewRow" action="#">
        <label for="name">Name</label><input type="text" name="name" id="name" class="required" rel="0" />
        <br />
        <label for="name">Address</label><input type="text" name="address" id="address" rel="1" />
        <br />
        <label for="name">Postcode</label><input type="text" name="postcode" id="postcode"/>
        <br />
        <label for="name">Town</label><input type="text" name="town" id="town" rel="2" />
        <br />
        <label for="name">Country</label><select name="country" id="country">
                                            <option value="1">Serbia</option>
                                            <option value="2">France</option>
                                            <option value="3">Italy</option>
                                         </select>
        <br />
</form>

```


Arbitrary fields can be placed within the form. A DataTables Editable plugin will handle standard operations opening/closing form in the new dialog, sending AJAX request when user confirm adding, etc.
Only requirement is that the fields that need to be copied back to the original table when update is performed must be marked with rel attribute. Rel attribute must have position of the cell where value of the input field should be placed if the add operation succeed. Any field in the form that should not be added in the table should not be mapped to the table with a rel attribute.