---
layout: page
title: Programming is Planning
---

A common misconception among beginner programmers is that programming is all about writing code. I would argue that less than half of programming requires sitting in front of a computer.

Programming is, at its core, about solving problems. Often the end goal is indeed a computer program which solves a problem, and it is true that such a program must be written in code. But the typing of the code isn't where most of the work is put in. Rather, the hard part about solving a problem is designing a solution.

To explain what I mean, I'm going to write a program to meet a certain set of specifications. Pay attention to the planning which goes on. The code doesn't require as much effort as the planning.

## Specification

(This specification is based on UW Bothell CSS 142C Spring 2021 Homework 3. The course to this point has not covered arrays or objects yet so I won't be using those.)

A T-shirt company needs a program to calculate the price of an order. Here are the company's pricing policies:
- The unit price for a custom T-shirt is determined based on the customer's design
- To encourage bulk purchases, each T-shirt after the first costs 1% less than the T-shirt before it
- There's a promotion going on. The customer chooses exactly one of these special deals:
    - A flat $100 discount
    - A 5% discount
    - T-shirts get cheaper twice as fast (so the 1% becomes a 2%)

(Shirts must be ordered in bulk, so the price will be positive even after the $100 discount.)

The company wants customers to be able to use the program so they can see the order price before they checkout. So the program must operate as follows:
1. Ask for the customer's name and save it
2. Display a greeting which uses the customer's name
3. Display a menu with these options:
    - Enter the unit price of the T-shirts
    - Enter the number of T-shirts you wish to order
    - Choose a promotional offer
    - Display the total price of the order
    - Exit the program
4. Depending on the option, prompt the user for input or display the cost of the order or exit
5. Go to step 3

The program need not validate input. Assume the user will always give valid input.

The customer should be able to do the top three tasks in any order. If they try to display the total price before entering the design price, number, and bonus, the program should display an error message but continue running.

## Planning

The specification is pretty complex. I won't even attempt to fit all of it into my head at once.

First I try to figure out the overarching flow of the program. In familiar terms, it's what order the program performs its tasks in, without any details of what goes on in those tasks. This specification gives the flow in the numbered list.

### Pseudocode

I'm gonna take a quick detour and talk about pseudocode.

The point of pseudocode is to be able to describe an algorithm or a program's structure without having to worry about language specifics. So it has no rigid syntax.

Nor does it use specific classes such as `Scanner`. Instead of `Scanner`, if we wanted to get input from the user, we could just write
```java
variable userInput = getInputString()
```
and not worry about what the guts of `getInputString` look like.

The idea that you don't need to know *how* a method works in order to use it is critical here. Humans are extremely bad at thinking about just two things at once, and programs can contain thousands of methods. It's flat out impossible to think about a complex program all at once. Instead, when designing or writing code, we should be thinking only about the task in front of us. And if we treat other methods as magically doing what they say they do, we can avoid thinking about more than just our own task.

Pseudocode does two things for us:
- We don't have to worry about syntax errors or runtime errors. We only have to think about the logic of the program.
- We don't have to implement things in any order. We can leave a method to be implemented later, or just never implement it (at least until we go to write the real code).

### Customer name

The first part of the flow is dealing with the customer's name.

I'm going to delegate the task of asking for the customer's name and handling input. I'll call that method `getCustomerName` since it gets the customer name. It requires no parameters. I want this method to send me back the customer's name, so I'll make it return that.

Then I'm going to delegate the task of displaying the greeting. I'll call that `displayGreeting`. That task needs to know what the customer's name is, so it has one parameter: the customer's name. It returns nothing, since I don't need to get any data from it.

So far my pseudocode looks like this:
```java
method main()
{
    variable customerName = getCustomerName()
    displayGreeting(customerName)
    ...
}
```

### Main menu

Reading ahead a little bit, I notice that the main menu code needs to be called repeatedly. In fact, it should continue to be called until the program exits. So I'll use an infinite loop here:
```java
while (true)
{
    run main menu code
}
```

As for what actually goes on in the main menu code, things start to get a little tricky. I would love to delegate the entire main menu to a separate method `runMainMenu`. But I need to get information back from the main menu in order to calculate the total price. By the way `return` works, I can only send one piece of information back from the main menu, meaning I can't send both the option selected and the data entered. So I have two options for getting and categorizing this data:
- Put a bunch of the main menu code into the `main` method
- Use global variables

Neither of these options is very appealing. I'm choosing to use global variables, but either will work.

Now that I've made this decision, the pseudocode looks like this:
```java
method main()
{
    variable customerName = getCustomerName()
    displayGreeting(customerName)

    while (true)
    {
        runMainMenu()
    }
}
```

This concludes the planning of the program's overarching flow.

## Methods

Now I can implement all the methods which I laid out above. Remember, I don't have to think about how these little methods are called. The point of the planning above was to allow me to turn my focus to these tasks.

### getCustomerName

All that must be done is
- Prompt the user for input
- Return the input

The pseudocode is therefore
```java
method getCustomerName()
{
    print("Please enter your name:")
    return getInputString()
}
```

### displayGreeting

Very simply,
```java
method displayGreeting(String customerName)
{
    print("Hello, " + customerName)
}
```

### runMainMenu

Much like the `main` method, I don't want this method to be doing anything too complex. I'll just have it organize other subtasks for the most part:
- Display the options
- Get user input
- Call the corresponding helper method

Then the pseudocode will be
```java
method runMainMenu()
{
    displayOptions()
    print("Please enter your choice:")
    variable userChoice = getInputInt()

    if (userChoice == 1)
    {
        handleUnitPrice()
    }
    else if (userChoice == 2)
    {
        handleNumberOfShirts()
    }
    else if (userChoice == 3)
    {
        handlePromotionalOffer()
    }
    else if (userChoice == 4)
    {
        displayTotalPrice()
    }
    else if (userChoice == 5)
    {
        exit()
    }
}
```

This introduces a bunch of new methods. (I'll assume `exit` is already implemented.)

### displayOptions

Straightforward, does what it says:
```java
method displayOptions()
{
    print("1. Enter the unit price of the T-shirts")
    print("2. Enter the number of T-shirts you wish to order")
    print("3. Choose a promotional offer")
    print("4. Display the total price of the order")
    print("5. Exit the program")
}
```

### handleUnitPrice

Suppose I have a global variable `unitPrice`. Then all this has to do is prompt the customer for a unit price and save it to `unitPrice`:
```java
global variable unitPrice

method handleUnitPrice()
{
    print("Enter the unit price of the T-shirts:")
    unitPrice = getInputDouble()
}
```

### handleNumberOfShirts

Suppose I have a global variable `numberofShirts`. Then all this has to do is prompt the customer for the number of shirts and save it to `numberofShirts`:
```java
global variable numberofShirts

method handleNumberofShirts()
{
    print("Enter the number of T-shirts you wish to order:")
    numberofShirts = getInputDouble()
}
```

### handlePromotionalOffer

The three promotional offers are fairly different from each other. I'm going to store this into a global variable `offerNumber` and handle it at the end of the calculations.
```java
global variable offerNumber

method handlePromotionalOffer()
{
    print("Choose a promotional offer:")
    print("1. A $100 discount")
    print("2. A 5% discount")
    print("3. T-shirts get cheaper twice as fast")

    return getInputInt()
}
```

### displayTotalPrice

There are really two parts to this task: calculating the price and actually displaying the price. So
```java
method displayTotalPrice()
{
    variable totalPrice = calculateTotalPrice()
    print("The total price is " + totalPrice)
}
```

But wait, what if the customer tries to calculate the total price before providing the necessary inputs? I have to do something to ensure the customer did those first.

A common strategy to see if a variable was set is to initialize the variable to something illegal, such as -1. So for the global variables above, I'll initialize them to -1:
```java
global variable unitPrice = -1
global variable numberofShirts = -1
global variable offerNumber = -1
```
and then I'll check in `displayTotalPrice`:
```java
method displayTotalPrice()
{
    if (unitPrice == -1 || numberofShirts == -1 || offerNumber == -1)
    {
        print("Please provide the required inputs before calculating the total")
    }
    else
    {
        variable totalPrice = calculateTotalPrice()
        print("The total price is " + totalPrice)
    }
}
```

### calculateTotalPrice

The actual price calculations are pretty mathy. There are formulas for quickly performing the calculation but I'll just do it programatically.

First I need to be able to calculate the price of each T-shirt. I'll save the running price of the `i`th T-shirt:
```java
variable currentPrice = unitPrice
for (i from 1 to numberOfShirts)  // inclusive
{
    // The price is correct _before_ the change below because
    // the below calculates the price of the _next_ shirt
    currentPrice *= 0.99
}
```

Oh wait, if the customer selected promo offer 3, then that should be `0.98`. So I'll save the multiplier to a variable (before I do the calculations):
```java
variable multiplier = 0.99
if (offerNumber == 3)
{
    multiplier = 0.98
}
```

Alright, so all that's left to do is sum the shirt prices and apply promo offers 1 and 2. The final method looks like this:
```java
method calculateTotalPrice()
{
    variable multiplier = 0.99
    if (offerNumber == 3)
    {
        multiplier = 0.98
    }

    variable totalPrice = 0
    variable currentPrice = unitPrice
    for (i from 1 to numberOfShirts)  // inclusive
    {
        totalPrice += currentPrice
        currentPrice *= multiplier
    }

    if (offerNumber == 1)
    {
        totalPrice -= 100
    }
    else if (offerNumber == 2)
    {
        totalPrice *= 0.95
    }

    return totalPrice
}
```

## Turning the pseudocode into a runnable program

The only thing we have to do is use the correct syntax.

## Takeaways

All of these methods are extremely short. Each does only a few things, but together they create complex behavior which would be absolutely disgusting if implemented all at once.

These methods can be simple because the code is structured around the assumption that every method should be simple. Using this assumption allows me to compartmentalize my thinking and only deal with the task immediately in front of me.

Good structure also makes modifying and debugging the code very simple. For example, if a problem starts in `displayTotalPrice`, I know it can't have been caused by any other method. So the problem must be in the 12 lines of `displayTotalPrice`. This program is short, about a hundred lines of code. But in a project with millions of lines of code, finding the problem can become a nightmare if the code isn't structured well.

This entire program was basically written without help from a computer: I didn't need to check the syntax and I didn't run the code. Repeatedly running code to verify the correctness of a program is fine, but do *not* run code to test if a solution is correct. You should know (or at least be confident) that a solution is correct before you write the code for the solution.
