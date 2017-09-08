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
 
#### CSVToFlatTable
The CSV parser is used to handle CSV data. It is based on the [CSV2Table](https://www.assetstore.unity3d.com/en/#!/content/36443) parser by Yoon ChangSik. The parser was adapted to add data into a FlatTable instance on a per column basis. The resulting FlatTable then serves as the output of the parser. 

#### JSONToFlatTable
The JSON parser is used to handle JSON data. It is based on the [JSOBObject](https://github.com/mtschoen/JSONObject/blob/master/JSONObject.cs) class by Matt Shoen (used under MIT License). The JSON parser behaves similarly to the CSV parser by adding data to a column, then adding the column to a FlatTable and finally outputting said FlatTable.

### Data Storage

#### Column Class

Individual data is grouped is stored in a column. Each column contains the following variables.

Attributes:

```c#
Type Dtype;
List<object> ColumnData;
Dictionary<object, List<int>> DataDistribution;
String Name;
Single LocalMax;
```
| Attribute     | Description     | 
| :-------------: |:-------------:|
| `Type Dtype`    | Stores the type of the data stored in the column |
| `List<object> ColumnData`      | Stores the actual data containted in the list      | 
| `Single LocalMax` | Stores the highest value in each column (used to facilitate scaling when plotting)     |


Methods:

```c#
public Column(List<object> values){} 
private void CheckAndAssignMax(object v){}
public void Add(object o){}
public int GetSize(){}
override public String ToString(){}  
```


The column contains two lists: a list to store the actual data (`ColumnData`), and a dictionary that stores the unique values in each column together with a list of indexes (ints) that document their occurences (`DataDistribution`). This DataDistribution field is used for grouping of columns. 

The column instantiator takes a list of objects as an input. This list is then used to build the column by extracting the column name out of the first item of the list, and then adding each of the following item to the `ColumnData`list as well as the `DataDistribution` list. 

The column class acts as a sub-class to the FlatTable class, described below. 



#### FlatTable Class



### Data Transformation

#### GroupBy function

#### Aggregation functions

### Data display

#### Plotter Function

#### BarChart Function
