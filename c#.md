.Net basics
-----------
    Normal program
        Code => language specific compiler => native code (specific to operating system)
    
    .NET app => .net compiler => intermediate language (IL) => JIT compiler => native code

    CLR Components
        Common Type System , Garbage Collector, Just-In-Time Compiler
        Advantages: Language Interoperability, Memory Management, Type Safety, Platform Independence, Performance Optimization, Exception Handling, Security

    ILASM => To see Manifest , Intermediate language    
        Manifest => name, version, culture and & public key, if the assembly is strong named

    Strong naming an assembly
        assembly name consists => Simple textual name , Version number, Culture information & Public key token
        assembly version => 1.0.0.0
        sn.exe -k c:\KeyFile.snk
        [assembly: AssemblyKeyFile("KeyFile.snk")]

    GAC (Global Assembly Cache)
        Gacutil -i C:\SampleProject\SampleAssembly.dll | Gacutil -u MyClassLibrary
    
    DLL Hell => DLL Hell refers to the problems caused when multiple applications attempt to share a common Dynamic Link Library (DLL) file, but require different versions. This can lead to issues such as version conflicts, missing dependencies
    If its strong name assemply the publickey and verison information can seen in manifest.

C# basics
---------
   Verbatim literal => @ => @"{Name}"

   Built-in-Types => 
        Boolean  
        Integral (Sbyte, byte, short , ushort, int, uint, long, ulong, char)
        Floating types (float and double)
        Decimal 
        String

   String excapes => "\"Pragim\"" | "One\nTwo\nThree" | "c:\\Pragim\\DotNet\\Training\\Csharp" | @"c:\Pragim\DotNet\Training\Csharp"

   Operators
        Assignment Operator =
        Arithmetic Operators like +,-,*,/,% 
        Comparison Operators like ==, !=,>, >=, <, <= 
        Conditional Operators like &&, ||
        Ternary Operator ?:
        Null Coalescing Operator ??

   Nullable Types => ? | To hold null values | usecase => Agree | Disagree | Not Answered.

   Conversions
        Implicit => Example: Converting an int to a float will not loose any data
        Explicit => Where as when converting a float to an int, we loose the fractional part and also a possibility of overflow exception 
        Parse() and TryParse()
    
    Arrays
        int[] array1 = new int[5]; // Declares an array of integers with 5 elements
        int[] array2 = new int[] { 1, 2, 3, 4, 5 }; // Declares and initializes an array
        int[] array3 = { 1, 2, 3, 4, 5 }; // Shorter syntax for declaring and initializing 

        int[][] jaggedArray1 = new int[3][]; // Declares a jagged array with 3 elements
        jaggedArray1[0] = new int[5]; // Initializes the first element with an array of 5 integers
        jaggedArray1[1] = new int[3]; // Initializes the second element with an array of 3 integers
        jaggedArray1[2] = new int[4]; // Initializes the third element with an array of 4 integers

    Comments
        Single line Comments       -   //
        Multi line Comments        -   /*  */
        XML Documentation Comments -   ///

    switch
        string result = number switch
        {
            1 => "One",
            2 => "Two",
            3 => "Three",
            _ => "Unknown"
        };

        string result = obj switch
        {
            int and > 0 => "Positive integer",
            int and < 0 => "Negative integer",
            int n => "Zero",
            string s => $"String of length {s.Length}",
            _ => "Unknown type"
        };

    Loops
        While , doWhile, for , foreach

    Methods => Instance (Each instance will have different method) vs Static (one method per program)
               Value | Reference | Out | Params (Should be at last)               

    ref Must be initialized | Can be read and modified | Use Case => Modify existing value
    out Does not need to be initialized | Must be assigned before return | Return multiple values

    Namespace

    Class 
        Can have fields , methods , properties, events, indexers, constructors.
        Staic Constructor
            Complex Initialization Logic
            If you want to know | print the values beeing set to static member
            Initialization of Static Read-Only Fields

    OOPS
        Inheritance
            Single Inheritance
            multiple interface
            child class is specialization of base class
            parent class constuctor execute before child class

            When you call a base class constructor from a derived class constructor, 
            the types and arguments are resolved in the context of the derived class, 
            but the actual execution order of the constructors is such that the base class constructor 
            is executed first, followed by the derived class constructor.

            Method hiding => compiler warning if don't use the new before the method 
                             cast child type to parent type and invoke the hidden member

            

        Polymorphism
            Runtime => virtual & override
                      BaseClass base = new DerivedClass();
                      base.Display();
                      //DerivedClass Display method

                      DerivedClass base = new DerivedClass();
                      base.Display();
                      //DerivedClass Display method

                      //Onlyway to call base is to have base.Display() inside the DisplayMethod
                      //Usecase additional logic ontop of the existing

            CompileTime => Overloading
                      Type & Kind (Value, ref or out)
                      Return type and params keyword not possible.

        Encapsulation
                    Definition: Encapsulation is the process of bundling the data (variables) 
                    and the methods (functions) that operate on the data into a single unit, known as a class. 
                    It also involves restricting direct access to some of the object's components, 
                    which is a means of preventing accidental interference and misuse of the data.
                    Purpose: The main purpose of encapsulation is to protect the internal state of an object 
                    and to ensure that the object's data is accessed and modified only in a controlled manner.

                    Access modifiers & properties

                    public int Age
                    {
                        get { return age; }
                        set
                        {
                            if (value >= 0)
                            {
                                age = value;
                            }
                        }
                    }

        Abstraction
                    Abstraction is the process of hiding the complex implementation details 
                    and showing only the essential features of an object. 
                    It focuses on what an object does rather than how it does it.

                    Purpose: The main purpose of abstraction is to reduce complexity 
                    and allow the programmer to focus on interactions at a higher level.
