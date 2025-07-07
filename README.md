# What is dependency injection? 

Dependencies, in the context of dependency injection, are  objects a specific class depends on.  
For example, a computer needs RAM, a CPU, an SSD, and a GPU. These components are the computer's dependencies.  

If we think of this computer as a class in OOP, we could either create the components in the Computer class, or we could create them somewhere else.  
If we create them in the class (as attributes), every computer (every instance of the Computer class) will have the same specs.  

## Constructor injection

One way of injecting dependencies is via **Constructor injection**.  
We pass the computer's components as constructor arguments, which allows us to give each computer instance different components.  

## Libraries (complex injection)

Besides constructor injection, there are more complex dependency injection **libraries** that inject the dependencies behind the scenes.  
With these libraries, we can usually also decide about the lifetime of our dependencies.  

In our Computer class example, there is no reason for the GPU to be active when we want to do some office work.  
But for playing video games, we need the GPU.  
So, we could say that the GPU lives as long as the game is open, and not during the whole lifetime of our application.  

## Dependency injection is a design pattern

Instead of initializing an object in a method, we pass it as an argument, or we get the object from somewhere else.  
That way, the place where you create the object is different from the place where you use it.  

## The Benefits

This separation is useful for **testing**, and it makes your code more **maintainable**, and  more **flexible**.  
