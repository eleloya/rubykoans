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
  
## About HASHES

```ruby
empty_hash = Hash.new
assert_equal Hash, empty_hash.class
assert_equal({}, empty_hash)
assert_equal 0, empty_hash.size
hash = { :one => "uno", :two => "dos" }
assert_equal 2, hash.size
# returns nil on noexistent elements
hash = { :one => "uno", :two => "dos" }
assert_equal "uno", hash[:one]
assert_equal "dos", hash[:two]
assert_equal nil, hash[:doesnt_exist]
# raises keyError when you try to fetch an nonexisten element
hash = { :one => "uno" }
assert_equal "uno", hash.fetch(:one)
assert_raise(KeyError) do
  hash.fetch(:doesnt_exist)
end
hash = { :one => "uno", :two => "dos" }
hash[:one] = "eins"
expected = { :one => "eins", :two => "dos" }
assert_equal expected, hash
# A hash has no order
hash1 = { :one => "uno", :two => "dos" }
hash2 = { :two => "dos", :one => "uno" }
assert_equal true, hash1 == hash2
hash = { :one => "uno", :two => "dos" }
assert_equal 2, hash.keys.size
assert_equal true, hash.keys.include?(:one)
assert_equal true, hash.keys.include?(:two)
assert_equal Array, hash.keys.class
hash = { :one => "uno", :two => "dos" }
assert_equal 2, hash.values.size
assert_equal true, hash.values.include?("uno")
assert_equal true, hash.values.include?("dos")
assert_equal Array, hash.values.class
# Merge adds and overrides
hash = { "jim" => 53, "amy" => 20, "dan" => 23 }
new_hash = hash.merge({ "jim" => 54, "jenny" => 26 })
assert_equal true, hash != new_hash
expected = { "jim" => 54, "amy" => 20, "dan" => 23, "jenny" => 26 }
assert_equal true, expected == new_hash
# Returns nil on nonexistent elements, as I said before.
hash1 = Hash.new
hash1[:one] = 1
assert_equal 1, hash1[:one]
assert_equal nil, hash1[:two]
# Returns object declared in instantiation on nonexistent elements.
hash2 = Hash.new("dos")
hash2[:one] = 1
assert_equal 1, hash2[:one]
assert_equal "dos", hash2[:two]
# That objects remains the same and is possible to alter it.
hash = Hash.new([])
hash[:one] << "uno"
hash[:two] << "dos"
assert_equal ["uno", "dos"], hash[:one]
assert_equal ["uno", "dos"], hash[:two]
assert_equal ["uno", "dos"], hash[:three]
assert_equal true, hash[:one].object_id == hash[:two].object_id
# A block gives you a new object everytime a nonexistent key is called
hash = Hash.new {|hash, key| hash[key] = [] }
hash[:one] << "uno"
hash[:two] << "dos"
assert_equal ["uno"], hash[:one]
assert_equal ["dos"], hash[:two]
assert_equal [], hash[:three]
```

## About STRINGS

```ruby
#double quoated literals are strings
string = "Hello, World"
assert_equal true, string.is_a?(String)
# single quote literals are strings
string = 'Goodbye, World'
assert_equal true, string.is_a?(String)

# single quote strings can use double-quotes inside.
string = 'He said, "Go Away."'
assert_equal "He said, \"Go Away.\"", string

# double quote strings can use single-quotes inside
string = "Don't"
assert_equal 'Don\'t', string
# You can also always escape them when neccesary
a = "He said, \"Don't\""
b = 'He said, "Don\'t"'
assert_equal true, a == b
#FLexible quotes help with hard-cases
a = %(flexible quotes can handle both ' and " characters)
b = %!flexible quotes can handle both ' and " characters!
c = %{flexible quotes can handle both ' and " characters}
assert_equal true, a == b
assert_equal true, a == c
# Flexible quotes also handle multiple-line strings
long_string = %{
It was the best of times,
It was the worst of times.
}
#The problem is it adds an unncesary linebreak at the beginning
assert_equal 54, long_string.length
assert_equal 3, long_string.lines.count
assert_equal "\n", long_string[0,1]
# EOS to the resque. It doesnt add the initial linebreak
long_string = <<EOS
It was the best of times,
It was the worst of times.
EOS
assert_equal 53, long_string.length
assert_equal 2, long_string.lines.count
assert_equal "I", long_string[0,1]

# Plus concatenates a string
string = "Hello, " + "World"
assert_equal "Hello, World", string

# Plus sign concat doesn't modify the oriignal
hi = "Hello, "
there = "World"
string = hi + there
assert_equal "Hello, ", hi
assert_equal "World", there
hi = "Hello, "
there = "World"
hi += there
assert_equal "Hello, World", hi
original_string = "Hello, "
hi = original_string
there = "World"
hi += there
assert_equal "Hello, ", original_string
# The shovel operator is the only one that alters the original string in memory
hi = "Hello, "
there = "World"
hi << there
assert_equal "Hello, World", hi
assert_equal "World", there
original_string = "Hello, "
hi = original_string
there = "World"
hi << there
assert_equal hi, original_string
string = "\n"
assert_equal 1, string.size
# Single quotes dont escape characters
string = '\n'
assert_equal 2, string.size
# Well, sometimes?, apparently it escapes the single-quote
string = '\\\''
assert_equal 2, string.size
assert_equal "\\'", string
# Use #{var} to interpolate a string
value = 123
string = "The value is #{value}"
assert_equal "The value is 123", string
# Single-quote strings can't handle interpolation
value = 123
string = 'The value is #{value}'
assert_equal "The value is \#{value}", string
string = "The square root of 5 is #{Math.sqrt(5)}"
assert_equal "The square root of 5 is " << Math.sqrt(5).to_s, string
# Substring slicing. 
string = "Bacon, lettuce and tomato"
assert_equal "let", string[7,3]
assert_equal "let", string[7..9]

string = "Bacon, lettuce and tomato"
assert_equal "a", string[1]

# split a string using whitespace as a default delimiter
string = "Sausage Egg Cheese"
words = string.split
assert_equal ["Sausage", "Egg", "Cheese"], words
# split also accepts a different delimiter
string = "the:rain:in:spain"
words = string.split(/:/)
assert_equal ["the", "rain", "in", "spain"], words
# join them :)
words = ["Now", "is", "the", "time"]
assert_equal "Now is the time", words.join(" ")
# strings are unique objects even if they say the same
a = "a string"
b = "a string"
assert_equal true, a           == b
assert_equal false, a.object_id == b.object_id
```