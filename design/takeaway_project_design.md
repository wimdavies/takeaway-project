# {{Takeaway}} Multi-Class Planned Design Recipe

## 1. Describe the Problem

> As a customer  
> So that I can check if I want to **order** something  
> I would like to **see a list** of *dishes with prices*.  
>   
> As a customer  
> So that I can **order** the meal I want  
> I would like to be able to **select** some number of several available *dishes*.  
>  
> As a customer  
> So that I can **verify** that my *order* is correct  
> I would like to **see** an *itemised receipt* with a *grand total*.  

## 2. Design the Class System

*Diagrammed on paper*

```ruby
class Dish
  def initialize(name, price)
    # name is a string, price is a float
  end

  def name
    # returns name
  end

  def price
    # returns price
  end
end

class Menu
  def initialize
    # @dishes is an array, initializes as empty
  end
  
  def add(dish) # dish is an instance of Dish
    # stores dish name and price as a hash in @dishes
    # returns nothing
  end

  def list
    # returns @dishes
  end
end

class Order
  def initialize(menu, order_number) 
    # menu is an instance of Menu
    # order_number is a string
    # @items is an array initialized with [order_number]
  end

  def add(dish, quantity) # quantity is an integer
    # extracts dish from @menu's @dishes
    # adds quantity as a key-value pair
    # pushes this hash into @items, without duplicating dish
    # returns nothing
  end

  def list
    # returns @items
  end
end

class OrderSubtotaller
  def initialize(order) # order is an instance of Order
    @subtotalled_order # is an array
  end

  def subtotal(order)
    # multiplies item price by quantity
    # stores this in the item hash
  end

  def list
    # returns @subtotalled_order
  end
end

class Receipt
  def initialize(subtotaller) 
    # subtotaller is an instance of OrderSubtotaller
    @grand_total # is a float (init 0.0)
    @receipt # is an array
  end

  def calculate_grand_total
    # adds subtotals to @grand_total
    # returns nothing
  end

  def checkout(subtotaller)
    # pushes subtotalled order into @receipt
    # appends @grand_total to @receipt
    # returns nothing
  end

  def read
    # returns formatted @receipt
  end
end
```

## 3. Create Examples as Integration Tests

_Create examples of the classes being used together in different situations and
combinations that reflect the ways in which the system will be used._

```ruby
# > As a customer  
# > So that I can check if I want to order something  
# > I would like to see a list of dishes with prices.  

# 1 Menu lists dishes with prices 
my_menu = Menu.new
dish_1 = Dish.new("Burger", 10.5)
dish_2 = Dish.new("Pie", 8.0)
my_menu.add(dish_1)
my_menu.add(dish_2)
my_menu.list # => [
  # {name: "Burger", price: 10.5},
  # {name: "Pie", price: 8.0}
  # ]

# 2 Menu will not allow duplicate dishes to be added
my_menu = Menu.new
dish_1 = Dish.new("Burger", 10.5)
dish_2 = Dish.new("Burger", 10.5)
my_menu.add(dish_1)
my_menu.add(dish_2) # => throws an error

# > As a customer  
# > So that I can order the meal I want  
# > I would like to be able to select some number of several available dishes.  

# 3 Order lists selected dishes
my_menu = Menu.new
dish_1 = Dish.new("Burger", 10.5)
dish_2 = Dish.new("Pie", 8.0)
my_menu.add(dish_1)
my_menu.add(dish_2)
my_order = Order.new(my_menu, "Order #1")
my_order.add(dish_1, 1)
my_order.add(dish_2, 2)
my_order.list # => [
  # "Order #1", 
  # {name: "Burger", price: 10.5, quantity: 1},
  # {name: "Pie", price: 8.0, quantity: 2}
  # ]

# 4 Order increments quantity without duplicating dishes 
my_menu = Menu.new
dish_1 = Dish.new("Burger", 10.5)
dish_2 = Dish.new("Pie", 8.0)
my_menu.add(dish_1)
my_menu.add(dish_2)
my_order = Order.new(my_menu, "Order #1")
my_order.add(dish_1, 1)
my_order.add(dish_2, 2)
my_order.add(dish_1, 1)
my_order.list # => [
  # "Order #1", 
  # {name: "Burger", price: 10.5, quantity: 2},
  # {name: "Pie", price: 8.0, quantity: 2}
  # ]

# 3 Order doesn't add dishes with 0 quantity
my_menu = Menu.new
dish_1 = Dish.new("Burger", 10.5)
dish_2 = Dish.new("Pie", 8.0)
my_menu.add(dish_1)
my_menu.add(dish_2)
my_order = Order.new(my_menu, "Order #1")
my_order.add(dish_1, 0)
my_order.add(dish_2, 1)
my_order.list # => [
  # "Order #1", 
  # {name: "Pie", price: 8.0, quantity: 1}
  # ]



```

## 4. Create Examples as Unit Tests

_Create examples, where appropriate, of the behaviour of each relevant class at
a more granular level of detail._

```ruby
# Menu initializes empty

# Order initializes with order number


```



```ruby
# EXAMPLE

# Constructs a track
track = Track.new("Carte Blanche", "Veracocha")
track.title # => "Carte Blanche"
```

_Encode each example as a test. You can add to the above list as you go._

## 5. Implement the Behaviour

_After each test you write, follow the test-driving process of red, green,
refactor to implement the behaviour._
