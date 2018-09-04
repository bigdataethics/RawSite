+++
description = "Simulation of customer traffic at a coffee shop using SimPy "
title = "Simulation using SimPy"

draft = false
tag = "tutorial"

date = 2018-08-28T22:00:00-05:00

highlight = true
index = true

+++

### Coffee shop scenario

Coffee shops are generally a popular place to hangout. Starbucks, Colectivo, Anodyne, Stone Creek, etc. are some of the sought-after coffee shops in Milwaukee. It is not just the rejuvenating coffee that attracts customers, but the ambience draws people who want to read, work on their laptops, write, do some deep thinking, carry on conversation, or just relax. However, sometimes getting a cup of coffee can take a long wait. Coffee shops try to minimize the wait times so that customers leave satisfied.

In this tutorial we will simulate a simple coffee shop. The purpose of this simulation is to see how the number of cashiers and baristas affect the wait times of customers. We will use Python with some relevant libraries to model this simulation.

When a customer arrives they go to the cashier to place the order and make the payment. The cashier accepts payments through cash or cards. It generally takes 15 to 30 seconds to service a payment by cash and 10 to 20 seconds to service a payment by card. Once the payment is made the item is prepared by the barista. The items on order are: Regular Coffee (10-15), Latte (30-45), Mocha (30-45), Cold Brew (10-20), Frappe (50-70), and Espresso (20-35). The numbers in brackets indicate how many seconds it takes to prepare the item. When the item is ready the customer picks up the order.

### How long will the wait be?

There are two wait periods in the above process. The first is the queue at the cashier and the second is the queue at the barista. The wait times at both the queues depend on the number of cashiers and baristas at work. More the number of cashiers and baristas, shorter will be the wait time. However, economic factors would stipulate that the shop could employ only a certain number of staff. This balance between number of staff and customer wait times is important for customer satisfaction as well as business viability.

### Getting started

Python is a popular general purpose programming language with many libraries to boot. Pandas, NumPy, SciPy, sklearn, random, matplotlib, SimPy are some of the commonly used libraries in data science. In this simulation tutorial we will use random for generating random numbers that correspond to customer traffic, etc., and SimPy to model the simulation. Finally, we will use MatPlotLib to plot the results of the simulation.

The **random** library provides functions for random number generation based on various distributions. The following are some examples:

```
import random
random.seed(9876)      #sets the seed value for the random number generator
random.random()        #returns a random value between 0 and 1
random.uniform(a, b)   #returns a float value between a and b
random.randint(a, b)   #returns an integer between a and b (inclusive of a and b)
```

The Random library also includes generators for triangular, Beta, Exponential, Gamma, Gaussian, Normal, Lognormal and Weibull distributions. You can read more about the random library at https://docs.python.org/3/library/random.html

Instead of random one can also use **SciPy** which is a collection of mathematical algorithms and convenience functions built on the NumPy extension of Python. It also includes a Statistics module that contains a number of random number generators. SciPy documentation related to random number generation can be read at https://docs.scipy.org/doc/scipy/reference/tutorial/stats.html

**SimPy** is a simulation library for Python. It is object-oriented and allows for process based discrete-event simulation. If you are using the standard Python distribution you can install SimPy using `pip` as follows:

```
pip install SimPy
```

SimPy models execute in an <u>environment</u>. Models comprise of processes which correspond to active components like customers in a shop, aircrafts in an airport, vehicles in a parking lot, etc. <u>Processes</u> interact with each other and the environment through events using the `yield` mechanism. Processes get suspended when they yield an event and resume when the event occurs. One of the important event types is the `timeout`. Timeouts are triggered after a user-defined amount of time has passed. This allows the process to sleep or hold its state for the given time. Events make use of <u>resources</u> which correspond to check-out counters in a shop, runways in an airport, parking spots in a lot, etc. Resources may be finite or infinite. 

Processes request resources during an event and release them when they are done. If resources are not available when requested, the process joins a queue and waits for the next available resource. There are two other kinds of resources called level, and store. These are used when we want to keep track of quantities that are being consumed by processes during events. An example for level would be the total amount of gasoline in the gas station. Whenever vehicles fill gas, the level of gasoline decreases. This can be factored in a simulation. A store tracks the production and consumption of items of multiple types. 

Data generated during the simulation can be collected using a `monitor`. You can read more about SimPy at https://simpy.readthedocs.io/en/latest/index.html 

<u>Here are some of the commands that we will be using:</u>

- Import the library: `import simpy`
- Create the environment:  `env = simpy.Environment()`
- Define a resource called cashier (with 2 cashiers): `cashier = simpy.Resource(env, 2)`
- Generate a random number to indicate payment by cash or card (1=cash; 2=card): `payment_type = random.randint(1,2)`
- Yield a timeout event (to prepare the coffee): `yield env.timeout(time_to_prepare)`
- Start the process to generate customers: `env.process(generate_customer(env, cashier, barista))`

### The simulation

We begin by importing the required libraries and initializing variables and constants:

```python
import simpy
import random
import numpy

NO_OF_CUSTOMERS = 20                          
NO_OF_CASHIERS = 2
NO_OF_BARISTAS = 3

menu = {1:["Regular Coffee",10,15], 2:["Latte",30,45], 
        3:["Mocha",30,45], 4:["Cold Brew",10,20], 5:["Frappe",50,70], 
        6:["Espresso",20,35]}
payment_options = {1:["Cash",15,30], 2:["Card",10,20]}

payment_time_list = []     #list to hold the time taken to make payments
order_time_list = []       #list to hold the time taken to prepare the order
```

We are simulating for 20 customers and a coffee shop with 2 cashiers and 3 baristas. `menu` is a dictionary that contains the items that may be ordered and their preparation time ranges.  `payment_options` is a dictionary that contains the options for making the payment and the time range it takes to process the payment.

We create two lists (`payment_time_list` and `order_time_list`) to keep track of the time it takes for each customer to order and to get their order prepared.

This simulation involves two processes. The first is a process to generate customers at a given frequency. The second process is the customer itself with two events: making a payment, and collecting the order. Making the payment involves the cashier resource and collecting the order involves the barista resource.

```python
def generate_customer(env, cashier, barista):
    for i in range(NO_OF_CUSTOMERS):
        yield env.timeout(random.randint(1,20))
        env.process(customer(env, i, cashier, barista))
```

`generate-customer` waits between 1 to 20 seconds before generating a customer.  The `timeout` event uses the `random.randint(1,20)` function call which returns a number between 1 and 20 using the uniform distribution. Once a customer is generated, the `customer` process is called. We will use other distributions to better replicate the arrival times of customers in the next tutorial. 

```Python
def customer(env, name, cashier, barista):
    start = env.now
    print("%s arrived at cashier counter at %.1f" % (name, start))
    with cashier.request() as req:
        start = env.now
        yield req
        menu_item = random.randint(1,6)
        payment_type = random.randint(1,2)
        time_to_order = random.randint(payment_options[payment_type][1], 			payment_options[payment_type][2])
        payment_name = payment_options[payment_type][0]
        yield env.timeout(time_to_order)
        print("> > > %s finished paying by %s in %.1f seconds" % (name, payment_name, env.now-start))
        payment_time_list.append(env.now-start)
    with barista.request() as req:
        yield req
        time_to_prepare = random.randint(menu[menu_item][1], menu[menu_item][2])
        item_name = menu[menu_item][0]
        yield env.timeout(time_to_prepare)
        print(">> >> >> %s served %s in %.1f seconds" % (name, item_name, env.now-start))
        order_time_list.append(env.now-start)
```

The `customer` process first prints the customer number and arrival time. `env.now` returns the current time in seconds since start of the simulation. The `start` variable holds the time the customer arrived at the coffee shop. When the customer arrives, they make a request for the `cashier` resource using `yield req` (note that `req` is defined with the `with` statement). When the `req` is made available (that is, the customer reaches the cashier counter), we randomly choose an item from the menu to order and a payment type. Both these random numbers are drawn using uniform distribution. 

Based on the payment type, we randomly determine the time it takes for the customer to make the payment using the time range we set for each payment type. We `timeout` for that duration and then print how much time since arrival it took to complete making the payment. 

Next a request is made for a `barista` so that the item ordered can be prepared. When the barista is available, a `timeout` is marked while the order is prepared. This random time duration is from the uniform distribution depending on the time range of the particular item ordered. Once the item is prepared, the time taken is printed and the customer gets the item ordered.

Next we define the environment and resources and start the process of generating customers. The environment is run for 300 seconds.

```Python
env = simpy.Environment()
cashier = simpy.Resource(env, NO_OF_CASHIERS)
barista = simpy.Resource(env, NO_OF_BARISTAS)

env.process(generate_customer(env, cashier, barista))
env.run(until=300)
```

Finally, we print the average time it takes for customers to pay and get the order serviced.

```python
print("WITH %s CASHIERS and %s BARISTAS and %s CUSTOMERS..." % (NO_OF_CASHIERS, NO_OF_BARISTAS, NO_OF_CUSTOMERS))
print("It takes %.1f seconds on average to pay for the orders." % (numpy.mean(payment_time_list)))
print("It takes %.1f seconds on average to service the orders." % (numpy.mean(order_time_list)))
```

When this above code was run five times the average payment times for 20 customers were {23.4, 26.4, 35.4, 22.5, 20.6} and the average times to prepare the item were {76.2, 97.1, 70.5, 54.7, 49.2}. So the average of the averages for payment time is 25.6 seconds and for the time to prepare the item is 69.5 seconds. Simulations have to be run multiple times to get a better picture of the times taken.

The above code can be re-run with different values for NO_OF_CASHIERS and NO_OF_BARISTAS to see its impact on wait times.

In the next tutorial we will explore other distributions from which we can sample random customer generation. We will also look at how we can plot the time taken for payments and order processing. 

