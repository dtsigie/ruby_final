#!/usr/bin/ruby

class Vector

  # create accessor methods
  attr :x
  attr :y
  attr :z

  # like a constructor --- create a vector
  def initialize x, y, z 
    @x = x
    @y = y
    @z = z
  end

  # add another vector to this vector
  # result is a new vector --- the sum
  def add v
    Vector.new @x + v.x, @y + v.y, @z + v.z
  end

  # subtract another vector from this vector
  # result is a new vector --- the difference
  def subtract v
    Vector.new @x - v.x, @y - v.y, @z - v.z
  end

  # create a new vector that has the same direction
  # as this vector but a different length
  def scale factor
    Vector.new factor * @x, factor * @y, factor * @z
  end

  # compute the dot product of this vector with
  # another vector
  def dot v 
    @x * v.x + @y * v.y + @z * v.z
  end

  # compute the length (magnitude) of this vector
  def magnitude
    Math.sqrt self.dot self
  end

  # create a new vector that has the same direction
  # as this vector but a length of one
  def normalize
    mag = self.magnitude
    Vector.new @x/mag, @y/mag, @z/mag
  end

  # compute the cross product of this vector with
  # another vector
  def cross v
    x = @y * v.z - @z * v.y
    y = @z * v.x - @x * v.z
    z = @x * v.y - @y * v.x
    Vector.new x, y, z
  end


  # produce a printable representation of this vector
  def to_s
    "(" + x.to_s + "," + y.to_s + "," + z.to_s + ")"
  end

end

class Line

  # create accessor methods
  attr :head
  attr :tail

  # like a constructor --- create a line
  def initialize head, tail
    @head = head
    @tail = tail
  end

  # create a vector that points from
  # the tail to the head of this line
  def vector
    head.subtract tail
  end

  # compute the length of this line
  def length
    self.vector.magnitude
  end

  # produce a vector of length one
  # that points from the tail toward the head
  def direction
    self.vector.normalize
  end

  # create a string that describes this
  # line in the SVG (Scalable Vector Graphics)
  # language
  def svg
    '<line x1= "' + @head.x.to_s + '" ' + 
      'y1= "' + @head.y.to_s + '" ' +
      'x2= "' + @tail.x.to_s + '" ' +
      'y2= "' + @tail.y.to_s + '" ' +
      "\n\t" +
      'style="stroke:rgb(0,0,0);stroke-width:2"/>'    
  end

end

class Polygon 
  # create accessor method 
  attr  :n 
  attr :vertices 
  # like a constructor --- creates an n sided polygon depending on orientation
  def initialize n, topOrBottom
    @n = n
    @vertices = makeVertices topOrBottom
  end

  # makes vertices for polygons depending on their locations on the prism: top, bottom, or sides
  def makeVertices  topOrBottom
    angularIncrement = (2 * Math::PI)/n

    if topOrBottom == "top"
      angle = lambda { |i| i * angularIncrement }#+ angularIncrement/2 }
      x = lambda { |i| Math.cos( angle.call(i) ) }
      y = lambda { |i| Math.sin( angle.call(i) ) }	      
      z = +1
      vertex = lambda { |i| Vector.new( x.call(i), y.call(i), z ) }
      vertices = Array.new
      n.times { |i| vertices << vertex.call(i) }
      vertices	
    
    elsif topOrBottom == "bottom"
      angle = lambda { |i| i * angularIncrement }
      x =  lambda { |i| Math.cos( angle.call(i) ) + 1 }  
      y = lambda { |i| Math.sin( angle.call(i) ) + 3  }      
      z = +1
      vertex = lambda { |i| Vector.new( x.call(i), y.call(i), z ) }
      vertices = Array.new
      n.times { |i| vertices << vertex.call(i) }
      vertices 
      
    else 
      angle = lambda { |i| i * angularIncrement }
      x1 = lambda { |i| Math.cos( angle.call(i) ) }
      y1 = lambda { |i| Math.sin( angle.call(i) ) }	
      x2 =  lambda { |i| Math.cos( angle.call(i) ) + 1 }  
      y2 = lambda { |i| Math.sin( angle.call(i) ) + 3  }      
      z = -1
      
      vertexT = lambda { |i| Vector.new( x1.call(i), y1.call(i), z ) } 
      vertexB =  lambda { |i| Vector.new( x2.call(i), y2.call(i), z) } 
      vertices = Array.new
      n.times { |i| vertices << vertexT.call(i) << vertexB.call(i) }
      vertices

    end    
  
end
  # describe this Polygon in the 
  # SVG (Scalable Vector Graphics) language
  def svg radius
    if self.vertices[0].z == -1
      translation = Vector.new radius, radius, radius
      vert_scaled = @vertices.map { |v| (v.scale radius).add translation }	
      a = vert_scaled.each_slice(2) 
      line_array = Array.new
      a.each { |(x,y)| line_array << (Line.new x, y) }
      line_array << (Line.new vert_scaled.first, vert_scaled.last)

      l_map = line_array.map{ |l| l.svg + "\n" }
      l_map.join(' ')
    else 
      translation = Vector.new radius, radius, radius
      vert_scaled = @vertices.map { |v| (v.scale radius).add translation }	
      a = vert_scaled.each_cons(2) 
      line_array = Array.new
      a.each { |(x,y)| line_array << (Line.new x, y) }
      line_array << (Line.new vert_scaled.first, vert_scaled.last)

      l_map = line_array.map{ |l| l.svg + "\n" }
      l_map.join(' ')
    end

  end

end

# creates an n sided Prism
class Prism
  #create accessor method
  attr :faces
  attr :n
  # create an n sided prism
  def initialize n   

    top = Polygon.new n , "top"
    bottom = Polygon.new n  , "bottom"
    sides = Polygon.new n, "sides"  
    
    @faces = Array.new
    @faces <<  top << bottom << sides

  end
  
  # produce a description of this prism
  # in the HTML (HyperText Markup Language)
  # and SVG (Scaleable Vector Graphics) languages.
  # The result may be displayed with a Web browser.

  def html
    radius = 256 

    edges = String.new
    @faces.each { |t| edges << (t.svg radius/2) << "\n" }


    "<html>" + "\n" +
      "<body>" + "\n" +
      "<h1>A Polyhedron</h1>" + "\n" +

      '<svg xmlns="http://www.w3.org/2000/svg"' + "\n" +
      'version="1.1" height="' + (4 * radius).to_s +
      '" width="' + (4 * radius).to_s + '">' + "\n" +

      edges + "\n" +

      "</svg>" + "\n" + 
      "</body>" + "\n" + 
      "</html>"  
  end

end

# Create a prism with any number of sides

p = Prism.new 15
puts p.html


# Output the HTML and SVG.
# Redirect this output into a file,
# then open the file with a Web browser.





