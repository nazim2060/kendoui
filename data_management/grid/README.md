# Kendo UI Grid for jQuery

Kendo UI Grid is a powerful control for displaying data in a tabular format. It is powerful because it has so many built-in powerful features, such as:
1. run CRUD operation with just 4 API's
2. export data natively in Excel and PDF format
3. paging, sorting, filtering, grouping
4. search panel, checkbox selection, aggregates, frozen columns, column menu, and templating
5. Incorporate external kendo ui components, such as: sparklines, barchart, badge etc. and many more....

https://demos.telerik.com/kendo-ui/grid/index

## Kendo UI Grid has 4 core parts:
1. Model: This is where you declare the schema. You can think of it as a duplicate of Mongoose Schema. In the `Model` you declare the unique/primary key field, then the fileds and field types that the grid will receive from the server.
2. Columns
3. DataSource
4. Initialize the Grid

# Rule of Thumb!
## When we are using kendo grid, we will always make sure of the followings:

### 1. The command column of the grid is named as `Actions`. The command column will not show more than 2 buttons. These 2 buttons are edit and delete button. The width of the column should be `120px` for 2 buttons and `70px` for one button. The buttons will only have icons, and no text.
### 2. `dataSource` should include the `error` configuration function to show errors.
### 3. The `kendoGrid` object should include the following configurations:
> `moibile`
```javascript
mobile: true
```
> `pageable`
```javascript
pageable: {
    pageSizes: true,
    buttonCount: 5,
    refresh: true
}
```
> `sortable`
```javascript
sortable: true
```
> `resizable`
```javascript
resizable: true
```
> `columnMenu`
```javascript
columnMenu: true
```
> `height`
```javascript
height: 550
```
> `toolbar`
```javascript
toolbar: [
    "create",
    "excel",
    {
        template: `<span class="k-spacer"></span>`
    },
    "search"
],
```
> `excel export`
```javascript
excel: {
    fileName: "Export.xlsx",
    proxyURL: "https://demos.telerik.com/kendo-ui/service/export",
    filterable: true
},
```
### 4. Alaways use `noRecords`, beecause this will help user to understand that there is no item available. The configuration is as follows:
```javascript
noRecords: true,
messages: {
    noRecords: "No data available on current page."
},
```