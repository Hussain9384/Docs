# Commands
* add-migration RenameTenantName
* update database
* dotnet ef migrations add Tenant


# Table relations
* To work with 'Foreign' key relation we need to identify how we are going to load the data.
* For example if the we want to load 'table B' data in 'table A' and vice versa then we need to maintaine the references in bothe tables.
  ```
  public class Customer
  {
      public int Id {set; get;}
      public int Name {set; get;}
      public ICollection<Order> Orders {set; get;}
  }
  
  public class Order
  {
      public int Id {set; get;}
      public int InvoiceAmount {set; get;}
      
      //Foreign key
      public int CustomerId {set; get;}
      public Customer Customer {set; get;}
  }
  //While inserting the Order information just fill the (Id , InvoiceAmount, CustomerId). 
  //While Retriving the  Order information, we can include the Customer Info by filling 'Customer' property.
  
  ```
# Model Builder usage
  ```
  protected override void OnModelCreating(ModelBuilder modelBuilder)
  {
      //Configuration should be here;
  }
  
  //Seed Data
  modelBuilder.Entity<Country>().HasData(
      new Country(){ Id=1, Name="India", Code="IND" },
      new Country(){ Id=2, Name="SriLanka", Code="SNK" }
  );
  
  //Common Filter
  modelBuilder.Entity<T>().HasQueryFilter(p => p.CountryCode == _currentSession.CountryCode);  // _currentSession runtime value.
  
  //Apply MetaData
  modelBuilder.Entity<Country>(entity =>{
      entity.Property(p=>p.Id).ValueGeneratedOnAdd();
      entity.HasIndex(i => new { i.CountryCode, i.CountryName });
  });
  
  
  ```

