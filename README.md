sources: 
- https://www.youtube.com/watch?v=f270BoTicMA
- https://www.youtube.com/watch?v=J1f5b4vcxCQ

# What is dependency injection? 

First of all, it sounds more fancy than it is.  

Dependency injection is simply when you have a piece of code which uses another piece of code,  
and instead of using that code directly, it's passed in as a parameter.  

When you pass something in to be used, we call it "**injection**".  
We inject the dependent code into the code that uses it.  

If a function needs a database connection which comes from a database service, we don't need to create that service inside the function.
We want the service to already be created so we can just focus on the business logic inside that function and do whatever needs to be done with the database.  

In that case, the database service is the dependency, and we're injecting it into the function that needs it.  

# Why does it even exist?

At its core, dependency injection was created to achieve "**inversion of control**".  
Instead of a class being responsible for creating its dependencies, they are provided automatically for us.  

To better understand dependency injection, we need to talk about another important notion in software development.  
This notion is **tightly coupled** vs **loosely coupled** systems, which affects how easy it is to change things in an existing application.  

Dependency injection promotes **loose coupling** between components, which makes the system more: 
- modular,
- testable,
- maintainable.

## Tightly coupled systems

Components are highly dependent on each other's internal details, such as data formats, logic, or implementation.  
- Changes in one component often require changes in others.
- Components are directly connected or reference each other.
- Harder to reuse, test, or maintain individual parts.

In a tightly coupled monolithic application, a change in the database schema may require changes in multiple business logic classes and user interface components.

## Loosely coupled systems

Components interact through well-defined interfaces and have minimal knowledge of each other's internal workings.  
- Components can be modified, replaced, or reused independently.
- Interactions are typically managed through interfaces, APIs, or messaging systems.
- Easier to scale, maintain, and test.

In a microservices architecture, each service communicates via APIs, so changes in one service do not directly affect others as long as the interface contract is maintained.

# Example 

We have a business app where users can chat with their coworkers. They can also send pictures and files to each other.  

When a user sends something, the file gets uploaded to our attachment service.  
The attachement service is responsible for storing, retrievin, and processing all attachments.  
We're going to build up this whole service using dependency injection, and we'll see what it enables us to do.  

When a user sends a message with an attachment, the message text gets sent to our standard chat service.  
We want people to receive their messages almost real-time, so this service is all about speed.  
The attachment, on the other hand, gets uploaded to our attachment service.  

The default storage location is an S3, a part of Amazon's Web Services.  
It's a simple storage service that lets you put up files and pull them down.  
While S3 is 


