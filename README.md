d3py
====

This is `d3py`: a plotting library for python based on d3. The aim of d3py is to provide a simple way to plot data from the command line or simple scripts into a browser window.

`d3py` accomplishes this by building on two excellent packages. The first is `d3.js`, which is a javascript library for creating data driven documents, which allows us to place arbitrary svg into a browser window. The second is the `pandas` Python module, which blesses Python with (amongst other things) the DataFrame data structure.

The idioms used to plot data are very simple, and borrow from R's (http://had.co.nz/ggplot2/)[ggplot2] (Hadley Wickham) and Python's (http://matplotlib.sourceforge.net/)[matplotlib] (John Hunter et al):

1. create a Figure object around a `DataFrame`
2. add 'geom's to the figure object to plot specific combinations of columns of the data frame. 

Each geom takes as parameters an appropriate number of column names of the data frame as arguments. For example a line, which has two dimensions, takes an x-value and a y-value. A point, which makes up a scatter plot, has three dimensions and so takes three parameters: x, y and colour (in the future it could take size, too!).

Each geom is styled using css which you can pass in arbitrarily. So, for example, the `Point` geom comes with a bunch of default styles, but you can also specify `fill=red` as a keyword argument which will add a custom css line for that set of points which will turn them red. This means you can style the plot live in the browser using Firebug in Firefox or Chrome's developer tools.

`d3py` aims to create really simple source code wherever possible, so you can go in and edit the plots to embed them into your own sites if needs be. The `.show()` method writes an html file containing the basic markup, a css file with the styles for each geom, a json file with the data from the Figure's DataFrame and a js file with the d3 code in it. The strings that generate the js and css files are always stored in the Figure object so you can see how d3py builds up your graph.

An example session could like:

	import d3py
	import pandas
	# some test data
	T = 100
	# this is a data frame with three columns (we only use 2)
	df = pandas.DataFrame({
	    "time" : range(T),
	    "pressure": np.random.rand(T),
	    "temp" : np.random.rand(T)
	})
	## build up a figure, ggplot2 style
	# instantiate the figure object
	fig = d3py.Figure(df, name="random_temp", width=300, height=300) 
	# add some red points
	fig += d3py.Point(x="pressure", y="temp", fill="red")
	# writes 3 files, starts up a server, then draws some beautiful points in Chrome
	fig.show() 