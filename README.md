
## rcviz

* Python module to visualize a recursion as a tree with arguments and return values at each node
* Provides a decorator to instrument target functions (as opposed to trace or debugger based approaches)
* Uses pygraphviz to render the graph


## try

This requires graphviz and pygraphviz.

```bash
make
```

## usage

1. Use the @viz decorator to instrument the recursive function.
> @viz <br>
> def factorial(n):

2. Render the recursion with
> callgraph.render("outfile.png")

The output file type is derived from the file name. Supported types include .dot (graphviz dot file), .png (png image), .svg (vector graphic)

## example

```python
from rcviz import callgraph, viz

@viz
def quicksort(items):
    if len(items) <= 1:
        return items
    else:
        pivot = items[0]
        lesser = quicksort([x for x in items[1:] if x < pivot])
        greater = quicksort([x for x in items[1:] if x >= pivot])
        return lesser + [pivot] + greater

print(quicksort(list("helloworld")))
callgraph.render("sort.png")
```

## output
![quicksort rcviz output](http://s30.postimg.org/7chmr6q35/sort.png)

Note:
1. The edges are numbered by the order in which they were traversed by the execution.
2. The edges are colored from black to grey to indicate order of traversal : black edges first, grey edges last.

*Experimental*

Show intermediate values of local variables in the output render by invoking decoratedfunction.track(param1=val1, param2=val2,...). In the quicksort example above you can track the pivot with:

```python
	pivot = items[0]
	quicksort.track(the_pivot=pivot) # shows a new row labelled the_pivot in each node
```

Setup script by [adampetrovic](https://github.com/adampetrovic).

