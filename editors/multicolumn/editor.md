# Kendo Multi-Column Component for GRID and FILTER editor mode

## Basic Multi-Column with **GET** requests for `grid editor or filter editor mode`
```javascript
function roleEditor(container, options) {
    let input = $(`<input id="${options.field}" name="${options.field}" style="width: 100%;" required />`);
    input.appendTo(container)
    input.kendoMultiColumnComboBox({
        autoClose: false,
        autoBind: false,
        filter: "contains",
        dataTextField: 'role_name',
        dataValueField: '_id',
        noDataTemplate: "No user role found",
        footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> roles found',
        columns: [
            { field: "role_name", title: "Name", width: 200 },
            { field: "parent_role", title: "Parent", width: 200 }
        ],
        filterFields: ["role_name", "parent_role"],
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
                console.log("event :: select (" + dataItem.role_name + " : " + dataItem._id + ")");
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

## Basic Multi-Column with **POST** requests for `grid editor or filter editor mode`
```javascript
function roleEditor(container, options) {
    let input = $(`<input id="${options.field}" name="${options.field}" style="width: 100%;" required />`);
    input.appendTo(container)
    input.kendoMultiColumnComboBox({
        autoClose: false,
        autoBind: false,
        filter: "contains",
        dataTextField: 'role_name',
        dataValueField: '_id',
        noDataTemplate: "No user role found",
        footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> roles found',
        columns: [
            { field: "role_name", title: "Name", width: 200 },
            { field: "parent_role", title: "Parent", width: 200 }
        ],
        filterFields: ["role_name", "parent_role"],
        dataSource: {
            type: "json",
            transport: {
                read: {
                    url: "/api/role/get",
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
                console.log("event :: select (" + dataItem.role_name + " : " + dataItem._id + ")");
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