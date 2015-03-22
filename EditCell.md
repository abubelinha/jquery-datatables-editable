**[Overview](Overview.md)** > Editing cells

---

More - [Advanced settings](EditorSettings.md) [Custom editors](CustomCellEditors.md) [Validation](CellEditorValidation.md)  [Non-standard editors](NonStandardEditors.md)  [Read-only cells](ReadOnlyCells.md)
# Table of content #


# Introduction #

The DataTables Editable plugin allows the user to edit the cells in the table. The core of inline editing is  implemented in the[JEditable plugin](http://www.appelsiini.net/projects/jeditable). The DataTables Editable plugin is internally configured to replace cell content with an editable textbox when the user double clicks on the cell. The following figure shows how user can edit data:

![http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/datatables_edit_cell.png](http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/datatables_edit_cell.png)

When the user finishes editing an AJAX call is sent to the server-side page that updates the record.

# Client-side configuration #

On the client-side (web page) minimal configuration is required. You need to provide the URL of the server-side update page during plugin initialization. The plugin will handle all other operations (inline editing, sending AJAX request, error handling). The only requirement is that the web page should be created according to the [guideline for the web page implementation](HTMLSource.md).

## Setting the server-side page used for update ##
Set the parameter sUpdateURL in the plugin initialization call, for example:

```
$('#myDataTable').dataTable().makeEditable({
                    sUpdateURL: "/Home/UpdateData.php"
                }); 
```

The AJAX call will be sent to the page /Home/UpdateData.php when the userhas finished editing the cell.
Instead of the URL, a function can be used as a target for posting values. This is shown in the following example:

```
$('#myDataTable').dataTable().makeEditable({
                    sUpdateURL: function(value, settings)
				{
					return(value);
				}
                });
```

This function accepts the value that is entered in the editor, and the settings of the current editor. The function should return the same value that is posted - any other text will be shown as an error message.
Use this option when you need to implement some custom processing of the value before it is sent using some custom AJAX call.

## AJAX Request ##

When the user finishes editing a cell, an AJAX request will be sent to the URL defined in the sUpdateURL parameter. An example  AJAX request is shown below:
![http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/update_data_ajax_request.png](http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable/update_data_ajax_request.png)

The AJAX request contains the following values:
  * id of the row. This is taken from the id attribute of the TR tag that surrounds the edited cell. Use this value to find a record that should be updated.
  * value - the value that is entered in the cell.  This value should be written in the company record.
  * columnName - the name of the column. This value is either the sName property set for the column or text found in the column heading. You can use this information to determine which property should be updated.
  * rowId the row ID from the HTML table.  If 10 rows are shown per page this will be a value between 0 and 9.
  * columnPosition - the position of the column, a value from 0 to the number of columns you see in the table - 1. Hidden columns are not counted. This value can be used instead of the column name to identify the property that should be updated. Use this value if names of the columns can be dynamically changed.
  * columnId - the id of the column from 0 to the total number of columns - 1. Hidden columns are counted. This value can be used instead of the column name to identify the property that should be updated.  You should use columnId instead of the columnPosition if you have hidden columns in the table (either initially hidden or dynamically hidden).


You can see the plugin in action on the [live demo page](http://jquery-datatables-editable.googlecode.com/svn/trunk/index.html). Note that the edit action returns a server error because the google.code site does not allow POST requests.

### Setting the columnName parameter ###

The parameter columnName should be name of the column where the value should be updated. By default, the plug-in takes this information from the text in the TH cell of the column that is edited. If the text in the column matches your database table column name that is everything you will need.
However, you can explicitly define what value should be set in each column if you set the sName parameter in the aoColumns setting during DataTable plugin initialization. An example is shown in the following code:

```

$('#myDataTable').dataTable({
                    aoColumns: [
				{
				    sName: "colTitle"
				},
				{
				    sName: "colAddress"
				}, 
				{
				    sName: "colDescription"
				},
				{
				    sName: "colNote"
				}
                              ] 

                 })
                 .makeEditable();
```

If you setup sName parameters in the aoColumn setting of the DataTable plugin, these values will be sent to the server-side update page as columnName parameters.
Good practice is to set actual names of the table columns in these parameters, so you can set some free text in heading cells.

# Server-side page implementation #
On the server-side one page that accepts the parameters described in the previous section needs to be created. The server-side page should accept these parameters and update record in the database. In the sections below are shown short examples of implementation in the PHP, ASP.NET MVC, and Java

## PHP Example ##

```
<?php
  //UpdateData.php
  $id = $_REQUEST['id'] ;
  $value = $_REQUEST['value'] ;
  $column = $_REQUEST['columnName'] ;
  $columnPosition = $_REQUEST['columnPosition'] ;
  $columnId = $_REQUEST['columnId'] ;
  $rowId = $_REQUEST['rowId'] ;

  /* Update a record using information about id, columnName (property
     of the object or column in the table) and value that should be
     set */ 
  echo $value;

?>
```
The plugin should be initialized with sUpdateURL parameter with value /UpdateData.php in order to send request to the UpdateData.php page.

## ASP.NET MVC Example ##


```
public class HomeController : Controller
{        
        public string UpdateData(   int id, string value, 
                                    int? rowId, int? columnPosition,
                                    int? columnId, string columnName)
        {
            //Add code to update cell
            return value;
        } 
} 
```

The plugin should be initialized with sUpdateURL parameter with value /Home/UpdateData in order to map the request to the UpdateData method of the HomeController. You can download the code example in the tutorial on the [Code Project](http://www.codeproject.com/KB/aspnet/MVC-CRUD-DataTable.aspx).

## Java Example ##

An example of a Java Servlet page that handles request is shown below:

```

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class Home extends HttpServlet {

  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
      throws ServletException, IOException {

     String id = request.getParameter("id");
     String value = request.getParameter("value");
     String columnName = request.getParameter("columnName");
     String columnId = request.getParameter("columnId");
     String columnPosition = request.getParameter("columnPosition");
     String rowId = request.getParameter("rowId");

     /*  Update record */

     PrintWriter out = response.getWriter();
     out.print(value);
  }
}

```

You can download the code example in the tutorial on the [Code Project](http://www.codeproject.com/KB/java/J2EE-Editable-Web-Table.aspx).
# Summary #
In the examples above it can be noticed that the server-side pages look very similar. They are accepting parameters, update records and return back the value that is edited. This interface is same in all implementations, the only difference is in the business logic code that updates the data.
# See also #
  * [Demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/index.html) - Live demo page. Note that the edit action returns a server error because the google.code site does not allow POST requests.
  * [Editor settings](EditorSettings.md) a page to see how you can additionally configure editor parameters.
  * [Custom editors](CustomCellEditors.md) and [non-standard editors](NonStandardEditors.md) pages to see how you can use editor types other than text boxes.
  * [Adding validation in the cell editors](CellEditorValidation.md) to see how you can implement validation of edited values.
  * [Adding new record](AddingNewRecords.md) - How to implement adding new rows in the table,
  * [Deleting records in the table](DeleteRecord.md) - How to implement deleting rows from the table.