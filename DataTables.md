# Introduction #

JQuery DataTables Editable plugin is a JQuery plugin that enhances standard HTML table and add data management functionalities (adding new records to the table, selecting and deleting records, and editing cells inline).

All required processing is encaplulated within the plugin, and only required implementation is a server-side code that accepts add, update and delete requests. Server-side code can be implemented in any language (PHP, ASP.NET/C#, Java) as long as plugin interface requirements are met.
This page contains short description explaining the features of the plugin. You can download complete example and see how it works.

# Details #

JQuery DataTables Editable Plugin enables user to make the following actions in the table:
  * Select and delete row in the table
  * Edit cell values inline
  * Add new row

Plugin handles all necessary functionalities - only required implementation is server-side logic that handles AJAX requests sent by the plugin.

# Implementation of clent-side web page #
First step in the integration of JQuery DataTables Editable plugin intp you application is implementation of the client-side page. Detailed instructions about implementation of client-side code can be found [here](HTMLSource.md). In this section is shown overview of implementation. There are two important elements that should be added to the client-side web page:
  * HTML table that contains data,
  * JavaScript initialization code that enchances the table.

## Table Source ##
This plugin works with a plain HTML table. Example of the HTML table is shown below:

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

As you can see standard HTML table need to be placed in the HTML source of the page. Only difference is that id of the each record need to be placed in the TR tags as an attribute.

## Initializing JQuery DataTables Editable plugin ##
JQuery DataTables Editable plugin integrates several plugins that add different functionalities to the table. Most important plugin is JQuery DataTables plugin that adds filtering, ordering and pagination functionalities to the table. This plugin is built on the top of DataTables plugin and it adds additional data-management functionalities. JQuery DataTables Editable plugin is initialized using the following code:

```
        <script language="javascript" type="text/javascript">
            $(document).ready(function () {
                $('#myDataTable').dataTable().makeEditable();
            });
        </script>
```

myDataTable is an id of the HTML table that should be enhanced. First, DataTables plugin is applied to add common table functionalities, and then this table is made editable using the plugin created in this project.

# Implementation of the server-side pages #

When the DataTables Editable plugin is initialized, it sends Update, Delete and Add requests to the server size. To accept these actions, server-side pages that handles data management requests need to be created. In the following sections is described what should be implemented on the server-side to make this plugin fully functional.

## Selecting and deleting rows ##
JQuery DataTables Editable plugin enables user to select and delete rows in the table. When user press delete button, plugin creates an AJAX request to the server-side with information of the id of the record that is deleted. Example of request that is sent to the server-side is:

```
   http://www.site.com/folder/DataData?id=22
```
URL of the delete page can be chnaged in plugin configuration.  id value that is sent to the server-side is placed as an id attribute of `<tr>` tag.

On the server-side, one page that handles delete AJAX requests that accepts one parameter (id) need to be created. Server-side page should return string "ok" if a record is successfully deleted or error message if delete failed.

## Editing cell values ##
Editing cell is done inline by clicking on the table cells. Whenuser finishes with editing AJAX request with cell position and cell value is sent to the server-side.

One sever-side page that handles update AJAX requests need to be created. This page should accepts the following parameters:

  * value - that contains text used entered in the table cell while editing
  * id - id of the record that is deleted (id is placed in TR tag that surrounds the cell that has been edited)
  * columnId - position of the column of the cell that has been edited (hidden columns are counted also)
  * columnPosition - position of the column of the cell that has been edited (hidden columns are not counted)
  * rowId - id of the row containing cell the cell that has been edited

Server-side page should return value that is updated if cell is successfully updated on the server-side or an error message.

## Adding new rows ##
To enable adding new record you will need to create a custom HTML form  where a user can enter information about the new record. When user enter information about new record in the form, these will be posted to the server-side page.

On the server-side one page that handles add AJAX requests need to be created. This page should accept input values entered in the form and return id of the new record.