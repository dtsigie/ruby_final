Prism - Ruby
Dawit Tsigie
CSC 315
10/19/13

files included: prism.rb 

prism.rb - I created a class called Polygon, based on the triangle class from "vector.rb", which creates a polygon of n sides by using the "makeVertices" method, posted up earlier this week on moodle, which i modified to make vertices of polygons, which are parts of a prism, depending on their orientation: top, bottom, or sides. I also modified the svg method of the triangle class to print out the array of the vertices of the Polygons in svg format. And finally i created a Prism class,based on the Polyhedron class, which creates a prism of n sides. I also modified the border sizes in the html method so that it displays the prism correctly. 

How to run: 

Type in a terminal window: ruby prism.rb > prism.html

Note- the number of sides of the prism can be changed when the program is run. The constructor of the prism class takes in a single value as input argument indicating the number of sides of the prism.



