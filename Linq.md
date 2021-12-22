# Linq
* Difference between the extension method and static methos.
  *   Static method can be inside the instance class
  *   this is not mandatory for static method.
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
