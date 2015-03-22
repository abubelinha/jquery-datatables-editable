# Introduction #

Here are placed various tips and tricks that can help you to use plugin in some non-standard cases.


# How to send another cell content in the update request #
When cell is updated, plugin sends information about the id of the row, updated value, column name, column index, etc. However in some cases you might want to include some additional information in the update request e.g. content of the first cell in the row. This is not standard feature, but you can combine fnOnEditing and oUpdateParameters to achieve this.
In the fnOnEditing function, you can take the input where content is edited, find TR parent that surrounds it, find first TD in that TR, and put that value in some local variable.
Then, you can in oUpdateParameters define additional parameter that is function that read local variable and add it to the input set parameters. I would look like the following code:

```
var cell= '';
$('#example')
        .dataTable()
        .makeEditable({
		fnOnEditing: function(input)
	                     { 	
                                    cell= input.parents("tr")
                                               .children("td:first")
                                               .text();
		                    return true
 			    },
		oUpdateParameters: {
				cell: function(){ return cell; } 
			}
      		});
```

Instead of the first cell you can put any other value in the request.