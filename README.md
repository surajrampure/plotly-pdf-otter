# plotly-pdf-otter

High-level overview:

Student creates `plotly` visualization and saves it to a variable -> notebook saves a static version of student's visualization to disk and displays it again in the notebook -> notebook-to-PDF routine extracts this displayed version into the necessary PDF

See `plotly-pdf-example.ipynb` for an example of this in practice.

1. Add `!pip install kaleido` to the top of your notebook in a cell of its own.
- `kaleido` helps generate static images of interactive visualizations. If you get `kaleido` installed on the hub, this step is unnecessary. (This was a last-minute fix for us so we didn't get a chance to get it installed on the hub.)
- If you don't put this line in a cell on its own, the Otter autograder on Gradescope will cause an error.

2. Somewhere at the top of your assignment notebook (e.g. in the same cell with all of your other imports), define `save_and_show` as shown below.

```py
from IPython.display import display, Image # necessary as well
def save_and_show(fig, path):
    if fig == None:
        print('Error: Make sure to use the argument show = False above.')
    plotly.io.write_image(fig, path)
    display(Image(path))
```

  `save_and_show` takes in a `plotly` graph and file path, saves a static version of the graph to that location, and displays it back in the notebook using `IPython.display`. This is not the most elegant solution, but it works.

3. Make sure that students assign their `plotly` graphs to a variable in each question that they are creating a graph. In Data 94, we gave students the following instructions:

>>> When creating graphs in this assignment, there are two things you will always have to do that we didn't talk about in lecture:
1. Assign the graph to a variable name. (As in previous assignments, we will tell you which variable names to use.)
2. Use the argument `show = False` in your graphing method, in addition to the other arguments you want to use.
  These steps are **required** in order for your work to be graded. The distinction is subtle, since you will see the same visual output either way. See below for an example.
  <b style="color:green;">Good:</b>
  ```py
  fig_5 = table.sort('other_column').barh('column_name', show = False)
  ```
  <b style="color:red;">Bad:</b>
  ```py
  table.sort('other_column').barh('column_name')
  ```

  The instructions provided above only really work for the `datascience` library; Data 100 folks will have to do something slightly different (I don't believe `show = False` is a vanilla `plotly` thing; I think by default regular `plotly` graphs return the graph object.)

  **Do not** demarkate the cell in which students write their visualization code as a question in Otter/`okpy`, as is normally done. Instead, follow the instructions below.

4. In a text cell that comes immediately after the code cell where students create and assign their visualization to a variable, add the Otter metadata that specifies this is a written question (or the equivalent `okpy` metadata). Also add some text specifying that the cell that proceeds this one needs to be run in order for their work to be saved.

5. In a code cell after the cell above (so two after the one in which students write code), write something similar to the following:

```py
# Run this cell, don't change anything.
save_and_show(fig_1c, 'images/saved/1c.png')
```

  Here, `fig_1c` is the variable name that we gave to students in the question (so they wrote `fig_1c = <some plotly graph>`). The path is one we arbitrarily chose, but you need to make sure that `images/saved` exists before trying this. After students run this cell, they will see a static version of their `plotly` graph, and the notebook-to-PDF routine will extract this static version for grading.

Reach out with any questions about this procedure!
