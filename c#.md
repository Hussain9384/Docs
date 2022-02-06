# Data Structure
  * Array -> Can't Grow in size
  * ArrayList -> Can grow in size but casting is required to retrive the value (ArrayList a= new ArrayList ; a.Add(0) //Add(object? value)
  * List -> Can grow in size (TypeSafe)
  * Hashtable -> Key - Value pair. Both values are Objects. So casting is required to retreive.
  * Dictionary -> Strongly typed key value pair
  * SortedList -> Sorted generic Generic, Bit slower while inserting.

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
