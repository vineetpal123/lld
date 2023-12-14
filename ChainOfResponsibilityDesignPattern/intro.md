### This Chain of responsibility Design pattern has been asked in Amazon and many other top product based companies.
### This pattern is used for many real time application usage like ATM design, Vending Machine design and Logging System design

### The Chain of Responsibility pattern provides a chain of loosely coupled objects one of which can satisfy a request. This pattern is essentially a linear search for an object that can handle a particular request.

# client -> Handler1 -> Handler 2 -> Handler

### This pattern falls under the category of behavioral Design patterns. In simple terms, this pattern lets the application pass requests along a chain of handlers. Each handler can decide either to process the request or to pass it to the next handler in the constructed sequence. This pattern is mainly used to introduce loose coupling to the application. It basically decouples the sender and receiver of a particular request based on the type.

# Usage
- When the program processes different kinds of requests in various ways but sequences are unknown beforehand.
- When there is a requirement to execute several handlers in a particular order.
- When the sequence of handlers changes at runtime.

# Pros and Cons
## Pros
- Possibility to control the order of request handling.
- Introduce new handlers into the application without breaking the existing code.
- Decoupling of classes(Operations invoking and performing.
## Cons
- Possibility of having unhandled requests.
- Possibility of the end-user to manipulate the application.

# https://medium.com/nerd-for-tech/chain-of-responsibility-design-pattern-4efe8de4910d