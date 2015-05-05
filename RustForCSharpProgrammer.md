
# Rust for C# programmer


```rust
	fn main() {
	    println!("Hello, world!");
	}
```

## Variables 

Rust
```rust
	let x: i32 = 5;
	or 
	let x = 5; // x: i32
	// Default is immutable 
	// reassignment is forbidden
	// x = 10; // ERROR!
```
C#
```c#
	readonly Int32 x = 5; 
```

## If

Normal

```rust
	let x = 5;
	
	if x == 5 {
	    println!("x is five!");
	} else if x == 6 {
	    println!("x is six!");
	} else {
	    println!("x is not five or six :(");
	}
```

c# 
```C#
	int x = 5;
	if (x == 5){
		Console.WriteLine("x is five");
	} else if (x == 6){
		Console.WriteLine("x is six");
	} else {
		Console.WriteLine("x is not five or six :(");
	}
```

Rust

c# 
```C#
	int x = 5;
	int y = x == 5 ? 10 : 15;
```

```rust
	let x = 5;
	
	let y = if x == 5 {
	    10    
	} else {
	    15
	}; // y: i32

	// Without semicolon rust treats the whole if for one single expression
	// and every expression has an return type.
	
	// or better
	let x = 5;
	let y = if x == 5 { 10 } else { 15 }; // y: i32
```

## Functions

```C#
	void foo(){
	}
```

```rust
	fn foo() {
	}
```

```C#
	void PrintNumber(int x) {
		Console.WriteLine("x is: " + x);
	}

	void PrintSum(int x, int y) {
		Console.WriteLine("sum is: " + (x + y));
	}
```

```rust
    // The *Rust* type *()* is like the C# *void*
	fn print_number(x: i32) -> () {
	    println!("x is: {}", x);
	}

	// Without return type specified, its *()*
	fn print_sum(x: i32, y: i32) {
	    println!("sum is: {}", x + y);
	}
```

Like with C# function argument types must be declared explicit.

### With Return values

```C#
	// C#
	int AddOne(int x) {
		return x + 1;
	}	
```

```rust
	// Rust
	fn add_one(x: i32) -> i32 {
	    x + 1
	}
```

With semicolon rust would return () (kind of void). e.g. the last "line" (expression) without semicolon is returned

#### Early return

```C#
	// C#
	int foo(int x) {
	
		if (x < 5) 
			return x;
			
		return x + 1;
	}	
```

```rust
	// Rust
	fn foo(x: i32) -> i32 {
	
	    if x < 5 { return x; }
	
	    x + 1    // Using return x + 1; is considered poor style in rust
	}
```

Rust documentation is recommending to write it his way

```rust
	// Rust
	fn foo(x: i32) -> i32 {
		// *If* is one single expression, and only expression in function foo
		// So return value will be the result of the *if* expression.
	    if x < 5 {
	        x
	    } else {
	        x + 1
	    }
	}
```

## Tuple 

```c#
    // C#
	var x = Tuple.Create(1, "hello");    // x is Tuple<int, string>

	// or
	 
	Tuple<int, string> x = new Tuple<int, string>(1, "hello");
	
```

```rust
    // Rust
	let x = (1, "hello");

	// or

	let x: (i32, &str) = (1, "hello");   // &str is read as *string slice*
```

Because pattern matching is available in *Rust* tuples are used much more than in C#.

```rust
    // Rust
    // A *destructing let*, breaks up the tuple
    // Pattern on the left must match right hand side
	let (x, y, z) = (1, 2, 3);
	println!("x is {}", x);
```

```c#
    // C#
    // Much harder in C# and not really as powerfull as in Rust.
	var tuple = Tuple.Create(1, 2, 3);
    int x = tuple.Item1, y = tuple.Item2, z = tuple.Item3;
    Console.WriteLine("x is " + x);	
```

Multiple return values via Tuples

```c#
    // C#
    Tuple<int, int> NextTwo (int x) 
    {
		return Tuple.Create(x + 1, x + 2);
	}
	
	// usage
	var xy = NextTwo(5);
```

```rust
    // Rust
    fn next_two(x: i32) -> (i32, i32) { (x + 1, x + 2) }

	// usage
	let (x, y) = next_two(5);
```

```rust
	// Tuples can be used as function arguments and as return values
	fn reverse(pair: (i32, bool)) -> (bool, i32) {
	    // `let` can be used to bind the members of a tuple to variables
	    let (integer, boolean) = pair;
	
	    (boolean, integer)
	}
```

```c#
	Tuple<bool, int> Reverse(Tuple<int, bool> pair) 
	{
		var reverse = Tuple.Create(pair.Item2, pair.Item1);
		
		return reverse;
	}
```

## Structs

In Rust *record type* like a tuple. Like classic C structs
But containing elements are a named *field* or *member* like with C# structs/classes.

```c#
	// C#
	public class Pair : Tuple<int, double>
    {
        public Pair(int i, double d) : base(i, d) { }
    }
```

Rust *tuple structs*. 

```rust
	// Rust
	// A tuple struct
	struct Pair(i32, f64);
```

Classic structs.

```c#
	// C#
	public struct Point 
	{
		public double x;
		public double y;
	}

	public struct Rectangle
	{
		public Point p1;
		public Point p2;
	}
```

```rust
	// Rust
	struct Point {
	    x: f64,
	    y: f64,
	}
	
	#[allow(dead_code)]
	struct Rectangle {
	    p1: Point,
	    p2: Point,
	}
```

```c#
	// C#
	// usage
	Point point = new Point() { x = 0.3, y = 0.4 };
	Console.WriteLine("point coordinates: are ({0}, {1})", point.x, point.y);

	int myX = point.x, myY = point.y;

	var rectangle = new Rectangle() {
		p1 = new Point() { x = myX, y = myY },
		p2 = point
	};

	// ...
	
	var pair = new Pair(1, 0.1);

	int intValue = pair.Item1;
	double dblValue = pair.Item2;
	Console.WriteLine("pair contains: {0} and {1}", intValue, dblValue);
```

```rust
	// Rust
	// usage
    // Instantiate a `Point`
    let point: Point = Point { x: 0.3, y: 0.4 };    
    println!("point coordinates: ({}, {})", point.x, point.y);

	// Destructure the point using a `let` binding
    let Point { x: my_x, y: my_y } = point;
    
	let _rectangle = Rectangle {
        // struct instantiation is an expression too
        p1: Point { x: my_y, y: my_x },
        p2: point,
    };

	// ...
	
    let pair = Pair(1, 0.1);
    let Pair(integer, decimal) = pair;
    println!("pair contains {:?} and {:?}", integer, decimal);
```


## Generics

```c#
	// C#
	
	public class Pair<T> 
	{
		public T first;
		public T second;
	}

	...
	public static class Pair
	{
	    public static Pair<X> Create<X>(X a, X b)
	   {
	       return new Pair<X>() { first = a, second = b };
	   }
		
		public static Pair<T> Swap<T> (Pair<T> pair)
		{
			return new Pair<T>() { first = pair.second, second = pair.first };
		}
	}
	...
```

```rust
	// Rust
	struct Pair<T> {
		first: T,
		second: T,
	}

	fn swap<T>(pair: Pair<T>) -> Pair<T> {
		
		// destruct to get 1st and 2nd
		let Pair { first, second } = pair;

		// return a new Pair with swapped elements
		Pair { first: second, second: first }
	}
```


```c#
	// C#
	// Explicitly specialize `Pair`
	var pairOfChars = new Pair<char>() { first = 'a', second = 'b' };

    // Implicitly specialize `Pair`
    var pairOfInts = Pair.Create(1, 2);

    // Explicitly specialize `swap`
    var swappedPairOfChars = Pair.Swap<char>(pairOfChars);

    // Implicitly specialize `swap`
    var swappedPairOfInts = Pair.Swap(pairOfInts);
```

```rust
	// Rust

	// Explicitly specialize `Pair`
    let pair_of_chars: Pair<char> = Pair { first: 'a', second: 'b' };

    // Implicitly specialize `Pair`
    let pair_of_ints = Pair { first: 1i32, second: 2 };

    // Explicitly specialize `swap`
    let _swapped_pair_of_chars = swap::<char>(pair_of_chars);

    // Implicitly specialize `swap`
    let _swapped_pair_of_ints = swap(pair_of_ints);   
```

#### Phantom types

```c#
	// B is only there to carry around additional type information
	public class PhantomClass<A, B> 
	{
		public A first;
		public Type phantom;
		
		public PhantomClass(A a)
		{
			first = a;
			phantom = typeof(B);
		}
	}

    // Specialized to <char, float>

	var struct1 = new PhantomClass<char, float> ('Q');
	
	var struct2 = new PhantomClass<char, double> ('Q');

	// In C# they can be compared but are not equal
	// e.g. there is no compilation or runtime error

	Console.WriteLine("struct1 == struct2 yields: {0}", struct1.Equals(struct2));
    // or
    Console.WriteLine("struct1 == struct2 yields: {0}", 
					    struct1.GetType().Equals(struct2.GetType()));

	// Both is returning false
```

```rust
	// Rust

	// This tuple is a phantom type. B is a hidden
	// parameter. Storage is allocated for generic type A
	// yet not for B. Therefore, B cannot be used in computations.
	#[derive(PartialEq)] // Allow equality test for this type
	struct PhantomTuple<A, B>(A,PhantomData<B>);
	
	// Similarly, a phantom type struct which is generic over A
	// with hidden parameter B
	#[derive(PartialEq)] // Allow equality test for this type
	struct PhantomStruct<A, B> { first: A, phantom: PhantomData<B> }  

	// usage
	
	// We can create similar types without carrying around extra info
    // PhantomTuple specialized to <char, f32>
    let _tuple1:  PhantomTuple<char, f32> = PhantomTuple('Q', PhantomData);
    // PhantomTuple specialized to <char, f64>
    let _tuple2: PhantomTuple<char, f64> = PhantomTuple('Q', PhantomData);

    // Error: type mismatch so these cannot be compared
    //println!("_tuple1 == _tuple2 yields: {}",
    //          _tuple1 == _tuple2);

    // Specialized to <char, f32>
    let _struct1: PhantomStruct<char, f32> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };
    // Specialized to <char, f64>
    let _struct2: PhantomStruct<char, f64> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };

    // Error: type mismatch so these cannot be compared
    //println!("_struct1 == _struct2 yields: {}",
    //          _struct2 == _struct2); 
```

## The power of functional programming

The real power of functional languages like *Rust*, *Haskell* or *F#*.

### Pattern matching

```rust
	let number = 13;

    println!("Tell me about {}", number);
    match number {
        1 => println!("One!"),
        2 | 3 | 5 | 7 | 11 => println!("This is a prime"),
        13...19 => println!("A teen"),
        _ => println!("Ain't special"),
    }

    let boolean = true;
    // Every *match* is an expression, so it can be assigned
    // and evaluated
    let binary = match boolean {
        false => 0,
        true => 1,
    };
```

```c#
	int number = 13;

    Console.WriteLine("Tell me about {0}", number);

	// The C# implementation does not really scale.
	// If statements would not make it better.
    switch (number)
    {
     case 1 : Console.WriteLine("One");
               break;
           case 2:
           case 3:
           case 5:
           case 7:
           case 11:
               Console.WriteLine("This is a prime");
               break;
           case 13:
           case 14:
           case 15:
           case 16:
           case 17:
           case 18:
           case 19: 
               Console.WriteLine("A teen");
               break;
           default:
               Console.WriteLine("Ain't special");

    }


    bool boolean = true;
    int binary = boolean ? 1 : 0;
    // again the C# implementation does not scale. 
    // With more than two possibilities an other approach is needed.
```

#### Destructing tuples

```rust
    // Rust
	let pair = (0, -2, 1);

    println!("Tell me about {:?}", pair);
    match pair {
        (0, y, 0) => println!("First and last is `0` and `y` is `{:?}`", y),
        (x, 0, 0) => println!("`x` is `{:?}` and last ones are `0`", x),
        _      => println!("It doesn't matter what they are"),
    }
```

```c#
	// C#
	var pair = Tuple.Create(0, -2, 1);

    Console.WriteLine(
	    "Tell me about ({0}, {1}, {2})", 
	    pair.Item1, pair.Item2, pair.Item3);

	if(pair.Item1 == 0 && pair.Item3 == 0)
		Console.WriteLine("First and last is `0` and `y` is `{0}`", y);
	else if(pair.Item2 == 0 && pair.Item3 == 0)
		Console.WriteLine("`x` is `{0}` and last ones are `0`", x);
    else
	    Console.WriteLine("It doesn't matter what they are");
	  
	// *Pattern matching* version is much cleaner. And clearer for the eyes 
	// meaning literally. Again C# does not scale here. Imaging tuples with more items.
```


#### Destructing enums

```rust

	enum Color {
	    // These 3 are specified solely by their name.
	    Red,
	    Blue,
	    Green,
	    // This requires 3 `i32`s and a name.
	    RGB(i32, i32, i32),
	}
	
	fn main() {
	    let color = Color::RGB(122, 17, 40);
	
	    println!("What color is it?");
	    match color {
	        Color::Red   => println!("The color is Red!"),
	        Color::Blue  => println!("The color is Blue!"),
	        Color::Green => println!("The color is Green!"),
	        Color::RGB(r, g, b) => {
	            println!("Red: {:?}, green: {:?}, and blue: {:?}!:", r, g, b);
	        },
	    }
	}

```
## Stack and heap

In Rust values are allocated at the stack by default, like it is with C and C++. And in contrary to C#, where
*everything* is allocated a the heap when constructed with *new*.

Rust uses a type *Box<T>*, that is a smart pointer to a heap allocated value, with automatic destroying when going out of scope. C/C++ pointer dereferencing operator * is used to get the value *out of the box*. 

## Objects 

Implementation/functionality is separated from the data in *Rust*.

```rust
	// Rust
	struct Point {
	    x: f64,
	    y: f64,
	}

	// Implementation block, all `Point` methods go in here
	impl Point {
	    // This is a static method
	    // Static methods don't need to be called by an instance
	    // These methods are generally used as constructors
	    fn origin() -> Point {
	        Point { x: 0.0, y: 0.0 }
	    }
	
	    // Another static method, that takes two arguments
	    fn new(x: f64, y: f64) -> Point {
	        Point { x: x, y: y }
	    }
	}   
```

```c#
	// C#
	public class Point
    {
        public double X { get; set; }
        public double Y { get; set; }
        public Point(double x, double y)
        {
            X = x;
            Y = y;
        }

        public static Point Origin()
        {
            return new Point(0.0, 0.0);
        }
    }
```

Or maybe more *Rust* like

```c#
	// C#
	public partial class Point
    {
        public double X { get; set; }
        public double Y { get; set; }
        public Point() { }
    }

	public partial class Point
    {
        public static Point New(double x, double y)
        {
            return new Point() { X = x, Y = y };
        }

        public static Point Origin()
        {
            return new Point(0.0, 0.0);
        }
    }
```

#### Non static member functions

```rust
    // Rust
	struct Rectangle {
	    p1: Point,
	    p2: Point,
	}

	impl Rectangle {
	    // This is an instance method
	    // `&self` is sugar for `self: &Self`, where `Self` is the type of the
	    // caller object. In this case `Self` = `Rectangle`
	    fn area(&self) -> f64 {
	        // `self` gives access to the struct fields via the dot operator
	        let Point { x: x1, y: y1 } = self.p1;
	        let Point { x: x2, y: y2 } = self.p2;
	
	        // `abs` is a `f64` method that returns the absolute value of the
	        // caller
	        ((x1 - x2) * (y1 - y2)).abs()
	    }
	
	    fn perimeter(&self) -> f64 {
	        let Point { x: x1, y: y1 } = self.p1;
	        let Point { x: x2, y: y2 } = self.p2;
	
	        2.0 * (x1 - x2).abs() + 2.0 * (y1 - y2).abs()
	    }
	
	    // This method requires the caller object to be mutable
	    // `&mut self` desugars to `self: &mut Self`
	    fn translate(&mut self, x: f64, y: f64) {
	        self.p1.x += x;
	        self.p2.x += x;
	
	        self.p1.y += y;
	        self.p2.y += y;
	    }
	}   
```

```c#
    // C#
    public class Rectangle
    {
	    public Point P1 { get; set; }
	    public Point P2 { get; set; }
	}

	public static class RectangleExtensions
	{
		public static double Area<T>(this T self)
			where T : Rectangle
		{
			return Math.Abs((self.P1.X - self.P2.X) * (self.P1.Y - self.P2.Y));
		}

		public static double Perimeter<T>(this T self)
			where T : Rectangle
		{
			return 2.0 * Math.Abs(self.P1.X - self.P2.X) + 
				   2.0 * Math.Abs(self.P1.Y - self.P2.Y);
		}
		
		// In C# there is no *mutable* indicator like in *Rust*
		// The only sign that this function is changing/mutating something at
		// the given argument is the return type *void*
		public static void Translate<T>(this T self, double x, double y)
			where T : Rectangle
		{
			self.P1.X += x;
			self.P2.X += x;
			
			self.P1.Y += y;
			self.P2.Y += y;
		}
	}  
```

```rust
	// Rust - usage
	let rectangle = Rectangle {
		p1: Point::origin(),
		p2: Point::new(3.0, 4.0),
	};

	println!("Rectangle perimeter: {}", rectangle.perimeter());
    println!("Rectangle area: {}", rectangle.area());

	// This Rectangle will be translated. Therefore it must be mutable.
	// This is different than a *readonly* variable in C#, where only 
	// the *square* variable could not be reassigned. 
	// But the members P1 and P2 could be reassigned.
	// *mut* in *Rust* is more. p1 and p2 are not allowed to be reassigned.
	let mut square = Rectangle {
        p1: Point::origin(),
        p2: Point::new(1.0, 1.0),
    };

    // Error! `rectangle` is immutable, but this method requires a mutable object
    //rectangle.translate(1.0, 0.0);

    // Ok, mutable object can call mutable methods
    square.translate(1.0, 1.0);
```

```c#
	// C# - usage
	var rectangle = new Rectangle() {
		P1 = Point.Origin(),
		P2 = Point.New(3.0, 4.0)
	};

	Console.WriteLine("Rectangle perimeter: {0}", rectangle.Perimeter());
	Console.WriteLine("Rectangle area: {0}", rectangle.Area());

	var square = new Rectangle() {
		P1 = Point.Origin(),
		P2 = Point.New(1.0, 1.0)
	};

	square.Translate(1.0, 1.0);
```

## *Interfaces* and *Traits*

```c#

	public interface
```
