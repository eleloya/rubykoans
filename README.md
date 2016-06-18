# Neo Ruby Koans Notes

This is my attempt to walk along the path of enlightment. I will post and document my progress here.

## About ASSERTIONS
```ruby
assert true, "message"
assert obj1 == obj2
assert_equal obj1, obj2
assert_equal 2, 1+2
```

## About NIL

```ruby
# nil is also an Object
# only nil responds true to nil?
assert_equal true, nil.is_a?(Object)
assert_equal true, nil.nil?
assert_equal "", nil.to_s?
assert_equal "nil", nil.inspect

# When a nonexistent method is called, a NoMethodError exception arises
object.chupacabras
=> "NoMethodError: undefined method 'chupacabras' for #<Object:0x>"

# Use begin-rescue blocks to catch exceptions
begin
object.method_doesnt_exist
rescue Exception => e
assert_equal NoMethodError, e.class
assert_match /undefined/, e.message
end
```
  
## About OBJECTS

```ruby
# Everything is an object
assert_equal true, 1.is_a?(Object)
assert_equal true, 1.5.is_a?(Object)
assert_equal true, "string".is_a?(Object)
assert_equal true, nil.is_a?(Object)
assert_equal true, Object.is_a?(Object)

# Objects can be transformed to strings.
# Every class implements, optionally, their own to_s. 
# By default, objects print the ObjectClass and the instance id: #<Object:0x123>
assert_equal "123", 123.to_s
assert_equal "", nil.to_s

# Sometimes alias to to_s, sometimes implementation changes. It's spirit is to give an internal string
# representation of the object itself. 
assert_equal "123", 123.inspect
assert_equal "nil", nil.inspect

# Everyobject has a specific id.
# Fixnum goes holds Integers, if it exceeds, converts to BigNum transparently
assert_equal Fixnum, obj.object_id.class

# Every object instance has a different id. Automatically generated on instantiation.
assert_equal false, obj.object_id == another_obj.object_id

# Fixnums, true, false, nil have fixed object_ids on memory.
# (n*2)+1,   20,     0,  8
assert_equal 1, 0.object_id
assert_equal 3, 1.object_id
assert_equal 5, 2.object_id
assert_equal 201, 100.object_id  
assert_equal 20, true.object_id
assert_equal 0, false.object_id
assert_equal 8, nil.object_id

# Objects Clones have different ids
object = Object.new
copy = object.clone
assert_equal false, object           == copy
assert_equal false, object.object_id == copy.object_id
```

## About ARRAYS

```ruby
# Basic stuff
empty_array = Array.new
assert_equal Array, empty_array.class
assert_equal 0, empty_array.size
array = Array.new
array[0] = 1
assert_equal [1], array
array[1] = 2
assert_equal [1, 2], array
array << 333
assert_equal [1,2,333], array
array = [:peanut, :butter, :and, :jelly]
assert_equal :peanut, array[0]
assert_equal :peanut, array.first
assert_equal :jelly, array[3]
assert_equal :jelly, array.last
assert_equal :jelly, array[-1]
assert_equal :butter, array[-3]
array = [:peanut, :butter, :and, :jelly]
# The slicing operator is [index,lenght]. Returns Nil when out of bounds (n+1)
assert_equal [:peanut], array[0,1]
assert_equal [:peanut, :butter], array[0,2]
assert_equal [:and, :jelly], array[2,2]
assert_equal [:and, :jelly], array[2,20]
assert_equal [], array[4,0]
assert_equal [], array[4,100]
assert_equal nil, array[5,0]
# Ranges: with .. is inclusive, with ... is exclusive
assert_equal Range, (1..5).class
assert_not_equal [1,2,3,4,5], (1..5)
assert_equal [1,2,3,4,5], (1..5).to_a
assert_equal [1,2,3,4], (1...5).to_a
# -1 represents the end in an array. Negative numbers is an index from the end to the beginning
array = [:peanut, :butter, :and, :jelly]
assert_equal [:peanut, :butter, :and], array[0..2]
assert_equal [:peanut, :butter], array[0...2]
assert_equal [:and, :jelly], array[2..-1]
# Apparently arrays in Ruby can hold different kind of Objects
array = [1,2]
array.push(:last)
assert_equal [1,2,:last], array{}
popped_value = array.pop
assert_equal :last, popped_value
assert_equal [1,2], array
# Shifting removes the first element. Or adds in the first element.
# Its like pop/push but on the other side of the array. A stack nonetheless
array = [1,2]
array.unshift(:first)
assert_equal [:first,1,2], array
shifted_value = array.shift
assert_equal :first, shifted_value
assert_equal [1,2], array
```

## About ARRAY ASSIGNMENTS
```ruby
names = ["John", "Smith"]
assert_equal ["John", "Smith"], names
# It assigns them  in left-to-right order
first_name, last_name = ["John", "Smith"]
assert_equal "John", first_name
assert_equal "Smith", last_name
# Ignores extra elements
first_name, last_name = ["John", "Smith", "III"]
assert_equal "John", first_name
assert_equal "Smith", last_name
# The splat operator slurps all reaming elements
first_name, *last_name = ["John", "Smith", "III"]
assert_equal "John", first_name
assert_equal ["Smith","III"], last_name
# Out of bounds array assignments results in a nil assignment
first_name, last_name = ["Cher"]
assert_equal "Cher", first_name
assert_equal nil, last_name
# Not surprised
first_name, last_name = [["Willie", "Rae"], "Johnson"]
assert_equal ["Willie", "Rae"], first_name
assert_equal "Johnson", last_name
# Its like using an anymous variable
first_name, = ["John", "Smith"]
assert_equal "John", first_name
# Simple swap using multiple assignments
first_name = "Roy"
last_name = "Rob"
first_name, last_name = last_name, first_name
assert_equal "Rob", first_name
assert_equal "Roy", last_name
  ```

