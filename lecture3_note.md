### Range 
- Floating point Countable Range
  - How do you 
  for(i = 0.5; i <= 15.25; i += 0.3)?
    Floating point numbers do not stride by Int, they stride by a floating point value. So, 0.5 ... 15.25 is just a Range, not a Countable Range(which is needed for 'for in')
- Luckily, there's a global function that will create a CountableRange from floating point values. 
for i in stride(from: 0.5, through: 15.25, by 0.3){}
The return type of stride is CountableRange
* Actually ClosedCountableRange in this case because it is through: instead of to: 
* As we will see with String later, CountableRange is a generic type.


### Tuple
- What is a tuple?
  - It is nothing more than a grouping of values. You can use it anywhere you can use a type.
let x: (String, Int, Double) = ("hello", 5, 0.85)
let (word, number, value) = x 
// this names the tuple elements when accessing the tuple
print(word) // hello
print(number) // 5
print(value) //0.85
... or the tuple elements can be named when the tuple is declared.
```swift
let x: (w: String, i: Int, v: Double) = ("hello",5, 0.85)
print(x.w) // hello
print(x.i) // 5
print(x.v) // 0.85
let(wrd, num, val) = x // this is also legal(renames the tuple elements on access)
```
- Tuples as return values
  - You can use tuples to return multiple values from a function or methods...
``` swift
func getSize() -> (weight: Double, height: Double) {
    return (250, 80)
}
let x = getSize()
print("weight is \(x.weight)") //250
...or ...
print("height is \(getSize().height)") //80
```
### Computed Properties
- The value of a property can be computed rather than stored.
  - A typical stored property looks something like this...
``` swift
var foo: Double
```
- A computed property looks like this...
``` swift
var foo: Double {
    get{
        //return the calculated value of foo
    }
    set(newVale){
        //do something based on the fact that foo has changed to newValue
    }
}
```
- You do not have to implement the set side of it if you do not want to. The property then becomes "read only"

- Why compute the value of a property?
  - Lots of times a "property" is "derived" from other state. 
  ``` swift
  var indexOfOneAndOnlyFaceUpCard: Int?
  ```
- In fact, properly keeping this var-up-to-date is just plain error-prone. This would be safer ...
``` swift
var indexOfOneAndOnlyFaceUpCard: Int? {
    get{
        // look at the cards and see if you find only one that is face up
        // if so, return it, else return nil
    }
    set {
        //turn all cards face down except the card at index newValue
    }
}
```

### Access Control
- Protecting our internal implementations.
  - Swift supports this with following access control keywords..
    - Internal - this is default, it means "usable by any object in my app or framework"
    - private - this means "only callable from within this object"
    - private(set) - this means "this property is readable outside this object, but not settable"
    - fileprivate - accessible by any code in this source file.
    - public - (for frameworks only) this can be used by objects outside my framework
    - open - (for frameworks only) public and objects outside my framework can subclass this

- A good strategy is to just mark everything private by default. Then remove the private designation when API is ready to be used by other code

### Extensions
- Extending existing data structures
  - You can add methods/properties to a class/struct/enum(even if you do not have the source)
- There are some restrictions
  - You can re-implement methods or properties that are already there(only add new ones). The properties you add can have no strange associated with them(computed only)
- This feature is easily abused
  - It should be used to add clarity to readability not obfuscation! Do not use it as a subtitute for good object-oriented design technique. Best used for very small, well contained helper functions. Can actually be used well to organize code but requires architectual commitment. 

### Enum
- Another variety of data structure in addition to struct and class
  - It can only have discrete states ...
  ``` swift
  enum FastFoodMenuItem {
      case hamburger(numberOfPatties: Int)
      case fries(size: FryOrderSize)
      case drinks(String, ounces: Int) // the unnamed String is the brand, e.g "Coke"
      case cookie
  }
  ```
- An enum is a VALUE TYPE(like struct), so it is copied as it is passed around.
- Note that the drink case has 2 pieces of associated data(one of them "unnamed") In the example above, FryOrderSize would also probably be an enum, for exampe ..

``` swift
enum FryOrderSize {
    case large
    case small
}
```
- Setting the value of an enum
  - When you set the value of an enum, you must provide associated  data(if any)...
  ``` swift
  let menuItem: FastFoodMenuItem = FastFoodMenuItem.hamburger(patties: 2)
  var otherItem: FastFoodMenuItem = FastFoodMenuItem.cookie
  var yetAnotherItem = .cookie // swift cannot figure this out
  ```
- Checking an enum's state
  - An enum's state is checked with a switch statement ...
  ``` swift
  var menuItem = FastFoodMenuItem.hamburger(patties: 2)
  switch menuItem {
      case FastFoodMenuItem.hamburger: print("burger")
      case FastFoodMenuItem.fries: print("fries)
  }
  ```
  This code would print "burger" on the console
  It is not necessary to use the fully-expressed FastFoodMenuItem.fries inside the switch(since Swift can infer the FastFoodMenuItem part of that)
- break
  - If you do not want to do anything in a given case, use break.
- default
  - You must handle ALL Possible Cases (although you can default uninteresting cases)
  ``` swift
  var menuItem = FastFoodMenuItem.cookie
  switch menuItem {
      case .hamburger: break
      case .fries: print("fries")
      default: print("other")
  }
  ```
- Multiple lines allowed
  - Each case in a switch can be multiple lines and does not fall through to the next case
  - By the way, we can let Swift infer the enum type of the size of the fries too...
  ``` swift
  var menuItem = FastFoodMenuItem.fries(size: .large)
  ```
- What about the associated data?
  - Associated data is accessed through a switch statement using this 'let' syntax ..
  ``` swift
  var menuItem = FastFoodMenuItem.drink("coke", ounces: 32)
  switch menuItem {
      case .hamburger(let pattyCount): print("a burger with \(pattyCount) patties")
      case .fries(let size): print("a \(size) order of fries!")
      case .cookie: print("a cookie!")
  }
  ```
  Note that the local variable that retrives the associated data can have a different name
- Methods yes, (stored) properties no
  - An enum can have methods (and computed properties) but no stored properties ...
  ``` swift
  enum FastFoodMenuItem {
      case hamburger(numberOfPatties: int)
      case fries(size: FryOrderSize)
      case drink(String, ounces: Int)
      case cookie

      func isIncludedInSpecialOrder(number: int) -> Boo {
          //
      }
      var calories: Int{
          //calculate and return caloric value here
      }
  }
  ```
  An enum's state is entirely which case it is in and that case's associated data.
- In an enum's own methods, you can test the enum's state(and get associated data) using self///
``` swift
enum Fast FoodMenuItem {
    ...
    func isIncludedInSpecialOrder(number: Int) -> Bool {
        switch self{
            case .hamburger(let pattyCount): return pattyCount == number
            case . fries, .cookie: return true // a drink and cookie in every special order
            case .drink(_, let ounces): return ounces == 16 
        }
    }
}
```
- Modifying self in an enum
  - You can even reassign self inside an enum methods. ..
  ``` swift
  enum FastFoodMenuItem {
      ...
      mutating func switchToBeingACookie() {
          self = .cookie // this works even if self is a .hamburger, .fries or .drink
      }
  }
   ```
  Note that 'mutating' is required because enum is a VALUE TYPE

### Optional
- an Optional is just an enum
- It essentially looks like this ...
``` swift
enum Optional<T>{
    // a generic type, like Array<Element> or Dictionary<Key, Value>
    case none
    // the some case has associated data of type T
    case some(<T>)
}
```
But this type is so imporant that it has a lot of special syntax that other types do not have...
- Special Optional syntax in Swift
  - "not set" case has a special keyword: nil
  - '?' is used to declare an Optional(var indexOfOneAnd: Int?)
  - '!' is used to "unwrap" the associated data if an Optional is in the "set" state (let index = cardButtons.index(of: button)!)
  - 'if' can also be used to conditionally get the associated data ...(if let index = cardButtons.index(of: butto) {})
  - An optional declared with '!' (instead of '?') will implicitly unwrap(add !) when accessed..(var flipCountIndex: UILabel!  -> enables flipCountIndex.text = "..." // no '!' here)
  - '??' to create an expression which "defaults" to a value if an Optional is not set. (return emoji[card.identifier] ?? "?")
  - You can also use '?' when accessing an optional to bail out of an expression midstream.


### Memory Management
- Automatic Reference Counting
  - Reference types(classes) are stored in the heap.
  - How does the system know when to reclaim the memory for these from the heap? It "counts references" to each of them and when there are zero references, they get tossed. This is done automatically. It is known as "Automatic Reference Counting" and it is nOT garbage collection.
- Influencing ARC
  - You can influence ARC by how you declare a reference-type var with these keywords ...
  - strong : "normal" reference counting. As long as anyone, anywhere has a strong pointer to an instance, it will   stay in the heap
  - weak: weak means "if no one else is interested in this, then neither am I, set me to nil in that cases." Because it has to be nil-able, weak only applies to Optional pointers to reference types. 
  A weak pointer will NEVER keep an object in the heap
  Great example: outlets(strongly held by the view hierachy, so outlets can be weak)
  
  - unowned: this means "do not reference count this; crash if I am wrong" This is very rarely used. Usually only to break memory cycles between objects.

### Struct
- Value type(structs do not live in the heap and are passed around by copying them)
- very efficient "copy on write" is automatic in Swift
- No inheritance(of data)
- This copy of on write behavior requires you to mark mutating methods.
- Mutability controlled via let(e.g. you cannot add elements to an Array assigned by let)
- Supports functional programming design
- Example: Card, Array, Dictionary, String, Character, Int, Double, UInt32
