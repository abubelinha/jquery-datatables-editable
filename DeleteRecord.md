**[Overview](Overview.md)** > Deleting rows in the table
# Table of Content #

# Introduction #

DataTables Editable plugin enables user to select and delete the rows in the table. Delete button is always disabled but when user select any row in the table it becomes enabled and user can delete selected row by pressing it.

# Client-side implementation #

There are tow ways to add delete functionalities to the DataTable:
  * Adding one delete button that will delete selected row
  * Adding delete link in each row in the table

## Adding separate delete button ##

Delete button that will be used by the user to delete rows must be manually placed in the page. Only requirement is that delete button must have an id "btnDeleteRow". Example of the delete button is shown in the following example:
```
<button id="btnDeleteRow">Delete</button>
```
Position, style and text of the button can be changed. Only requirement it that id must be id="btnDeleteRow" because plugin finds the delete button be this id.

### Setting the server-side page ###

In the plugin initialization call should be set parameter sDeleteURL, that is used to define what server-side page should be used for submitting the add AJAX request. Example of the initialization code is shown below:
```
$('#myDataTable').dataTable().makeEditable({
                    sDeleteURL: "/Home/DeleteData.php"
                }); 
```

## Put inline delete link in each row in the table ##

If you do not like select/delete functionality you can put delete link in each row. In that case, you will need to add class called 'table-action-deletelink' in each link and put the url that will be called to delete row in the href attribute. Example of that kind of link is shown in the following code:

```
<a class="table-action-deletelink" href="DeleteData.php?id=2">Delete</a>
```

You can see it in action on the
[live demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/ajax-inlinedeletebuttons.html) page.

# Server-side implementation #
Server side page that handles "delete record" AJAX request should read id from the request sent by plugin, delete the record by id and return back string "ok". If deleting of the record fails, this page should return error text - any text different than "ok" will be shown to the user as an error message. In the following sections are show examples of the server-side page implementation in the PHP, ASP.NET MVC, and Java.

## PHP Example ##
Example of the server-side page that can be implemented in PHP is shown below:
```
<?php
  //DeleteData.php
  $id = $_REQUEST['id'] ;

  /* Delete a record by id */ 

  echo "ok";

?>
```
Plugin should be initialized with sDeleteURL parameter with value "/DeleteData.php" in order to send request to the !Delete.php page.

## C# Example ##
Example of the server side page that can be implemented in the ASP.NET MVC using C# language is shown below:
```
public class CompanyController : Controller
{
        public string DeleteData(int id)
        {
            /* Delete a record by id */

            return "ok";
        }
}
```
Plugin should be initialized with sDeleteURL parameter with value /Company/DeleteData in order to map request to the DeleteData method of the CompanyController.

## Java Example ##
Example of the Java Servlet that handles request is shown below:
```
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class DeleteDataHandler extends HttpServlet {

  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
      throws ServletException, IOException {

     String id = request.getParameter("id");

     /*  Delete a record by id */

     PrintWriter out = response.getWriter();
     out.println("ok");
  }
}
```
## Summary ##
In the examples above it can be noticed that server-side pages looks same. They accepting parameters, add record and return back id of the created record. Interface is same in all implementations only difference is in the business logic that creates new records.

# Customization #

## Changing id of the delete button ##
Id attribute of the button can be configured. Following code is used to define custom id of the delete button:
```
$('#myDataTable').dataTable().makeEditable({
                        sDeleteRowButtonId: "btnDeleteCompany"
                });
```
In this case, a delete button must have a "sDeleteRowButtonId" id.

## Customize delete event handlers ##

You can set methods that will be called before and after delete action. Example is shown in the following code:

```
$('#myDataTable').dataTable()
                 .makeEditable({
         			fnOnDeleting: function(tr, id)
				{ 	
					$("#trace").append("Deleting row with id " + id);
					return true;
				},
         			fnOnDeleted: function(status)
				{ 	
					$("#trace").append("Deleted action finished. Status - " + status);
				}
                });
```

Functions fnOnDeleting and fnOnDeleted are called before and after delete action.
  * Function fnOnDeleting gets the id of the record that will be deleted and the reference to row that will be deleted. This is useful if you want to add some client-side logic that check whether the row should be deleted. Note that you will need to return either true or false value to let plugin know whether it should proceed with delete action.
  * Function fnOnDeleted just gets the status of delete action.

You can see how it works on the [live demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/events.html) page.

## Customize look of Delete button ##

You can convert standard delete button to JQuery UI button. You will need to pass parameters for initialization of button via oDeleteRowButtonOptions parameter. Example is shown below:
```
$('#example').dataTable().makeEditable({
                            oDeleteRowButtonOptions: {
				label: "Remove",
                                icons: { primary: 'ui-icon-trash' }
                            }
                          });
```

In this case is set label "Remove" on the button with trash icon. You can see full list of options on the [JQuery UI button](http://jqueryui.com/demos/button/) demos/documentation page.

See how it works on the [live example](http://jquery-datatables-editable.googlecode.com/svn/trunk/customize-buttons.html) page.

## Customize confirmation message ##

When user press delete button, standard JavaScript alert box will be shown so user can confirm that he want to perform this action. You can override this behavior by setting your own handler that will be executed before delete Ajax request is sent. Example is shown in the following code:
```

$('#example').dataTable()
             .makeEditable(
                 {
                    fnOnDeleting: function (tr, id, fnDeleteRow) {
                        jConfirm('Please confirm that you want to delete row with id ' + id, 'Confirm Delete', function (confirmed) {
                            if (confirmed) {
                                fnDeleteRow(id);
                            }
                        });
                        return false;
                    }
                  });

```

In this example is overridden OnDeleting handler that is called before delete action. This handler returns false in order to prevent standard delete action. You will need to stop standard delete action because in this function you will choose whether you should proceed with delete or not.
Also it opens custom dialog form confirmation and if user confirms that he want to delete row, handler calls fnDeleteRow method with id of the row that should be deleted. You can see how it works on the [live demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/custom-messages.html) page.


## Customize error message ##

If delete fails on the server-side, plugin will show message returned from server and retain row. By default standard JavaScript alert() is user where is shown content of the server-side page response.
However, you can change this behavior, use your own dialog, or even you can put your own message.
In the following example you can see how you can override method for showing error messages:
```

$('#example').dataTable()
             .makeEditable(
                 {
                    fnShowError: function (message, action) {
                        switch (action) {
                            case "delete":
                                jAlert(message, "Delete failed");
                                break;
                            default:
                                alert(message);
                                break;
                        }
                    }
                  });

```
This function receives two parameters - a message that should be shown (usually error message returned from server), and an action that has caused an error. In this example instead of standard alert I have used jAlert plug-in. You can see how it works on the [live demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/custom-messages.html) page.

> Note that function is used to show error messages for other action too (edit, and delete), so when you override it make sure that you provide default action or override those actions too.

# See also #
  * [Live demo](http://jquery-datatables-editable.googlecode.com/svn/trunk/index.html)
  * [Adding new record](AddingNewRecords.md)
  * [Editing cells in the table](EditCell.md)