# Intro to Object-Oriented Programming in Ruby

- Define Object-Oriented Programming, and it's benefits
- Define and differentiate between classes and objects
- Create a Ruby a class with an initialize method
- Instantiate an object from a class and interact with it
- Explain the difference between local and instance variables
- Write setter and getter methods
- Describe the use of `attr_reader` / `attr_writer` / `attr_accessor`
- Write methods to define an interface to the class' behaviors
- Describe how OOP supports encapsulation and abstraction

## What is OOP? (5 minutes)

Object-oriented programming is the idea that our programs consist mainly of
objects, which have attributes (aka properties), and methods (aka behavior).

We often think of these objects modeling things in the real world. Examples
might include:

- Bank Account
  - attributes: balance, transactions, owner
  - methods: deposit, withdraw, close_account
- Person
  - attributes: first_name, last_name, hunger_level
  - methods: speak, run, eat
- Squad
  - attributes: name, instructor, students, size
  - methods: add_student, remove_student, meet, high_five, dance

Often in OOP, these objects interact with each other. For example, we might
create a *person* object and then add them to a *squad* object. If we call a
*dance* method on a squad, it might ask make all the people in the squad *dance*
as well.

### Exercise (10 mintues)

Students should model a car, listing attributes and methods it may have.

Example Model: *(don't peek!)*

- Car
  - attributes: make, model, year, color, speed, milage, num_doors, driver
  - methods: turn_on, turn_off, change_gear, accelerate, brake


## Classes vs Objects (5 minutes)

It's very common (in the world and in our programs), that we have many objects
that share the same list of properties, and the same methods. E.g. there are
lots of *people* who each have all the attributes and methods of a *person*.

*Classes* are like blueprints that define the generic list of of properties and
methods, and can be used to build individual *objects* of that class. The
objects each have their own unique values for properties. In Ruby, it might look
something like this:

```ruby
# assuming we have a `Person` class
class Person
  # define properties and methods, more on this later
end

bob   = Person.new("Bob", "Ross", 5)
kanye = Person.new("Kanye", "West",10)

bob.first_name     # returns "Bob"
bob.hunger_level   # returns 5
bob.eat("Banana")  # hunger_level drops to 1
bob.hunger_level   # returns 1

kanye.first_name    # returns "Kanye"
kanye.hunger_level  # returns 10
kanye.eat("Chipotle Burrito")  # hunger_level drops to 0
kanye.hunger_level  # returns 0
```

## Writing Classes

Classes are define with the `class` keyword:

```ruby
class Person
end
```

Pure ruby classes often have an `initialize` method, which gets run when we
create `new` instances of that class.

```ruby
class Person
  def intitialize()
    puts("new person created")
  end
end

bob = Person.new # "new person created"
```

### Instance Variables (10 minutes)

Often, the initialize method is used to set some/all *instance variables* of an
instance. Instance variables are somewhat different, each instance of a class
can have unique values for that variable. Instance variables start with an
at-sign, like so `@first_name`.

```ruby
class Person
  def intitialize(initial_name)
    @name = initial_name
  end

  def introduce
    puts "Hello, I'm #{@name}"
  end
end

me     = Person.new("Adam Bray")
jesse  = Person.new("Jesse")

me.introduce # prints "Hello, I'm Adam Bray"
jesse.introduce # prints "Hello, I'm Jesse Shawl"
```

### Getters and Setters (10 minutes)

We can't *directly* access instance variables of an object. We can *only* call
methods. If we want to access or modify (*get* or *set*) properties of an object,
we need to create methods to do so, often called *getters* and *setters*.

```ruby
class Person
  def intitialize(initial_name, initial_hunger_level)
    @name = initial_name
    @hunger_level = initial_hunger_
  end

  def introduce
    puts "Hello, I'm #{@name}"
  end

  # GETTER
  def first_name
    return @first_name
  end

  def hunger_level
    return @hunger_level
  end

  # SETTERS
  def name=(new_name)
    @name = new_name
  end

  def hunger_level=(new_hunger_level)
    if new_hunger_level < 0
      @hunger_level = 0
    else
      @hunger_level = new_hunger_level
    end
  end

end

me = Person.new("Adam Bray", 10)

# Using Getters
me.name           # returns "Adam Bray"
me.hunger_level   # returns 10

# Using / Testing Setters
me.name = "Adam Bray, Esq." # changes name
me.name                     # returns "Adam Bray, Esq."

me.hunger_level = 5         # changes hunger level
me.hunger_level             # returns 5
me.hunger_level = -8        # changes hunger level, according to rules
me.hunger_level             # returns 0
```

## Exercise

Clone this exercise and follow the instructions in the readme.
[Monkies!!!](https://github.com/adambray/oop_monkey)


### attr_reader, attr_writer, attr_accessor
Since these getters and setters are so common, ruby gives us shortcuts to create
them for us:

* `attr_reader :hunger_level` - creates a getter
* `attr_writer :name` - creates a setter
* `attr_accessor :name` - creates a getter & a setter

These are used inside of a class (customarily at the very top).

```ruby
# functionally identical to the previous example, much less code
class Person
  attr_accessor :name
  attr_reader :hunger_level

  def intitialize(initial_name, initial_hunger_level)
    @name = initial_name
    @hunger_level = initial_hunger_
  end

  def introduce
    puts "Hello, I'm #{@name}"
  end

  # Custom setter for hunger_level
  def hunger_level=(new_hunger_level)
    if new_hunger_level < 0
      @hunger_level = 0
    else
      @hunger_level = new_hunger_level
    end
  end

end
```


## Why OOP? (10 mintues)

#### Easy to Understand

Objects help us build programs that model how we tend to think about the world.
Instead of a bunch of variables and functions (procedural style), we can group
relevant data and functions into objects, and think about them as individual,
self-contained units. This grouping of properties (data) and methods is called
*encapsulation*.

#### Managing Complexity

This is especially important as our programs get more and more complex. We can't
keep all the code (and what it does) in our head at once. Instead, we often want
to think just a portion of the code.

Objects help us organize and think about our programs. If I'm looking at code
for a Squad object, and I see it has associated *people*, and those people can
dance when the squad dances, I don't need to think about or see all the code
related to a person dancing. I can just think at a high level "ok, when a squad
dances, all it's associated people dance". This is a form of *abstraction*... I
don't need to think about the details, just what's happening at a high-level.

#### Ensuring Consistency

One side effect of *encapsulation* (grouping data and methods into objects) is
that these objects can be in control of their data. This usually means ensuring
consistency of their data.

Consider the bank account example... I might define a bank account object
such that you can't directly change it's balance. Instead, you have to use the
`withdrawl` and `deposit` methods. Those methods are the *interface* to the
account, and they can enforce rules for consistency, such as "balance can't be
less than zero".

#### Modularity

If our objects are well-designed, then they interact with each other in
well-defined ways. This allows us to refactor (rewrite) any object, and it
should not impact (cause bugs) in other areas of our programs.

## Lab

Clone this exercise and follow the instructions in the readme.
[Scrabble Word Scorer](https://github.com/ga-dc/scrabbler)
