# Kendo DorpDownList Component for GRID and FILTER editor mode

## Basic Dropdown with **GET** requests for `grid editor or filter editor mode`
```javascript
function roleEditor(container, options) {
    let input = $(`<input id="${options.field}" name="${options.field}" style="width: 100%;" required />`);
    input.appendTo(container)
    input.kendoDropDownList({
        autoClose: false,
        autoBind: false,
        filter: "contains",
        dataTextField: 'role_name',
        dataValueField: '_id',
        noDataTemplate: "No user role found",
        footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> roles found',
        dataSource: {
            type: "json",
            transport: {
                read: {
                    url: "/api/role/get"
                }
            }
        },
        select: function(e){
            // Only Select event passes the selected item
            if (e.dataItem) {
                let dataItem = e.dataItem;
                console.log("event :: select (" + dataItem.category_name + " : " + dataItem._id + ")");
            } else {
                console.log("event :: select");
            }
        },
        change: function(){
            console.log("Change event does not pass any value.");
        }
    });
}
```

## Basic Dropdown with **POST** requests for `grid editor or filter editor mode`
```javascript
function roleEditor(container, options) {
    let input = $(`<input id="${options.field}" name="${options.field}" style="width: 100%;" required />`);
    input.appendTo(container)
    input.kendoDropDownList({
        autoClose: false,
        autoBind: false,
        filter: "contains",
        dataTextField: 'role_name',
        dataValueField: '_id',
        noDataTemplate: "No user role found",
        footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> roles found',
        dataSource: {
            type: "json",
            transport: {
                read: {
                    url: "/api/category/get",
                    type: "POST"
                },
                parameterMap: function (options, operation) {
                    if (operation === "read") {
                        return { 
                            models: kendo.stringify({
                                country_name: "Qatar"
                            }) 
                        };
                    }
                }
            }
        },
        select: function(e){
            // Only Select event passes the selected item
            if (e.dataItem) {
                let dataItem = e.dataItem;
                console.log("event :: select (" + dataItem.category_name + " : " + dataItem._id + ")");
            } else {
                console.log("event :: select");
            }
        },
        change: function(){
            console.log("Change event does not pass any value.");
        }
    });
}
```