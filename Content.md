# imrsv.data Manuals

This documentation helps you understand how to use imrsv.data to visualise data and it also helps you understand what is going on inside. 

imrsv.data was built from the start to allow the anyone to visualise data in 3-dimensions. As such, we hope that it helps you understand and find patterns in your data. 

## User Manual

The app was made to be self-explanatory, however here are some tips to get you started.

**Important:** The app is only useable if you have a filepicker installed on your Hololens. [OneDrive](https://www.microsoft.com/en-gb/store/p/onedrive/9wzdncrfj1p3?wa=wsignin1.0) is a great choice.

### Accepted File Formats

Missing data values should be left empty. The following file formats are accepted:
- CSV: files using commas as separators will work. Do not use an index. The first line defines the column names and hence needs to consist of strings. [Here](https://1drv.ms/u/s!ArSq-hTe1BlYgQtmPZfea8iSWcj-) is an example.
- JSON: the file needs to be formatted as follows: one key/value pair per data column/dimension containing the column name as the key and an array of data as the value. All dimensions/columns need to have the same length and the values need to be of the same type. [Here](https://1drv.ms/u/s!ArSq-hTe1BlYgQ0YYcdVFaJA97SW) is an example. 

Side note: Files exported using the pandas [`pandas.DataFrame.to_json`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.to_json.html) method without any parameters should work fine. 

### Application Start
The application welcomes you with a screen that prompts you to load a file. Upon pressing the load file button, the app will switch to the file picker installed on your device. Choose the file you want to load and press "open".

### Placing The Graph

As soon as you select a file you will be able to place the graph anywhere in the room. The graph will then appear at that exact location. Placement will look like this:

![UI](https://github.com/lucasvaltl/imrsv.data-documentation/raw/master/TapToPlaceScreenshot.jpg?raw=true "UI")

### Visualising Data

As soon as you place the graph, your data will be plotted. It may look like this:

![UI](https://github.com/lucasvaltl/imrsv.data-documentation/raw/master/GraphScreenshot.png?raw=true "UI")

### Data Manipulation

You can now manipulate the dimensions being displayed using the UI that was created based on the headers of the data you loaded. The UI is made of three main sections:

![UI](https://github.com/lucasvaltl/imrsv.data-documentation/raw/master/UIScreenshot.png?raw=true "UI")

- The leftmost section controls how your data is grouped. The data will be grouped along this dimension - think of it like the x axis of your graph.
- The middle section allows you to add a secondary / sub-grouping category. Each group created will be sub-grouped using this dimension. Sub-groups are displayed as tip-tools when gazing at a data point. 
- The rightmost section allows you to select the dimensions that are displayed. You can choose which dimensions are displayed and how they are aggregated by clicking the `Count`, `Sum` and `Avg` buttons. Pressing any button visualises the corresponding dimension, and pressing it again deletes said dimension from the visualisation. 

Use these buttons to change how and which data is being displayed - then walk around the graph to see it from different angles. This will hopefully allow you to understand the data better and find new patterns between different dimensions.

### Changing The Data Loaded

To change the data you are visualising, just press the `Load Data` button at the top of the UI panel. The app will switch to your file picker and you can select a different data set to visualise. 


## Deployment Manual

### Application Building / Deployment

At the time of writing, the application is not yet available in the Windows App Store. Therefore, the application needs to be built and deployed on the Hololens device manually. To do so you will need to install the following development tools:

- Unity: For development version [2017.1p5](https://unity3d.com/unity/qa/patch-releases/2017.1.0p5) was used
- Visual Studio: For development version 15.3.3 was used

In order to build the app and run it on the Hololens, the following steps need to be followed:

1. First, the Unity project needs to be loaded using Unity. The Unity version used for this project was [Unity 2017.1p5](https://unity3d.com/unity/qa/patch-releases/2017.1.0p5). Open the project using the "open project" function in Unity and navigate to the folder where it is stored. 
2. Then the application needs to be built and deployed to the device. Just follow the steps outlined [here](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_100#compile_the_visual_studio_solution). Make sure Unity is set up correctly as detailed by the guide.
 

## Dependencies

This app is dependent on the HoloToolkit  (soon to be renamed to MixedRealityToolkit-Unity) provided by Microsoft. This toolkit is constantly being updated and improved. Therefore, future changes may break this application. As such, careful note needs to be taken when updating to a newer version of the HoloToolkit. The version of the toolkit used for this project is `v1.2017.1.0`. The newest version can be found [here](https://github.com/Microsoft/MixedRealityToolkit-Unity/releases). 

Other dependencies include:
- The JSON parser is based on the [JSOBObject](https://github.com/mtschoen/JSONObject/blob/master/JSONObject.cs) class by Matt Shoen
- The CSV parser is based on the [CSV2Table](https://www.assetstore.unity3d.com/en/#!/content/36443) parser by Yoon ChangSik

The rest of the application was entirely self-written - and as such is not dependent on any other library/external code. 

## Documentation

This documentation is intended dor developers who want to extend the application or are just curious as to how it works. The application can be easily extended by plugging new classes into the existing infrastructure in order to extend the app's functionalities. 

### App Manager

The app manager class is responsible for running the application and coordinating all functions. It is the "controller" in the model-view-controller model. As such, any fundamental changes to the application will need to be done in the app manager class. 

The app manager is also responsible for all things related to the user interface. It has functions that dynamically create the UI buttons from the data currently loaded. 

Finally, the app manager also shows and hides different elements of the application, depending on the state of the application. 

### Data Import
The main data importation functions are parsers that take an input file from a file picker and output a FlatTable data structure which can then be used to visualise data. In order to add a new file option, an additional parser would have to be created. 

Further, new data importation options could be created that stream data via an API directly into the app, which is then parsed and stored in a FlatTable instance.
 
#### CSVToFlatTable Class
The CSV parser is used to handle CSV data. It is based on the [CSV2Table](https://www.assetstore.unity3d.com/en/#!/content/36443) parser by Yoon ChangSik. The parser was adapted to add data into a FlatTable instance on a per column basis. The resulting FlatTable then serves as the output of the parser. 

**Methods:**

| Method    | Description     | 
| :------------- |:-------------|
| `public FlatTable Parse(TextAsset csv)`    | Parses a CSV Unity textasset and outputs a FlatTable - used in the Unity Editor|
| `public FlatTable Parse(string csv)`    | Parses a CSV string and outputs a FlatTable - used in the Unity Editor|


#### JSONToFlatTable Class
The JSON parser is used to handle JSON data. It is based on the [JSOBObject](https://github.com/mtschoen/JSONObject/blob/master/JSONObject.cs) class by Matt Shoen (used under MIT License). The JSON parser behaves similarly to the CSV parser by adding data to a column, adding each column to a FlatTable and finally outputting said FlatTable.

**Methods:**

| Method    | Description     | 
| :------------- |:-------------|
| `public FlatTable Parse(TextAsset json)`    | Parses a JSON Unity textasset and outputs a FlatTable - used in the Unity Editor|
| `public FlatTable Parse(string json)`    | Parses a JSON string and outputs a FlatTable - used in the Unity Editor|

### Data Storage

#### Column Class

Individual data is grouped is stored in a column. The column contains two lists: a list to store the actual data (`ColumnData`), and a dictionary that stores the unique values in each column together with a list of indexes (ints) that document their occurence in the data (`DataDistribution`). This DataDistribution field is used for grouping of columns. 

The column instantiator takes a list of objects as an input. This list is then used to build the column by extracting the column name out of the first item of the list, and then adding each of the following items to the `ColumnData`list as well as the `DataDistribution` list. 

The column class acts as a sub-class of the FlatTable class, described below. 

**Attributes:**

| Attribute     | Description     | 
| :------------- |:-------------|
| `Type Dtype`    | Stores the type of the data stored in the column |
| `List<object> ColumnData`      | Stores the actual data contained in the list      | 
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
The FlatTable is the main data storage facility in this application. It acts as an in-memory flat table data structure consisting of individual data columns. It is not only used to persistently store the data loaded into this application, but also temporarily generated groupings created by the `GroupBy`function. Further, it acts as the input to the plotting function which uses the value in each cell to create a data point.

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


#### Grouper Class
The static `Grouper`class is the main way this application transforms data. It contains only one method: `GroupBy`. The main use case of this method is similar to a pivot table: it groups a table along a user defined dimension. The method also allows for sub-grouping each group of data. The main output is a `FlatTable` file, where each cell holds a key value pair: The key is the main data value of the group, and the value is a list of key value pairs that hold the names and values of the subgroups. When the main grouping column consists of a format parseable as `DateTime`, the rows are ordered by ascendingly by time. 

**Methods and Instantiators:**

| Method    | Description     | 
| :------------- |:-------------|
| `public static FlatTable GroupBy(GroupingColumns grpcol, FlatTable input)`    | This method takes a grouping column instance (which holds the grouping dimensions) as an input and outputs a FlatTable|
| `private static List<KeyValuePair<object, float>> SecondaryGrouping(AggregateFunction aggfunc, List<int> uniqeindexesprim, Dictionary<object, List<int>> datadistributionsecgrouping, Column currentcol)`    | Performs the secondary grouping of the data, based on the primary grouping|


#### GroupingColumns

This class acts as a mediator between the user interface and the GroupBy class. It holds the dimensions on which the user wishes to group the data as well as how he wishes to aggregate: by count, sum or average.

**Attributes:**

| Attribute     | Description     | 
| :------------- |:-------------|
| `string PrimaryGrouping;`    | Primary grouping column |
| `string SecGrouping`      | Secondary grouping column      | 
| `List<KeyValuePair<string, string>> SelectedColumns` | Stores the  columns to be displayed (key) along with information on how each column is to be aggregated (value)|

**Methods and Instantiators:**

| Method    | Description     | 
| :------------- |:-------------|
| ` public GroupingColumns(string prim, string sec, List<KeyValuePair<string, string>> selcols)`    | Instantiator|
| ` public string ColumIsSelected(string input)`    | Used to check if a specific column is selected - returns null if false |
| ` public void RemoveSelectedCol(string input)`    | Removes a column from the selection |

#### Aggregation Class

This class handles the aggregation of values during grouping. Based on the strategy pattern, it acts as an abstract class which is used to create different aggregation algorithms. The current aggregation functions are:

- Sum
- Count
- Average

These simply take a list as an input and give an int as an output and are omited here for brevity.

### Data display

#### Plotter Class

The `Plotter` class is an abstract class that acts as a template for different plotting implementations. 

#### BarChart Function

The `BarChart` class is responsible for plotting the data grouped by the `GroupBy` method. It requires several Unity prefabs as inputs:

- A prefab that holds the 3-dimensional representation of a bar as well as the tip tool used to show its data value and information on the sub-groups contained in it
- A prefab that holds the text used to label a group of data
- A prefab that holds the text used to label a dimension of data

These prefabs are cloned for each data point and their size / position is then adjusted according to the value that it aims to visualise. The positioning is especially complicated as the entire graph is placeable at runtime.  The objects need to be rotated along 3 axes as well as placed in the 3-dimensional grid correctly. As such, the graph placement is done by calculating the data points/labels position relative to the parent object using vector arithmetic. Further, sub-groupings are also added to the text that is displayed in the tip tool of each data point. 

**Methods and Instantiators:**

| Method    | Description     | 
| :------------- |:-------------|
| ` public override void Plot(FlatTable input, Transform barprefab, Transform parentobject, Transform plane, List<Color32> colors, RectTransform grouplabel, RectTransform dimensionalabel)`    | Plots a three dimensional bar chart on top of a plane. Requires several unity prefabs which it the clones to create the graph: a barprefab, a grouplabel prefab and a dimensionlabel prefab. Bar colors are taken from the colors input.|
| ` private void CreateGroupLabel(string text, Transform labelprefab, Vector3 curpos, Quaternion currot, Transform parentobject, Transform plane, Quaternion parentrotation)`    | Creates and places a group label|
| `private void CreateDimensionLabel(string text, Transform labelprefab, Vector3 curpos, Quaternion currot, Transform parentobject, Transform plane, Quaternion parentrotation)`    | Creates and places a dimension label |



## Final Words

Thanks for reading through this entire document - or did you just skip to the end? Nonetheless, I hope this was of help. If you have any questions, don't hesitate to contact me. 
