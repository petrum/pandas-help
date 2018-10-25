==== Problem 01 ====

I have my input data in a dictionary (of DataFrames):
	
	df = pdr.data.DataReader(['F', 'C'], 'iex', start='2018-10-18', end='2018-10-19')
	
	df
	Out[77]: 
	{'F':               open    high     low   close    volume
	 date                                                
	 2018-10-18  8.5858  8.6054  8.3500  8.3598  59669220
	 2018-10-19  8.1732  8.3746  8.0455  8.3500  59831126,
	 'C':              open     high    low  close    volume
	 date                                              
	 2018-10-18  69.54  70.3949  68.41  68.62  18168520
	 2018-10-19  68.54  69.6000  68.16  68.86  16721947}


I want to convert the dictionary in a single dataframe, and add the key as a new column.
(and I want to do this in a single line)


==== Best solution: using df.assign() ====

	pd.concat([df[s].assign(sym=s) for s in df])
	Out[202]: 
				  open    high     low   close    volume   sym
	date                                                      
	2018-10-22  260.68  261.86  252.59  260.95   5600260  TSLA
	2018-10-23  263.87  297.93  262.10  294.14  19027753  TSLA
	2018-10-22    8.38    8.45    8.27    8.41  39803505     F
	2018-10-23    8.30    8.64    8.23    8.59  55443298     F


==== One liner solution====

	pd.concat([(lambda d, s: pd.DataFrame(s, d.index, ['sym']).join(d))(df[s], s) for s in df])
	Out[127]: 
			   sym     open     high      low    close    volume
	date                                                        
	2018-10-18   F   8.5858   8.6054   8.3500   8.3598  59669220
	2018-10-19   F   8.1732   8.3746   8.0455   8.3500  59831126
	2018-10-18   C  69.5400  70.3949  68.4100  68.6200  18168520
	2018-10-19   C  68.5400  69.6000  68.1600  68.8600  16721947


==== Old solution, using additional function ====

Found the "insert()" method that almost does what I want:

	df['F'].insert(0, 'sym', 'F')

	df['F']
	Out[83]: 
			   sym    open    high     low   close    volume
	date                                                    
	2018-10-18   F  8.5858  8.6054  8.3500  8.3598  59669220
	2018-10-19   F  8.1732  8.3746  8.0455  8.3500  59831126

Unfortunately it always adds the column in place.

The solution is to create a function that calls insert() on a copy():

	def addCol(df, n, v):
		ret = df.copy()
		ret.insert(0, n, v)
		return ret

	addCol(df['F'], 'sym', 'F')
	Out[86]: 
			   sym    open    high     low   close    volume
	date                                                    
	2018-10-18   F  8.5858  8.6054  8.3500  8.3598  59669220
	2018-10-19   F  8.1732  8.3746  8.0455  8.3500  59831126

The result is this:

	pd.concat([addCol(df[s], 'sym', s) for s in df])
	Out[95]: 
			   sym     open     high      low    close    volume
	date                                                        
	2018-10-18   F   8.5858   8.6054   8.3500   8.3598  59669220
	2018-10-19   F   8.1732   8.3746   8.0455   8.3500  59831126
	2018-10-18   C  69.5400  70.3949  68.4100  68.6200  18168520
	2018-10-19   C  68.5400  69.6000  68.1600  68.8600  16721947
