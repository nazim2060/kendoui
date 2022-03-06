# Rule of Thumb!
## When we are using dropdown lists, we will always make sure of the followings:

### 1. `filter` option is available in the dropdown. Filter option allows users to select item from dropdown by searching text. There are various types of filter options, such as: `"startswith"`, `"endswith"`, `"contains"` etc. In our case, filter option should aways be `"contains"`. This will allow user to search text from anywhere in the dropdown item.
### 2. If the dropdown needs to show any data from the database, then the dropdown dataSource should always use kendo `dataSource framework` to pull data. 
**Note:** Previously we used to fetch data using ajax, and then store the data in a global variable and finally create a dropdown and assign the global variable to the dropdown dataSource. This is an ineffecient process mainly because we are writing a lot of code and not leveraging the kendo dataSource framework which is available out of the box. Another disadvanatge is, the dropdown list can't be refreshed.
### 3. Alaways use `noDataTemplate`, beecause this will help user to understand that there is no item available for the dropdown. `footerTemplate` is option to use.
### 4. Never use `optionLabel` in grid and filter editor mode. You can use `optionLabel` in MVVM or Basic dropdown depending on the situation but not mandatory.