# Kendo MVVM DorpDownList Component
## Basic MVVM Multi-Column with post requests
HTML Body
```html
<div>
    <h4>Choose a category e.g. 'Fruits'</h4>
    <input 
        data-role="multicolumncombobox"
        data-placeholder="Type a category e.g. 'Chai'"
        data-auto-bind="false"
        data-auto-close="false"
        data-text-field="category_name"
        data-value-field="_id"
        data-columns="[
            {field: 'category_name', title: 'Name'},
            {field: 'parent_category', title: 'Parent'}
        ]"
        data-filter-fields = "['category_name', 'parent_category']"
        data-bind="
            value: selectedCategory,
            source: categoryData,
            visible: isVisible,
            enabled: isEnabled,
            events: {
                change: onCategoryChange,
                select: onCategorySelect,
                open: onCategoryOpen,
                close: onCategoryClose
            }
        "
        style="width: 100%;" 
    />
</div>
```
Script Body
```javascript
let viewModel = kendo.observable({
    selectedCategory: null,
    isVisible: true,
    isEnabled: true,
    displaySelectedCategory: function () {
        let selectedCategory = this.get("selectedCategory");
        return kendo.stringify(selectedCategory, null, 4);
    },
    onCategoryOpen: function () {
        kendoConsole.log("event :: open");
    },
    onCategoryChange: function () {
        kendoConsole.log("event :: change (" + this.displaySelectedCategory() + ")");
    },
    onCategoryClose: function () {
        kendoConsole.log("event :: close");
    },
    onCategoryClose: function () {
        // Only Select event passes the selected item
        if (e.dataItem) {
            let dataItem = e.dataItem;
            console.log("event :: select (" + dataItem.category_name + " : " + dataItem._id + ")");
        } else {
            console.log("event :: select");
        }
    },
    categoryData: new kendo.data.DataSource({
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
                            posted_by: "Nazmul"
                        }) 
                    };
                }
            }
        }
    })
});
```