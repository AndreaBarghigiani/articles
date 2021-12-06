## How to create methods that accept a block

In Ruby there are many methods that accept blocks, one of the first that comes to mind is the `times` method that allows us to run specific block X times. 

Now it's time to learn how to create a method that can accept (and use) a block.

Everything is done by the magic word `yield`.

Define a method that accepts a block is pretty simple, all you have to do is  add `yield` in the body of the method like so:
```ruby
def my_method
	puts "This is the start of my method."
	yield # <- I am waiting for a block here
	puts "This is the end of my method."
end
```
And you're done.

Now you can call your method with a block:
```ruby
my_method do 
	puts "This text comes from the block"
end

# Prints
# This is the start of my method.
# This text comes from the block
# This is the end of my method.
```
Basically the method **stops** what it's doing to let the block take over and run the code we specify in it, but there's more...

First and foremost **you can add `yield` as many times as you want**, doing so the code in the block will just get executed as many times as `yield` is present. So if we change the method in:
```ruby
def my_method
	puts "This is the start of my method."
	yield
	yield
	yield
	puts "This is the end of my method."
end
```
We get:
```ruby
my_method do 
	puts "This text comes from the block"
end

# Prints
# This is the start of my method.
# This text comes from the block
# This text comes from the block
# This text comes from the block
# This is the end of my method.
```

But obviously, there are some gotchas. 

The first one that probably you'll face creating your methods that accept blocks is that **you have to pass a block to it** otherwise you'll get a `LocalJumpError`.

In order to make it conditional, you have to use the method `block_given?`

## Pass a parameter to the block
As it happens for `times` or `upto` you are able to pass parameters to the blocks that can be used within the block itself, you just need to pass it to `yield` as follows:
```ruby
def my_method
	puts "This is the start of my method."
	yield "Andrea"
	puts "This is the end of my method."
end
```
And when we use it we get the following:
```ruby
my_method do |name|
	puts "The name you passed is #{name}"
end

# Prints
# This is the start of my method.
# The name you passed is Andrea
# This is the end of my method.
```
## Return values from a block
Probably the method you're creating needs to do something with the value calculated in the block, so it's time to know how to handle it.

The most important thing to notice is that the *implicit `return`*, the ability of a Ruby method to return the value of the last expression, is available in the blocks as well. 

This means that **your blocks are already returning values**, you just are not using them.

In order to make our method aware of the returned value we just have to change it by storing the value, you guess, in a variable:
```ruby
def my_method
	puts "This is the start of my method."
	result = yield "Andrea"
	puts "This is the end of my method and the result from the block is #{result}."
end
```
Now the `result` holds the value returned from the block, here's an example:
```ruby
my_method do |name|
	puts "The name you passed is #{name}"
	rand(1..5)
end

# Prints
# This is the start of my method.
# The name you passed is Andrea
# This is the end of my method and the result from the block is 4.
```

### Resources
Those notes are taken while following the [Ruby Blocks](https://pragmaticstudio.com/ruby-blocks) course at Pragmatic Studio.