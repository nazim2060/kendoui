# Basic Kendo DorpDownList Component

## Basic Dropdown with simple **GET** requests
```javascript
$("#categories").kendoDropDownList({
    optionLabel: "Select a category...",
    autoClose: false,
    autoBind: false,
    filter: "contains",
    dataTextField: "category_name",
    dataValueField: "_id",
    noDataTemplate: "No category found",
    footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> categories found',
    dataSource: {
        type: "json",
        serverFiltering: false,
        transport: {
            read: {
                url: "/api/store/customer/city/get",
                type: "GET"
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
}).data("kendoDropDownList");
```

## Basic Dropdown with **POST** requests
```javascript
$("#cities").kendoDropDownList({
    optionLabel: "Select a city...",
    autoClose: false,
    autoBind: false,
    filter: "contains",
    dataTextField: "city_name",
    dataValueField: "_id",
    noDataTemplate: "No city found",
    footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> cities found',
    dataSource: {
        type: "json",
        serverFiltering: false,
        transport: {
            read: {
                url: "/api/store/customer/city/get",
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
}).data("kendoDropDownList");
```