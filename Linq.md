# Linq
* Difference between the extension method and static methos.

  *   Static method can be inside the instance class
  *   'this' keyword is not mandatory for static method.
  *   Example for Extension method: 
     ```
     public static class Test 
     {
       public static string ReplaceSpaceWithStar(this string input)
       {
          return input.Replace(" ","*");
       }
     } 
     ```
  *  Extension metods can be called as 
    ```
    var input ="My String With Space";
    input =Test.ReplaceSpaceWithStar(input);     
    ```
* Extension Method Categories

  * Aggregate Operators, Grouping Operators, Restriction Operators, Projection Operators, Set Operators, Partitioning Operators, Conversion Operators, Generation Operators, Query Execution, Join Operators, Custom Sequence Operators, Quantifiers Operators, Miscellaneous Operators.
  
  *  Aggregate Operators
  
     *  Min, Max, Sum, Count, Average, Aggregate
     ```
     int[] numbers={1,2,3,4,5};
     
     numbers.Max() //5
     
     numbers.Min() //1
     
     numbers.Sum() //15
     
     numbers.Count() //5
     
     numbers.Average() //3
     
     numbers.Aggregate((a,b) => a * b); // 120
     numbers.Aggregate(10,(a,b) => a * b); // 1200
     
     //For Complex types
     employees.Max(c=>c.Salory)     
     
     ```
     
  *  Restriction Operators
     
     ```
     int[] numbers={1,2,3,4,5};
     numbers.Where(i=> i%2 == 0)
     
     //We can pass 'index' to identify the position
     numbers.Where((i,index)=> i%2 == 0)
     
     ```
     
  *  Projection Operators
     ```
     //Select
     IEnumerable<int> employeelds = Employee.GetA11Employees().Select(emp=>emp.Id); 
     foreach (int id in employeelds) 
     {
       Console.WriteLine(id); 
     }
     
     //SelectMany
     IEnumerable<string> subjects = students.SelectMany(st=>st.Subjects); 
     foreach (string sub in subjects) 
     {
       Console.WriteLine(sub); 
     }
     
     string[] stringArray = 
     {
        "ABC",
        "0123456789"
     }
     
     IEnumerable<char> result = stringArray.SelectMany(s=>s); 
     foreach (char c in result) 
     {
       Console.WriteLine(c); 
     }    
     
     //Output
     A
     B
     C
     0
     1
     2
     3
     4
     5
     
     
     //SelectMany with root value name
     var res = students.SelectMany(s=>s.Subjects,(st,subject) => new { name = st.Name , subject = subject} )
     
     foreach(var v in result)
     {
         Console.WriteLine($"{v.name} ==> {v.subject}");
     }
     
     //With Index
     students.SelectMany(s=>s.Subjects,(st,i,subject) => new { name = st.Name , subject = subject} )
     
     ```
  *  Ordering Operators
     * OrderBy, OrderByDescending, ThenBy, ThenByDescending, Reverse   
     
     ```
     IEnumerable<Student> result = Student.GetAllStudents().OrderBy(s => s.Name);
     IEnumerable<Student> result1 = Student.GetAllStudents().OrderByDescending(s => s.Name);
     foreach (Student student in result)
     {
       Console.WriteLine(student.Name);
     }
     
     //Secondary Sorting
     IEnumerable<Student> result = Student.GetAllStudetns()
                                   .OrderBy(s => s.TotalMarks).ThenBy(s => s.Name).ThenBy(s => s.StudentID);
     foreach (Student student in result)
     {
       Console.WriteLine(student.TotalMarks + "\t" + student.Name + "\t" + student.StudentID);
     }  
     ```
  *  Partitioning Operators
     * Take, Skip, TakeWhile, SkipWhile    
     
     ```
     string[] countries = { "Australia", "Canada", "Germany", "US", "India", "UK", "Italy" };

     IEnumerable<string> result = countries.Take(3);
     
     IEnumerable<string> result1 = countries.Skip(3);
     
     IEnumerable<string> result = countries.TakeWhile(s => s.Length > 2);
     
     IEnumerable<string> result = countries.SkipWhile(s => s.Length > 2);
     
     //Pageing
     int pageSize = 3;
     IEnumerable<Student> result = students.Skip((pageNumber - 1) * pageSize).Take(pageSize);
          
     ```
   *  LINQ query deferred execution
      * Deferred execution (select, where, Take, Skip etc)
      * Immediate execution (count, average, min, max, ToList etc)   

   *  Conversion Operators
      * ToList, ToArray, ToDictionary, ToLookup, Cast, OfType, AsEnumerable , AsQueryable  
     ```
     var res= students.Where(s=>s.Grade == "A").ToList();
     
     var res= students.Where(s=>s.Grade == "A").ToArray();
     
     var res= students.ToDictionary(s=>s.Id);     
            foreach (KeyValuePair<int, Student> kvp in result)
            {
                Console.WriteLine(kvp.Key + "\t" + kvp.Value.Name + "\t" + kvp.Value.TotalMarks);
            }
            
     //Lookup ==> Just like Disctionory but allows duplicate keys 
     // Group employees by City
          var employeesByCity = listEmployees.ToLookup(x => x.City);
          Console.WriteLine("Employees Grouped By City");
          foreach (var kvp in employeesByCity)
            {
                Console.WriteLine(kvp.Key);
                // Lookup employees by City
                foreach (var item in employeesByCity[kvp.Key])
                {
                    Console.WriteLine("\t" + item.Name + "\t" + item.JobTitle + "\t" + item.City);
                }
            }
            
     ArrayList list = new ArrayList();
            list.Add(1);
            list.Add(2);
            list.Add(3);
     IEnumerable<int> result = list.Cast<int>();
     
     //OfType operator will return only elements of the specified type. The other type elements are simply ignored and excluded from the result set.
     //Where as 'Cast' operation will throw exception incase of conversion failure (InvalidCastException).
     
     ArrayList list = new ArrayList();
            list.Add(1);
            list.Add(2);
            list.Add(3);
            list.Add("4");
            list.Add("ABC");

     IEnumerable<int> result = list.OfType<int>();
     
     //When your entities are connected to DB then use IQueryable ==> which help to generate the query
     //AsEnumerable() will breaks the query and gets the results into inmemory.
     //Under System.Linq NameSpace we have two classes 'Enumerable' & 'Queryable'    
     //Queryable deals with Expressions         public static IQueryable<TResult> Select<TSource, TResult>(this IQueryable<TSource> source, Expression<Func<TSource, TResult>> selector);
     //Enumerable deals with delegate functions public static IEnumerable<TResult> Select<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);
     
     ```
  * Grouping Operators
     *  GroupBy
     ```
    employeeGroup = Employee.GetAllEmployees().GroupBy(x => x.Department).OrderBy(c => c.Key)
    .Select(x => new
    {
     Key=x.Key,
     employee = x.OrderBy(c => c.Name)
    });
    
    foreach (var group in employeeGroup)
    {
       Console.WriteLine("{0} - {1}", group.Key, group.Employees.Count());
       Console.WriteLine("----------");
       foreach (var employee in group.Employees)
       {
         Console.WriteLine(employee.Name + "\t" + employee.Department);
       }
       Console.WriteLine(); Console.WriteLine();
    }    
     ```
     *  GroupBy Muliple Keys
    ```
    var employeeGroups = Employee.GetAllEmployees()
                                        .GroupBy(x => new { x.Department, x.Gender })
                                        .OrderBy(g => g.Key.Department).ThenBy(g => g.Key.Gender)
                                        .Select(g => new
                                        {
                                            Dept = g.Key.Department,
                                            Gender = g.Key.Gender,
                                            Employees = g.OrderBy(x => x.Name)
                                        });

     foreach(var group in employeeGroups)
     {
        Console.WriteLine("{0} department {1} employees count = {2}",
        group.Dept, group.Gender, group.Employees.Count());
        Console.WriteLine("--------------------------------------------");
        foreach (var employee in group.Employees)
        {
            Console.WriteLine(employee.Name + "\t" + employee.Gender
            + "\t" + employee.Department);
        }
        Console.WriteLine(); Console.WriteLine();
    }
    
    ```
    
  *   Element Operators
      * First / FirstOrDefault, Last / LastOrDefault, ElementAt / ElementAtOrDefault, Single / SingleOrDefault, DefaultIfEmpty  
      ```
      int[] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
      int result = numbers.First();
      
      //Throws InvalidOperationException.
      int[] numbers = { };
      int result = numbers.First();
      
      int[] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
      int result = numbers.First(x => x % 2 == 0);
      
      int[] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
      int result = numbers.FirstOrDefault(x => x % 2 == 100);
      
      //Last : Very similar to First, except it returns the last element of the sequence.

      //LastOrDefault : Very similar to FirstOrDefault, except it returns the last element of the sequence.
      
      //ElementAt : Returns an element at a specified index. 
      //If the sequence is empty or if the provided index value is out of range, then an ArgumentOutOfRangeException is thrown.
      
      //ElementAtOrDefault : Similar to ElementAt except that this method does not throw an exception, if the sequence is empty or if the provided index value is out of range.         Instead, a default value of the type that is expected is returned.

      //Single() method throws an exception if the sequence is empty or has more than one element.
      
      //SingleOrDefault : Very similar to Single(), except this method does not throw an exception when the sequence is empty or when no element in the sequence satisfies the         given condition. Just like Single(), this method will still throw an exception, if more than one element in the sequence satisfies the given condition.
      
      //DefaultIfEmpty : If the sequence on which this method is called is not empty, then the values of the original sequence are returned.
      
      int[] numbers = { };
      IEnumerable<int> result = numbers.DefaultIfEmpty();  //numbers.DefaultIfEmpty(10);
      foreach (int i in result)
      {
        Console.WriteLine(i);
      }

      Output:
      0  // 10 Incase we used numbers.DefaultIfEmpty(10);
      
      
      ```
      
      ==> Pending <==
      (21 to 30)
      
    
     
     
     
     
     
     
          
  
