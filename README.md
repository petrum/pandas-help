## My Pandas summary
* Built on top of ```numpy```
* You get Series (1D), DataFrames (2D), or Panels (+3D)
* The name Pandas is derived from "Panel Data"

### Series
* it's like a fast dictionary (or like a hashmap)
* ctor: from an array (index is the array position)
* ctor: from two arrays with the same size (first has the values, second with labels/index)
* ctor: from scalar and arrays (the scalar has the value, the array has the labels/index)
* ctor: from a dictionary (keys are the label)

### Dataframes
* it's like a colection of ```Series``` sharing the same index
* ctor: from a dictionary of Series (all with same index)
* ctor: from a dictionary of arrays (dictionary of columns; keys are column names, all arrays MUST have the same size; the index is the position)
* ctor: from an array of dictionaries (array of rows; each row is a dictionary); the index is the position in array
  
### Combining data
* ```concat()```: global, combine together vertically multiple datasets (use axis = 1 to do it horizontally, like adding columns)
* ```merge()```: both global and method, it joins on any column
* ```join()```: method, it uses pd.merge() to join index-on-index (you save some typing in some specific cases)

### Modifying data
* ```update()```: method, it modifies in place with values from another table
* ```append()```: method, it adds new rows, and return a df copy
* ```assign()```: method, it adds a new column, and return a copy (yay!)
* ```insert()```: method, it inserts a new column at specified location (always in place)
* ```pop()```: method, it returns a col and it drops it in the same time

### Accesing data, selection
* ```iloc[]```: integer location based indexing
* ```loc[]```: label based indexing
* ```ix[]```: (deprecated) smart indexing; label-based with integer fallback (prefer the two above explicit indexing)
* ```at[]```: label row/col single value access
* ```iat[]```: integer row/col single value access
* ```where()```: method, it returns an object based on cond (df1.where(cond, df2) ~ np.where(cond, df1, df2))
* ```groupby()```: method, it does the split/apply/combine



