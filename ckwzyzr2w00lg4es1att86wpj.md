## Simplify your code with Execute Around pattern

The *Execute Around* it's a pattern that lets us simplify our code and make more clean and easy to read.

Basically, the main focus of this approach is to abstract all the code that we want to happen before and after some code in the middle that can change over time.

Here is a couple of examples to better understand this concept.

## Measuring code
From time to time can happen that you need to measure the time a specific code has taken in order to run. For example, you want to know how slow is a specific loop is before returning your results or probably you're more interested in the time spent by a query to get the data stored in the database.

It does not matter **what** code is taking time, what you're looking for is *how much time it took*.

Generally speaking, we can do something like that:
```ruby
start_time = Time.now

# Some code...

end_time = Time.now - start_time
puts "It took #{end_time}"
```
As you can see the code seems pretty simple, well probably because I totally omitted the complex code that you can put between the *measuring code*.

But if you think for a moment about the kind of pattern we are studying right now, the measuring code has **executed around** the main code that is vital for our application. Basically, our application will work just fine without the additional code, but we added it because we want to know how much time a specific code took to execute.

This is a silly example, but try to think all the time this bit of code could be useful.

I am sure you think it's gonna be useful too **but** you should remember how to implement it.

Lucky you the blocks are coming for a rescue because you could create your own method that will be executed around the actual code of your application.
```ruby
def timing_code(desc)
	start_time = Time.now
	
	yield
	
	end_time = Time.now - start_time
	puts "#{desc} took #{end_time}"
end
```
Now with the `timing_code` at our disposal, where we also added a nice description to it, we can simplify our code like so:
```ruby
timing_code('Reverse string') do
	"stressed".reverse
end 
```
Obviously, with a code simple as the above you'll get really small results but still, now you have a method that will let you measure the time spent by any code in your application.
## Reduce duplication of code
The usefulness of the *Execute Around* pattern does not end only when you need to time your code, it gets really useful even to simplify the code you write and abstract annoying parts of your code.

Let's say, for example, that you want to display a message as a result of a check. In this case, the only thing that it's changing in your code is just the conditionals.

Take this code for example:
```ruby
class Sensor
  def temperature
    rand(100..200)
  end

  def level
    rand(1..5)
  end

end

sensor = Sensor.new
puts "Checking water temperature..."
result = sensor.temperature < 150
if result
  puts "OK"
else
  puts "FAILED!"
end

puts "Checking water level..."
result = sensor.level > 3
if result
  puts "OK"
else
  puts "FAILED!"
end
```
We have a `Sensor` class that generate random values for temperature and levels of water, down below you see that we repeat a lot of code:
```ruby
puts "Checking water temperature..."
result = sensor.temperature < 150
if result
  puts "OK"
else
  puts "FAILED!"
end
```
Besides the check that we store in the `result` variable and the string representing the value we will check, all the other code **it's just repeated**, what a waste of time!

Now that we know how blocks work it's time to abstract the repetition and create a method that let us simplify our work:
```ruby
def with_checking(description)
  puts "Checking #{description}..."
  result = yield
  if result
    puts "OK"
  else
    puts "FAILED!"
  end
end
```
We added a method parameter to let the user add its own description but after that, we simply copy/paste the original code with the only exception that the condition stored in `result` is returned by the block the user passes.

Now our code can be much cleaner and any time we want to have a message with *OK!* or *FAILED!* as a result of a condition we can simply write:
```ruby
# Recreating the previous checks
with_checking("temperature") { sensor.temperature < 150 }
with_checking("level") { sensor.level > 3 }

# Custom check for the sake of it
with_checking("it's payday") { Time.now.day > 25 }
```
The last, silly, check I've implemented here it has been added just to demonstrate that you can execute **any condition** you wish inside the block, the important part is that the answer you get will make sense 😉

# Resources

Those notes are taken while following the Ruby Blocks course at Pragmatic Studio.