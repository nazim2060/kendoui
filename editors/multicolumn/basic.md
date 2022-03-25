# Basic Kendo Multi-Column Component

## Basic Multi-Column with simple **GET** requests
```javascript
$("#categories").kendoMultiColumnComboBox({
    optionLabel: "Select a category...",
    autoClose: false,
    autoBind: false,
    filter: "contains",
    dataTextField: "category_name",
    dataValueField: "_id",
    height: 400,
    noDataTemplate: "No category found",
    footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> categories found',
    columns: [
        { field: "category_name", title: "Category Name", width: 200 },
        { field: "parent_category", title: "Contact Title", width: 200 }
    ],
    filterFields: ["category_name", "parent_category"],
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
}).data("kendoMultiColumnComboBox");
```

## Basic Multi-Column with **POST** requests
```javascript
$("#cities").kendoMultiColumnComboBox({
    optionLabel: "Select a city...",
    autoClose: false,
    autoBind: false,
    filter: "contains",
    dataTextField: "city_name",
    dataValueField: "_id",
    noDataTemplate: "No city found",
    footerTemplate: 'Total number of <strong>#: instance.dataSource.total() #</strong> cities found',
    columns: [
        { field: "city_name", title: "City Name", width: 200 },
        { field: "country_name", title: "Country", width: 200 }
    ],
    filterFields: ["city_name", "country_name"],
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
}).data("kendoMultiColumnComboBox");
```