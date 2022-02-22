# Swift

## Basics

- If a parameter has an argument label, the argument must be labeled when you call the function.

- The parameters to functions and closures are always constants.

- A `switch` statement must be exhaustive when considering an enumeration’s cases. Requiring exhaustiveness ensures that enumeration cases aren’t accidentally omitted. When it isn’t appropriate to provide a `case` for every enumeration case, you can provide a `default` case to cover any cases that aren’t addressed explicitly.

- If all of the associated values for an enumeration case are extracted as constants, or if all are extracted as variables, you can place a single `var` or `let` annotation before the case name, for brevity:

    ```swift
    switch productBarcode {
    case let .upc(numberSystem, manufacturer, product, check):
        print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
    case let .qrCode(productCode):
        print("QR code: \(productCode).")
    }
    // Prints "QR code: ABCDEFGHIJKLMNOP."
    ```

- All structures have an automatically generated memberwise initializer, which you can use to initialize the member properties of new structure instances. Initial values for the properties of the new instance can be passed to the memberwise initializer by name: `let vga = Resolution(width: 640, height: 480)`. Unlike structures, class instances don’t receive a default memberwise initializer.

- In fact, all of the basic types in Swift —— integers, floating-point numbers, Booleans, strings, arrays and dictionaries —— are value types, and are implemented as structures behind the scenes. All structures and enumerations are value types in Swift. This means that any structure and enumeration instances you create —— and any value types they have as properties —— are always copied when they’re passed around in your code.

- If you create an instance of a structure and assign that instance to a constant, you can’t modify the instance’s properties, even if they were declared as variable properties:

    ```swift
    let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
    // this range represents integer values 0, 1, 2, and 3
    rangeOfFourItems.firstValue = 6
    // this will report an error, even though firstValue is a variable property
    ```

    Because `rangeOfFourItems` is declared as a constant (with the `let` keyword), it isn’t possible to change its `firstValue` property, even though `firstValue` is a variable property.

    This behavior is due to structures being _value types_. When an instance of a value type is marked as a constant, so are all of its properties.

    The same isn’t true for classes, which are _reference types_. If you assign an instance of a reference type to a constant, you can still change that instance’s variable properties.

- You must always declare a lazy property as a variable (with the `var` keyword), because its initial value might not be retrieved until after instance initialization completes. Constant properties must always have a value _before_ initialization completes, and therefore can’t be declared as lazy.

- Lazy properties are useful when the initial value for a property is dependent on outside factors whose values aren’t known until after an instance’s initialization is complete. Lazy properties are also useful when the initial value for a property requires complex or computationally expensive setup that shouldn’t be performed unless or until it’s needed.

- If a property marked with the `lazy` modifier is accessed by multiple threads simultaneously and the property hasn’t yet been initialized, there’s no guarantee that the property will be initialized only once.

- Global constants and variables are always computed lazily, in a similar manner to [Lazy Stored Properties](https://docs.swift.org/swift-book/LanguageGuide/Properties.html#ID257). Unlike lazy stored properties, global constants and variables don’t need to be marked with the `lazy` modifier. Local constants and variables are never computed lazily.

- You can apply a property wrapper to a local stored variable, but not to a global variable or a computed variable.

- Stored type properties can be variables or constants. Computed type properties are always declared as variable properties, in the same way as computed instance properties.

- Unlike stored instance properties, you must always give stored type properties a default value. This is because the type itself doesn’t have an initializer that can assign a value to a stored type property at initialization time.

    Stored type properties are lazily initialized on their first access. They’re guaranteed to be initialized only once, even when accessed by multiple threads simultaneously, and they don’t need to be marked with the `lazy` modifier.

- You define type properties with the `static` keyword. For computed type properties for class types, you can use the `class` keyword instead to allow subclasses to override the superclass’s implementation. 

- Structures and enumerations are _value types_. By default, the properties of a value type can’t be modified from within its instance methods.

    However, if you need to modify the properties of your structure or enumeration within a particular method, you can opt in to _mutating_ behavior for that method. The method can then mutate (that is, change) its properties from within the method, and any changes that it makes are written back to the original structure when the method ends. The method can also assign a completely new instance to its implicit `self` property, and this new instance will replace the existing one when the method ends.

    You can opt in to this behavior by placing the `mutating` keyword before the `func` keyword for that method:

    ```swift
    struct Point {
        var x = 0.0, y = 0.0
        mutating func moveBy(x deltaX: Double, y deltaY: Double) {
            x += deltaX
            y += deltaY
        }
    }
    var somePoint = Point(x: 1.0, y: 1.0)
    somePoint.moveBy(x: 2.0, y: 3.0)
    print("The point is now at (\(somePoint.x), \(somePoint.y))")
    // Prints "The point is now at (3.0, 4.0)"
    ```

    Note that you can’t call a mutating method on a constant of structure type, because its properties can’t be changed, even if they’re variable properties, as described in [Stored Properties of Constant Structure Instances](https://docs.swift.org/swift-book/LanguageGuide/Properties.html#ID256).

- Within the body of a type method, the implicit `self` property refers to the type itself, rather than an instance of that type. This means that you can use `self` to disambiguate between type properties and type method parameters, just as you do for instance properties and instance method parameters.

    More generally, any unqualified method and property names that you use within the body of a type method will refer to other type-level methods and properties. A type method can call another type method with the other method’s name, without needing to prefix it with the type name. Similarly, type methods on structures and enumerations can access type properties by using the type property’s name without a type name prefix.

- Classes and structures must set all of their stored properties to an appropriate initial value by the time an instance of that class or structure is created. Stored properties can’t be left in an indeterminate state. You can set an initial value for a stored property within an initializer, or by assigning a default property value as part of the property’s definition. 

    When you assign a default value to a stored property, or set its initial value within an initializer, the value of that property is set directly, without calling any property observers.

- For class instances, a constant property can be modified during initialization only by the class that introduces it. It can’t be modified by a subclass.

- We don't use argument labels when calling a function through a variable that's of type function.
