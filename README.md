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
The attachement service is responsible for storing, retrieving, and processing all attachments.  
We're going to build up this whole service using dependency injection, and we'll see what it enables us to do.  

When a user sends a message with an attachment, the message text gets sent to our standard chat service.  
We want people to receive their messages almost real-time, so this service is all about speed.  
The attachment, on the other hand, gets uploaded to our attachment service.  

The default storage location for the attachments is an S3, a part of Amazon's Web Services.  
It's a simple storage service that lets you put up files and pull them down.  

While S3 is nice and most of our clients are okay with it, some companies might not want us to permanently store their data.  
This means we actually need our service to handle multiple storage locations.  
Then, depending on which compay a user is from, we need to put their attachments in the right place.  

Most picky clients will give us an SFTP server to connect to, but some might want us to use a specific protocol such as WebDAV.  
In this example, our attachment service needs to handle 3 different storage locations: 
- AWS S3,
- SFTP server,
- WebDAV server.

## Without dependency injection

Our first thought might be to simply extend our existing **upload** method with some if statements, and then  
have the caller of our upload method pass in where to upload the file. But this is not a good idea...  

**First**, it gives our **Storage** class too many responsibilities, which breaks the first SOLID principle and makes the code hard to read.  
The code for SFTP is intermingled with the code for AWS and WebDAV, and it's not ideal for every dev to understand what it does.  

**Second**, using the class is not very simple.   
We have one upload function which needs a bunch of info to determine where it's going to upload the attachment.  
But what info it needs is very different depending on where the attachment is going:
- if it's AWS, we need the access key ID and the secret access key
- if it's SFTP, we need the host, port, username, and private key
- if it's WebDAV, we need a URL and the Auth key
This forces us to have a bunch of optional variables that need to be filled out in certain cases, and we also need comments
to clarify our code:
![image](https://github.com/user-attachments/assets/d9885e5c-b8e3-4756-90bd-407be254e32a)

And **finally**, the part of the code that actually calls the upload method needs to have all of this destination-specific context
to perform the upload, which is not good for clarity and does not apply the DRY principle (Don't Repeat Yourself).

## With dependency injection

Let's create an interface that represents our attachment storage:
```ts
export interface Storage {
  /**
   * Store an attachment
   * @param attachment The attachment to store
   * @return The attachment id
   */
  upload(attachment: Attachment): Promise<string>;

  /**
   * Retrieve the attachment from the storage server
   */
  download(attachment_id: string): Promise<Attachment>: 
}
``` 

@4/13 CodeAesthetic
