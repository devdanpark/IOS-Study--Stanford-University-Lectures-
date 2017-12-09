### Protocol
- A type which is a declaration of functionality only
- No data storage of any kind(so it does not make sense to say it is a "value" or "ref" type)
- Essentially provides multiple inheritance(of functionality only, not storage) in swift
- Protocols are a way to express an API more concisely
  - Instead of forcing the caller of an API to pass a specific class, struct or enum, an API can let callers pass any class/struct/enum that the caller wants.
  - But, can require that they implement certain methods and/or properties that the API wants.
  - THe API expresses the functions or variables it wants the caller to provide using a protocol. So, a protocol is simply a collection of method and property declarations.

- What are protocols good for?
  - Making API more flexible and expressive
  - Blind, structured communication between View and Controller(delegation)
  - Mandating behavior
  - Sharing functionality in disparate types(String, Array, CountableRange are all Collections)
  - Multiple inheritance(of functionality, not data)

- There are three aspects to a protocol
  - the protocol declaration(which properties and methods are in the protocol)
  - a class, struct or enum declaration that makes the claim to implement the protocol
  - the code in said class, struct or enum(or extension) that implements the protocol.

- Optional methods in a protocol
  - Any protocol that has optional methods must be marked @objc
  - Any class that implements an optional protocol must inherit from NSObject
  - 