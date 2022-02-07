# Data Structure
  * Arrays 
    * Disadvantages: Arrays cannot grow in size once initialized. Have to rely on integral indices to store or retrieve items from the array.
    * Advantages: Arrays are strongly typed.
    ```
     int[] EvenNumbers = new int[3];
     int[] OddNumbers = { 1, 3, 5};

    ```
    
 
  * ArrayList -> Can grow in size but casting is required to retrive the value (ArrayList a= new ArrayList ; a.Add(0) //Add(object? value)
  * List -> Can grow in size (TypeSafe)
  * ![image](https://user-images.githubusercontent.com/71544024/152848521-c97b938d-fa9b-4822-93b7-03226fb52e4f.png)
  * ![image](https://user-images.githubusercontent.com/71544024/152848834-94464146-79a1-4ba8-9330-4cbecdb56f59.png)
  * ![image](https://user-images.githubusercontent.com/71544024/152848944-a0afc39f-6493-4797-aabb-de1cd3eb1b6a.png)
  * Hashtable -> Key - Value pair. Both values are Objects. So casting is required to retreive.
  * Dictionary -> Strongly typed key value pair
   * ![image](https://user-images.githubusercontent.com/71544024/152847865-8799d65c-6dd1-49d6-b9dd-9b1386064fc5.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152848199-30274034-09d5-46c0-ad5b-ec5462fe06d3.png) 
   * ![image](https://user-images.githubusercontent.com/71544024/152851675-9e5f4d50-3905-49fc-8cbe-d45475bac972.png)
  * Queue
   * ![image](https://user-images.githubusercontent.com/71544024/152851850-6610e688-402c-4700-a135-dbf65217581f.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152852156-7f16df22-584c-44fe-87e8-8a3b0ce8d3b6.png)
   
  * Stack
   * ![image](https://user-images.githubusercontent.com/71544024/152851951-7e1b2c26-9801-4f77-8c6d-006c47974427.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152852346-e401a451-ccd8-46b9-8f9f-9a1d8d5ba165.png)
 
  * SortedList -> Sorted generic Generic, Bit slower while inserting.
  * Sorting
    * ![image](https://user-images.githubusercontent.com/71544024/152849119-80ceafa5-a29d-4658-9741-899575e3d164.png)
    * ![image](https://user-images.githubusercontent.com/71544024/152849367-953df9af-c390-40e6-bec5-a3d0bfe1e616.png)
    * ![image](https://user-images.githubusercontent.com/71544024/152849635-fed44d1a-839f-44c4-ac43-d21e4d03858c.png)
    * ![image](https://user-images.githubusercontent.com/71544024/152849841-c5c136b6-7558-4794-a007-dc8f1866da73.png)
    * ![image](https://user-images.githubusercontent.com/71544024/152850082-5d878a63-7ed0-4868-8b73-1cfb3455dc88.png)
  * Usefull Methods
    * ![image](https://user-images.githubusercontent.com/71544024/152851361-9ff4c68a-644f-44d2-96e7-20493dc472b3.png)
   

# Generics
  * ![image](https://user-images.githubusercontent.com/71544024/152842954-8165968c-4db3-418e-b84f-62c06696385e.png)
  * ![image](https://user-images.githubusercontent.com/71544024/152843089-45d4d946-7b22-4356-a330-c5cdc7ac5724.png)
  * ![image](https://user-images.githubusercontent.com/71544024/152843486-b283dbec-5148-4ba7-97d6-42b5cdc15218.png)

# Override Equals Method
  * ![image](https://user-images.githubusercontent.com/71544024/152845072-985c7e5c-d9a9-4db5-9e94-d696b0553d14.png)


# New features
  * Init only setters
  * Record typr (with key)
  * Global Usings => (global using System.Linq) / project file '<using include='System.Linq'>'
  * Inline namespace 'namespace MyNamespace;'
 
# Thread
 * Process - Process is what the operating system uses to facilitate the execution of a program by providing the resources required. Each process has a unique process Id associated with it. You can view the process within which a program is being executed using windows task manager.
 * Thread - Thread is a light weight process. A process has at least one thread which is commonly called as main thread which actually executes the application code. A single process can have multiple threads.

Please Note: All the threading related classes are present in System.Threading namespace
 * Advantages of multithreading:
1. To maintain a responsive user interface
2. To make efficient use of processor time while waiting for I/O operations to complete.
3. To split large, CPU-bound tasks to be processed simultaneously on a machine that has multiple CPUs/cores.
 
 Disadvantages of multithreading:
1. On a single-core/processor machine threading can affect performance negatively as there is overhead involved with context-switching.
2. Have to write more lines of code to accomplish the same task 
3. Multithreaded applications are difficult to write, understand, debug and maintain
   ```
   Thread T1 = new Thread(Number.PrintNumbers); //It's working in spite of not creating the ThreadStart delegate explicitly because the framework is doing it automatically for us
   Thread T1 = new Thread(new ThreadStart(Number.PrintNumbers));
   Thread T1 = new Thread(delegate() { Number.PrintNumbers(); });
   Thread T1 = new Thread(() => Number.PrintNumbers());
 
   // Create an instance ParameterizedThreadStart delegate
   ParameterizedThreadStart parameterizedThreadStart =
   new ParameterizedThreadStart(Number.PrintNumbers);
   Thread T1 = new Thread(parameterizedThreadStart);
   // Pass the traget number to the start function, which
   // will then be passed automatically to PrintNumbers() function
   T1.Start(target);   
   public static void PrintNumbers(object target){ }
 
   //The code in the Main() function can also be written as shown below
   public static void Main()
   {
    Console.WriteLine("Please enter the target number");
    object target = Console.ReadLine();

    Thread T1 = new Thread(Number.PrintNumbers);
    T1.Start(target);
   }
   
   ```
 * Here we are not explicitly creating an instance of ParameterizedThreadStart delegate. Then how is it working?
It's working because, the compiler implicitly converts new Thread(Number.PrintNumbers)  to new Thread(new ParameterizedThreadStart(Number.PrintNumbers)).

When to use ParameterizedThreadStart over ThreadStart delegate?
Use ParameterizedThreadStart delegate if you have some data to pass to the Thread function, otherwise just use ThreadStart delegate.

Please note: Using ParameterizedThreadStart delegate and Thread.Start(Object) method to pass data to the Thread function is not type safe as they operate on object datatype and any type of data can be passed. If you try to change the data type of the target parameter of PrintNumbers() function from object to int, a compiler error will be raised as the signature of PrintNumbers() function does not match with the signature of ParameterizedThreadStart delegate.
 * Avoid parameter conversion by using the internal variable and assign values using constructor.
 * CallBacks:
 ```
 public delegate void SumOfNumbersCallback(int input)
 class NumberCompute
    {
        int _target
        SumOfNumbersCallback _callbackMethod;
        Number(int target,SumOfNumbersCallback sumOfNumbersCallback)
        {
            _tartget=target;
            _callbackMethod = sumOfNumbersCallback;
        }
 
       public static void print(int input)
       { //Do Something }
 
       public void Compute()
       { //Do Something }
    }
 
 Main()
 {
   SumOfNumbersCallback sumOfNumbersCallback = new SumOfNumbersCallback(print);
   var target=20;
   
   NumberCompute obj= new NumberCompute(target,sumOfNumbersCallback)
 
   Thread t=new Thread(new ThreadStart(obj.Compute));
   t.Start();
 }
 ```
* Join blocks the current thread and makes it wait until the thread on which Join method is invoked completes. Join method also has a overload where we can specify the timeout. If we don't specify the timeout the calling thread waits indefinitely, until the thread on which Join() is invoked completes. This overloaded Join(int millisecondsTimeout) method returns boolean. True if the thread has terminated otherwise false. Join is particularly useful when we need to wait and collect result from a thread execution or if we need to do some cleanup after the thread has completed.

IsAlive returns boolean. True if the thread is still executing otherwise false
* Protecting shared resources
 
  ```
  using System;
  using System.Threading;
  class Program
  { 
    static int Total = 0;
    public static void Main()
    {
        Thread thread1 = new Thread(Program.AddOneMillion);
        Thread thread2 = new Thread(Program.AddOneMillion);
        Thread thread3 = new Thread(Program.AddOneMillion);

        thread1.Start();
        thread2.Start();
        thread3.Start();

        thread1.Join();
        thread2.Join();
        thread3.Join();

        Console.WriteLine("Total = " + Total);
    }

    public static void AddOneMillion()
    {
        for (int i = 1; i <= 1000000; i++)
        {
            Total++;
        }
      }
    }

  ```
  * Every time we run the above program, we get a different output. The inconsistent output is because the Total field which is a shared resource is not protected from concurrent access by multiple threads. The operator ++ is not thread safe. There are several ways to fix this. Let's explore 2 of the options.
  * Interlocked.Increment(ref Total);
  * static object _lock = new object();  lock (_lock){ Total++; }
  * Which option is better? From a performance perspective using Interlocked class is better over using locking. Locking locks out all the other threads except a single thread to read and increment the Total variable. This will ensure that the Total variable is updated safely. The downside is that since all the other threads are locked out, there is a performance hit.
  * The Interlocked class can be used with addition/subtraction (increment, decrement, add, etc.) on and int or long field. The Interlocked class has methods for incrementing, decrementing, adding, and reading variables atomically.
  ```
    Stopwatch stopwatch = Stopwatch.StartNew(); //1 millisecond consists of 10000 ticks.
    stopwatch.Stop();
        stopwatch.ElapsedTicks
         Please Note: You can use the TimeSpan object to find ticks per second, ticks per millisecond etc. 
         Stopwatch class is in System.Diagnostics namespace.
         
    Monitor Vs Lock (Lock is shortcut for monitor)
         
    static object _lock = new object();
 
        lock (_lock)
        {
            Total++;
        }
         
        Monitor.Enter(_lock);
        try
        {
            Total++;
        }
        finally
        {
            // Releases the exclusive lock
            Monitor.Exit(_lock);
        }
         
         
                bool lockTaken = false;
        // Acquires the exclusive lock
        Monitor.Enter(_lock, ref lockTaken);
        try
        {
            Total++;
        }
        finally
        {
            // Releases the exclusive lock
            if (lockTaken)
                Monitor.Exit(_lock);
        }
      So, in short, lock is a shortcut and it's the option for the basic usage. If you need more control to implement advanced multithreading solutions using TryEnter() Wait(),       Pulse(), & PulseAll() methods, then the Monitor class is your option.
                                    
      class ThreadSafe
     {
      static readonly object locker = new object();
      static int val1, val2;

      static void Go()
      {
       lock (locker)
       {
          if (val2 != 0) Console.WriteLine (val1 / val2);
          val2 = 0;
         }
       }
     }


     Monitor.Enter (locker);
         try
         {
           if (_val2 != 0) Console.WriteLine (val1 / val2);
           val2 = 0;
         }
     finally { Monitor.Exit (locker); }
                                    
                                    
                                    
                       class OneAtATimePlease
                       {
                         static void Main()
                         {
                           // Naming a Mutex makes it available computer-wide. Use a name that's
                           // unique to your company and application (e.g., include your URL).

                           using (var mutex = new Mutex (false, "howcsharp.com OneAtATimeDemo"))
                           {
                             // Wait a few seconds if contended, in case another instance
                             // of the program is still in the process of shutting down.

                             if (!mutex.WaitOne (TimeSpan.FromSeconds (3), false))
                             {
                               Console.WriteLine ("Another app instance is running. Bye!");
                               return;
                             }
                             RunProgram();
                           }
                         }

                         static void RunProgram()
                         {
                           Console.WriteLine ("Running. Press Enter to exit");
                           Console.ReadLine();
                         }
                       }
                          
         
         
                 class TheClub      // No door lists!
                      {
                        static SemaphoreSlim sem = new SemaphoreSlim (3);    // Capacity of 3

                        static void Main()
                        {
                          for (int i = 1; i <= 5; i++)
                            new Thread (Enter).Start(i);
                        }

                        static void Enter(object id)
                        {
                          Console.WriteLine (id + " wants to enter");
                          sem.Wait();
                          Console.WriteLine (id + " is in!");           // Only three threads
                          Thread.Sleep (1000 * (int) id);               // can be here at
                          Console.WriteLine (id + " is leaving");       // a time.
                          sem.Release();
                        }
                      }
  ```

  * ![image](https://user-images.githubusercontent.com/71544024/152676957-79c0b2ee-90c4-4db0-b3ec-449fea5802d1.png)
  * http://www.howcsharp.com/90/description-of-lock-monitor-mutex-and-semaphore.html
  * https://www.onlinebuff.com/article_understand-monitor-vs-mutex-vs-semaphore-vs-semaphoreslim-onlinebuff_60.html

* Built-in types in C#
    1. Boolean type – Only true or false 
    2. Integral Types - sbyte, byte, short, ushort, int, uint, long, ulong, char
    3. Floating Types – float and double
    4. Decimal Types 
    5. String Type
* Escape sequences
    * Without Verbatim Literal : “C:\\Pragim\\DotNet\\Training\\Csharp” – Less Readable
    * With Verbatim Literal : @“C:\Pragim\DotNet\Training\Csharp” – Better Readable
* String interpolation
    * Console.WriteLine(@"Hello {MyVar}");
    * ![image](https://user-images.githubusercontent.com/71544024/152845325-cb950bf5-af13-4265-8b64-ca0d197125d1.png)
    * ![image](https://user-images.githubusercontent.com/71544024/152845446-3687700d-96d2-47ae-a59b-df3cabab77fc.png)

* Partial Class
    * ![image](https://user-images.githubusercontent.com/71544024/152845605-2beb71b2-3b6b-4700-ab2c-4c8675df50e5.png)
    * ![image](https://user-images.githubusercontent.com/71544024/152845719-5046a0fc-a26d-4441-aa4c-1443096bbbd4.png)
    * ![image](https://user-images.githubusercontent.com/71544024/152845825-13c66655-a147-4fd1-ba1f-e31f6a345070.png)

* Indexers
    * ![image](https://user-images.githubusercontent.com/71544024/152846086-b14f3a33-9bb3-4e5e-8980-930035f30d7b.png)
 
        
* Common Operators
    * Assignment Operator =
    * Arithmetic Operators like +,-,*,/,% 
    * Comparison Operators like ==, !=,>, >=, <, <= 
    * Conditional Operators like &&, ||
    * Ternary Operator ?:
    * Null Coalescing Operator ??
* Nullable Types
    * Value Types  - int, float, double, structs, enums etc
    * Reference Types – Interface, Class, delegates, arrays etc
    * By default value types are non nullable. To make them nullable use ? int i = 0 (i is non nullable, so "i" cannot be set to null, i = null will generate compiler error)
    * int? TicketsOnSale = null; 
    * AvailableTickets = TicketsOnSale ?? 0;
* Conversion / Casting
    * 1. Implicit conversions 
    * 2. Explicit Conversions 
    * Example: Converting an int to a float will not loose any data and no exception will be thrown, hence an implicit conversion can be done. 
    * Where as when converting a float to an int, we loose the fractional part and also a possibility of overflow exception. Hence, in this case an explicit conversion is required. For explicit conversion we can use cast operator or the convert class in c#.
    * //Example : Explicit Conversions : int i = (int)f;
    * 1. If the number is in a string format you have 2 options - Parse() and TryParse() 
    * 2. Parse() method throws an exception if it cannot parse the value, whereas TryParse() returns a bool indicating whether it succeeded or failed.
* Comments:
    * Single line Comments -   //
    * Multi line Comments -   /*  */
    * XML Documentation Comments -   ///
* Switch:
    ```
         Switch(gender)
         {
            case "Male":
                       //Do Someting
                       Break;
            case "Female":
                       //Do Someting
                       Break;
            default:
                       //Not mentioned
                      
         }
         
         //Assign values based on the 
         var value = gender Switch
         {
            "Male" => Function1(),
            "FeMale" => Finction2(),
            _ => Default
         };
         
    ```
* While / DoWhile:
    ```
         While(i <= n)
         {
           //Repeat Loop
         }
         
         Do
         {
           //This will execute atleast once
         }While(i < =n);
    ```
* For / Foreach
    ```
         for(int i=0;i < =n ;i++ )
         {
         }
         
         foreach(var item in items)
         {
         }
    ```
* Methods/Functions
* Method Perameters (Value / Reference /  Out / Parameter Arrays)      
* Method Parameter vs Arguments
   * Please make a note that the method argument and method parameter are sometimes used interchangeably. Parameters refer to the list of variables in a method declaration. Arguments are the actual values that are passed in when the method is invoked. When you invoke a method, the arguments used must match the declaration’s parameters in type and order.
* Instance method vs Static method
   * Staic variables is to store the data once and share across the instances.
   * Static Constructor is used to initiate the static values.
   * Static variables are initialized even before the instance created.
   * Static varibles are initiated only once through static constuctor.
   * We can't reference / use Instance variables / methods inside the static method.
* Oops
   * Inheritence
     * DRY (Do not repeate yourself) - rule
     * Reusable properties to base.
     * Supports only single class inheritance
     * Supports muliple iterface inheritance
     * Base class are initiated even before the child class initiated.
     * Parent cunstuctor will be called before child constructor.
     * Method hiding (Same method in both Parent and clild) class.
     * If Method hiding is intensional then use 'New'
     * ParentClass P = new ParentClass()
       p.Fun() // Parent will be called
       ClildClass c= new ChildClass()
       c.Fun() // Child method
       ((Parent)c).fun()
   * Polymorphism
     * Polymorphism allow us to invoke child class object using base class ref.
     * This is achived using the keywords 'Virtual' in parent and 'Override' in child.
     * 'Virtual' indicates that the method can be overriden in any derived class.
     ![image](https://user-images.githubusercontent.com/71544024/152688460-06f8585b-dc4b-457f-8bd0-098093290bce.png)
   * Encapsulation
     * Exposing the public varibles to external world is bad.
     * We are not sure what they will insert into this variable // ex: empId =-10 .
     * Properties can be called as varibles .
     * public int Id { set { if(value <0 ) {Throw ex}} else { this._id=value; } get { return this._id }}
     * We can do readonly by reading 'set' implementation.
 
   * Method Overloding
     * This can be done based on number , kind (value , ref , out) & type
     * Can't include return type / params keyword / optional parameter
     * Can be done using Private methods / properties (get-set)
     * 
 * Optional Parameters
   * ![image](https://user-images.githubusercontent.com/71544024/152847028-5ed683fc-f469-4116-9e7d-44762ee61ab2.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152847202-6246d728-9e24-4249-a44e-d1c305351eb5.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152847276-a469660c-33a7-42b2-9e2a-3615fa276ee1.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152847465-c7c8eca7-fbb7-4533-a18b-91206f0aeaa3.png)
   
 
 * Structures vs class
   * ![image](https://user-images.githubusercontent.com/71544024/152689601-91697d5d-0a21-42d7-90b1-56678004ed8d.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152689852-28364abd-7e56-45a6-b864-7b9a4b31fb3d.png)
 * Interfaces
   * ![image](https://user-images.githubusercontent.com/71544024/152689969-3f0ff59f-aaec-4336-b566-cdee646c4da7.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152690127-4866b6c2-e920-4f87-a7be-603cc984ac5b.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152690208-fb212f8d-65bc-4d5c-b6e8-52b7e3f9094a.png)
 * Abstract Class
   * ![image](https://user-images.githubusercontent.com/71544024/152690291-90a32e2a-4e4f-4dbf-ae8f-833cadee4022.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152690312-9bd3be5a-9ab9-4afa-b4aa-96045d0eb795.png)
 * Problem with muli-class inheritance
   * ![image](https://user-images.githubusercontent.com/71544024/152690386-c90b359c-2123-43fc-81e9-f94fbb48b9d6.png)
   * Solution
   * ![image](https://user-images.githubusercontent.com/71544024/152690497-98a3749e-62cf-4530-9440-090ae0141803.png)
 
 
 * Delegates
   * ![image](https://user-images.githubusercontent.com/71544024/152833840-94364bce-edc6-4ec9-b3c2-c550d9a71dee.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152835079-5ac342d0-81d2-4fed-8257-6e36b0e05b96.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152852532-cbb6913b-0760-42a9-9048-14e7cd7135b5.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152852718-d1deb0aa-6cd2-4790-8425-1a3d5b8dc936.png) 
   * ![image](https://user-images.githubusercontent.com/71544024/152853067-5562dee9-9ea0-4db3-bf9d-970a47dee4f4.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152853180-3d94e151-e1f4-4626-9052-784d1523c04c.png)

 
 * Exception Handing
   * ![image](https://user-images.githubusercontent.com/71544024/152835641-ac301828-5018-4dd7-b204-cb408e6584de.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152835936-e2e6d38a-1955-4035-abca-f93cb1523d23.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152836375-eea0ab7e-2f62-425a-9684-17e9ee705be7.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152836795-f2103706-fbb4-41e1-819d-b431174a2987.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152837589-39bdcc12-fc21-490b-972b-72bf9c133439.png)
 
 * Enums
   * ![image](https://user-images.githubusercontent.com/71544024/152837853-553af37d-589f-4ab5-a74e-3b9e77da3bfb.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152838186-267994a6-29a7-40f8-9988-9e930e14d3ce.png)

 * Types & Type Members
   * ![image](https://user-images.githubusercontent.com/71544024/152839806-9cdf9ee8-3512-4e77-b030-090dd5249123.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152840023-dbb0f218-fe0b-48dd-86c9-bbfd98066367.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152840131-41d7820d-d62a-408c-b140-cb8de16fc3d7.png)

* Attributes
   * ![image](https://user-images.githubusercontent.com/71544024/152840334-69cbaad0-b2de-4f14-9f1f-2feaee80facf.png)

* Reflection
   * ![image](https://user-images.githubusercontent.com/71544024/152841751-7491cc8f-6c31-41f0-b95e-cee3b7f95da1.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152842554-f5b03aa5-bd1d-4225-a86a-7777b2d726ad.png)

* Async / Await
   * ![image](https://user-images.githubusercontent.com/71544024/152853801-21dec564-c14d-4a0e-a829-e4923b294814.png)
   * ![image](https://user-images.githubusercontent.com/71544024/152854869-818f246b-6071-4a67-b29d-5195389b12f0.png)

  

 

         

                                    
                                    
                                    
                                    
   
  

     


       
      
   



 
