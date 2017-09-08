# imrsv.data Manuals

This documentation helps you understand how to use imrsv.data to visualise data and it also helps you understand what is going on inside. 

## User Manual

The app was made to be self-explanator, however it might help to get some tips to get you started. 

## Dependencies

This app is entirely dependent on the HoloToolkit  (soon to be renamed to MixedRealityToolkit-Unity) provided by Microsoft. This toolkit is constantly being updated and improved. Therefore, future changes may break the code of this application. As such, careful note needs to be taken when updating to a newer version of the HoloToolkit. 

The rest of the application was entirely self-written - and as such is not dependent on any other library/external code. 

## Developer Manual

This developer manual is intended at developers who want to extend the application or are just curios as to how it works. The application can be easily extended by plugging new classes into the existing infrastructure.

### App Manager

The app manager class is responsible for running the application and coordinating all functions. It is the "controller" in the model-view-controller model. As such, any fundamental changes to the application will need to be done in the app manager class. 

The app manager is also responsible for all things related to the user interface. It has functions that dynamically create the UI buttons from the data currently loaded. 

Finally, the app manager also shows and hides different elements of the application, depending on the state of the application. 

### Data Import
The main data importation functions are parsers that take an input file from a file picker and output a FlatTable data structure which can then be used to visualise data. In order to add a new file option, an additional parser would have to be created. 

Further, new data importation options could be created that stream data via an API directly into the app, which is then parsed and stored in a FlatTable instance.
 
#### CSVToFlatTable Class
The CSV parser is used to handle CSV data. It is based on the [CSV2Table](https://www.assetstore.unity3d.com/en/#!/content/36443) parser by Yoon ChangSik. The parser was adapted to add data into a FlatTable instance on a per column basis. The resulting FlatTable then serves as the output of the parser. 

#### JSONToFlatTable Class
The JSON parser is used to handle JSON data. It is based on the [JSOBObject](https://github.com/mtschoen/JSONObject/blob/master/JSONObject.cs) class by Matt Shoen (used under MIT License). The JSON parser behaves similarly to the CSV parser by adding data to a column, then adding the column to a FlatTable and finally outputting said FlatTable.

### Data Storage

#### Column Class

Individual data is grouped is stored in a column. The column contains two lists: a list to store the actual data (`ColumnData`), and a dictionary that stores the unique values in each column together with a list of indexes (ints) that document their occurences (`DataDistribution`). This DataDistribution field is used for grouping of columns. 

The column instantiator takes a list of objects as an input. This list is then used to build the column by extracting the column name out of the first item of the list, and then adding each of the following item to the `ColumnData`list as well as the `DataDistribution` list. 

The column class acts as a sub-class to the FlatTable class, described below. 

**Attributes:**

| Attribute     | Description     | 
| :------------- |:-------------|
| `Type Dtype`    | Stores the type of the data stored in the column |
| `List<object> ColumnData`      | Stores the actual data containted in the list      | 
| `Single LocalMax` | Stores the highest value in each column (used to facilitate scaling when plotting)|


**Methods and Instantiators:**

| Method    | Description     | 
| :------------- |:-------------|
| `public Column(List<object> values)`    | Instantiates a column from a list of values|
| `private void CheckAndAssignMax(object v)`      | Checks if a value is higher than the local maximum      | 
| `public void Add(object o)` | Adds an object to the list     |
| `public int GetSize()` | Returns the amount of data stored in the column     |
| `override public String ToString()` | Overwrites the ToString() method to make it more readable   |



#### FlatTable Class
The FlatTable is the main data storage facility in this application. It acts as an in-memory flat table data structure consisting of individual data columns. It is not only used to persistently store the data loaded into this application, but also temporarily generated groupings created by the `GroupBy`function. Further, it acts as the input to the plottinf function which uses the value in each cell to creat a data point.

**Attributes:**

| Attribute     | Description     | 
| :------------- |:-------------|
| `Hashtable Index`    | Acts as the index - currently not used but kept for future extensibility |
| `Hashtable Columnlist`      | Stores the names of the columns stored in the FlatTable      | 
| `List<Column> DataColumns` | Stores the columns the FlatTable is made of|
| `Single GlobalMax` | Stores the maximum value contained in the table - used for plotting|
| `Single nrrows` | Stores the number of rows in the table|

**Methods and Instantiators:**

| Method    | Description     | 
| :------------- |:-------------|
| `public FlatTable Read_csv(TextAsset csv)`    | Create a FlatTable from a CSV from a Unity Textasset - calls the CSVToFlatTable class|
| ` public FlatTable Read_csv(string csv)`    | Create a FlatTable from a CSV from a String - calls the CSVToFlatTable class|
| `public FlatTable Read_json(TextAsset json)`    | Create a FlatTable from a JSON from a Unity Textasset - calls the CSVToFlatTable class|
| ` public FlatTable Read_json(string json)`    | Create a FlatTable from a JSON from a String - calls the CSVToFlatTable class|
| `public void AddValue(int colindex, KeyValuePair<object, List<KeyValuePair<object, float>>> val)`      | Adds a specific value to an output | 
| ` public string GetHead(int length)` | Prints the head of the table with a user defined number of rows     |
| ` public string GetTail(int length)` | RPrints the tail of the table with a user defined number of rows     |
| `private string PrintRows(int length, bool reverse)` | Prints the data contained in the table in a user friendly manner  |


### Data Transformation

#### GroupBy function

#### Aggregation functions

### Data display

#### Plotter Function

#### BarChart Function
