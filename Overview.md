# Site content #
  * [HTML code](HTMLSource.md)
  * [Editing cells](EditCell.md)
  * [Adding new records](AddingNewRecords.md)
  * [Deleting rows](DeleteRecord.md)
  * [Customization/API](Customization.md)

# Introduction #

The JQuery DataTables Editable plugin enhances the standard HTML table by adding data management functionalities on the client-side:

  * Select and delete a row in the table
  * Edit cell values inline
  * Add a new row

An example is shown below:

![http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/datatables_edit_cell.png](http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/datatables_edit_cell.png)



The JQuery [http://datatables.net](DataTables.md) plugin provides standard navigation functionalities such as pagination, ordering, filtering by keyword, and changing the number of items that should be shown per page. In addition to these, the JQuery DataTables Editable plugin adds the ability to add a new record, select and delete records and edit the cells inline as shown in the figure.
This plugin integrates several plugins such as JQuery DataTables, JQuery Editable, JQuery Validation plugins and implements common data management functionalities.

All client-side processing is encapsulated within the plugin. It needs server-side code that accepts add, update and delete requests. Server-side code can be implemented in any language (PHP, ASP.NET/C#, Java) as long as the plugin interface requirements are met.

You can download complete examples and see how it works.



# Implementation of client-side web page #
The first step in the integration of JQuery DataTables Editable plugin into the your application is implementing the client-side, detailed instructions can be found [here](HTMLSource.md). This section contains an overview of the implementation. There are two important elements that should be added to the client-side web page:
  * a HTML table that contains data,
  * JavaScript initialization code that enhances the table.

## Table Source ##
This plugin works with a plain HTML table, for example:

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

As you can see it looks like a normal HTML table, the only difference is that an ID for each record has to be placed in the TR tags.

## Initializing the JQuery DataTables Editable plugin ##
The JQuery DataTables Editable plugin integrates several plugins that add different functionalities to the table. The most important one is the JQuery DataTables plugin that adds filtering, ordering and pagination functionalities to the table.

The JQuery DataTables Editable plugin is initialized using the following code:

```
        <script language="javascript" type="text/javascript">
            $(document).ready(function () {
                $('#myDataTable').dataTable().makeEditable();
            });
        </script>
```

myDataTable is an id of the HTML table that should be enhanced. First, the DataTables plugin is applied to add common table functionalities, and then this table is made editable using the plugin created in this project.

# Implementation of the server side #

When the DataTables Editable plugin is initialized, it sends Update, Delete and Add requests to the server-side. Example AJAX calls made by the plugin are shown in the figure below:

![http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/Ajax_trace.png](http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/Ajax_trace.png)

The following sections describe what should be implemented on the server-side to make this plugin fully functional.

## [Selecting and deleting rows](DeleteRecord.md) ##
JQuery DataTables Editable plugin enables the user to select and delete rows in the table. When the user presses the delete button, an AJAX request is made with information of the id of the record that is deleted. Example :

```
   http://www.site.com/folder/DataData?id=22
```
The URL of the delete page can be changed in plugin configuration.  The id value that is sent to the server-side is placed as an id attribute in the `<tr>` tag.

The server-side page should return "ok" if a record is successfully deleted or an error message if the delete request failed.

## [Editing cell values](EditCell.md) ##
Editing a cell is done inline by clicking on it. An AJAX request with cell position and cell value is sent to the server-side when the user finishes editing.

The update request has the following parameters:

  * value - the edited text
  * id - id of the record that is edited (id is placed in TR tag that surrounds the cell that has been edited)
  * columnId - position of the column of the cell that has been edited (hidden columns are omcluded in the count)
  * columnPosition - position of the column of the cell that has been edited (hidden columns are not counted)
  * rowId - id of the row containing the cell that has been edited

The server should return the new value for the cell if the cell was successfully updated on the server-side, or an error message.

## [Adding new rows](AddingNewRecords.md) ##
To enable adding new record you will need to create a custom HTML form  where a user can enter information about the new record. When the user enters information about new record in the form, these will be posted to the server-side page.

The server-side should accept the input values entered in the form and return the id of the new record.
Detailed information about adding new records using the DataTables Editable plugin can be found [here](AddingNewRecords.md).