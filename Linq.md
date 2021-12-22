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
          
     ```
          
  
