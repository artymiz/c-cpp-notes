# C++ features


## Constructors
The constructor method is called when an object of a class is created. 
The constructor function should be the **same as the class name**.


### Member variable initialization
The member variable of a class can be initialized with several ways during the
creation of a class object.


See [cppreference](https://en.cppreference.com/w/cpp/language/constructor) for more information.


1. **Default Member initialization** - member fields are assigned to constant values
    ```clike
    class Point {
        double x = 0;
        double y = 0;
    public:
        Point();
        // ...
    };
    ```
2. **Assignment in constructors** - setting the member values in the constructor body
    ```clike
    class Point() {
        double x, y;
    public:
        Point() {
            x = 0;
            y = 0;
        }

        Point(double x, double y) {
            // 'this' keyword is needed to differentiate
            // member variable of the class and the constructor argument (parameter)
            this->x = x;
            this->y = y;
        }
    };
    ```
3. **Initialization Lists (member initializor in constructors)** -
    ```clike
    class Point() {
        double x, y;
    public:
        Point() : x{0}, y{0} {
        }

        // 'this' keyword NOT needed, the part inside {} is the argument,
        // and the outside is the class member variable
        Point(double x, double y) : x{x}, y{y} {
        }
    };
    ```


*Which method is preferred when writing C++?*


According to the [Cpp Core Guidelines (C48 & C49)](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-in-class-initializer):


generally, **default member initialization** > **Initialization Lists** > **assignment in constructors**


## Function Overloading

Sometimes you may want to have different parameters for the same functionality.

C++ has the **function overloading** feature - when there are multiple functions
with the same name, the compiler will **infer which version of the function to use**.


## Destructor
The destructor cleans up memory used by the class, it is called when the class
object **goes out of scope**, or when the `delete` keyword is used.


The destructor of a class or struct is prefixed by `~` followed by the class name. 

```clike
class Square {
private:
    Point position;

public:
    Square(); // default constructor
    Square(double x, double y) : position{ Point(x,y) } {
    }
    ~Square(); // the destructor
    draw();
};


void drawSquare() {
    Square someSquare; // the calls the default constructor and puts 
    someSquare.draw();
    // when this function exits, someSquare goes 'out of scope'
    // and the destructor is called for the statically (stack) allocated object
}

int main() {
    Square *squareA = new Square();
    // dynamically (heap) allocated squareA's destructor is called on the 'delete' keyword
    delete squareA;

    drawSquare();
}
```

The destructors of a class is propagated -

in the example above,  when the `Square` class gets destructed the destructor for `position` also gets triggered
because `position` is stack allocated.

In the different example where the members heap allocated with the `new` keyword,
`delete` must be used to trigger the destructor.

```clike
// memory of houseAddress gets cleaned up only after calling `delete`
class Neighbourhood {
    House *houseAddress = new House();
public:
    Neighbourhood();
    ~Neighbourhood() {
        delete houseAddress;
    }
};

// when Cat is destroyed, statically allocated body is as well
class Cat {
    Body body;
public:
    Cat();
    ~Cat();
};
```


