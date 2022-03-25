# Rule of Thumb!
## When we are using multi-column lists, we will always make sure of the followings:

### 1. `filter` option is available in the multi-column. Filter option allows users to select item from multi-column by searching text. There are various types of filter options, such as: `"startswith"`, `"endswith"`, `"contains"` etc. In our case, filter option should aways be `"contains"`. This will allow user to search text from anywhere in the multi-column item.
### 2. If the multi-column needs to show any data from the database, then the multi-column dataSource should always use kendo `dataSource framework` to pull data. 
**Note:** Previously we used to fetch data using ajax, and then store the data in a global variable and finally create a multi-column and assign the global variable to the multi-column dataSource. This is an ineffecient process mainly because we are writing a lot of code and not leveraging the kendo dataSource framework which is available out of the box. Another disadvanatge is, the multi-column list can't be refreshed.
### 3. Alaways use `noDataTemplate`, beecause this will help user to understand that there is no item available for the multi-column. `footerTemplate` is option to use.
### 4. Never use `optionLabel` in grid and filter editor mode. You can use `optionLabel` in MVVM or Basic multi-column depending on the situation but not mandatory.