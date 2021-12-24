# Installation & Configuration
* Required Packages (Microsoft.EntityFrameworkCore, Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkcore.Tools)
* Connection String
   ```
    {
      "ConnectionStrings" :
       {
        "MySQLConnection" : "server=localhost;uid=root;pwd=12345;database=EFCore",
        "SQLConnection"   : "Server=(localdb)\\MSSQLLocalDB;Database=EFCore;Trusted_Connection=True;MultipleActiveResultSets=true"
        "SQLConnection_SA": "Server=Server;Initial Catalog=DataBase;Persist Security Info=False;User ID=sa;Password=password"
       }
    } 
    
    //AppSettings.json 
    //Select the property “Copy to output directory” and change the value “copy always” or “Copy if newer”
    
    public class EFContext : DbContext
    {
        public EFContext(DbContextOptions options) : base(options)
        {
        }
        public DbSet<Product> Products { get; set; } 
    }
    
    public static IConfiguration Configuration { get; set; }
    public Startup(IConfiguration config)
    {
            Configuration = config;
    }
    
    public void ConfigureServices(IServiceCollection services)
    {
            services.AddMvc();
 
            //Alternate Way : Configuration.GetSection("ConnectionStrings:MySQLConnection")
            var connectionString = Configuration.GetConnectionString("SQLConnection");
 
            services.AddDbContext<EFContext>(
                options => options.UseSqlServer(connectionString)
            );
    }
    
    //For Console application
    
    var builder = new ConfigurationBuilder();
    builder.AddJsonFile("appsettings.json", optional: false); 
    var configuration = builder.Build(); 
    connectionString = configuration.GetConnectionString("SQLConnection");
    
   ```
* 
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
# DBContext
* The DBContext is heart of the Entity Framework. It is the connection between our entity classes and the database. The DBContext is responsible for the database interactions like querying the database and loading the data into memory as entity. It also tracks the changes made to the entity and persists the changes to the database .
*It is possible to use both the Constructor & OnConfiguring method to configure the DbContext. In such a scenario the OnConfiguring is executed last. Hence any changes applied in the constructor is overwritten.
*The optionsBuilder.IsConfigured returns a boolean value indicating whether any options have been configured.
```
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
        if (!optionsBuilder.IsConfigured)
        {
            optionsBuilder.UseSqlServer("Server=(localdb)\\MSSQLLocalDB;Database=EFCore;Trusted_Connection=True;MultipleActiveResultSets=true");
        }
 
}
```

# Change Tracking
* The entities can be added/deleted or modified. The DBContext’s change tracker keeps track of these operations and sets the EntityState of each entity. This state is then used while saving the entity into the database, by generating the proper insert, alter, delete queries.

# Transaction Management
* By default each SaveChanges are wrapped in a single transaction. You can control the transactions better by using DbContext.Database API. You can begin transaction commit, and rollback transactions

# DbSet
* EF Core scans all the types, which have a DbSet property and includes them in the model. It also looks for the public properties & types and uses it to determine the columns & types. We can also provide extra modeling information using conventions, data annotation attributes, & Fluent API.
* The DBSet Provides methods like Add, Attach, remove, etc on the Entity Types. The Context class maps these operations into a SQL query and runs it against the database using the Database Providers.
* DbSet Implements the IQueryable & IEnumerable interfaces This allows us to query the database using the LINQ query.
* Add(TEntity) / AddAsync(TEntity, CancellationToken) & db.SaveChanges();
* AddRange(TEntityList) / AddRangeAsync(TEntityList, CancellationToken) & db.SaveChanges();
* Attaching Records
* The Attach method attaches the given entity to the context. The records are marked as Unchanged. It means calling savechanges on the context will have no effect on the database
* Attach(TEntity), AttachRange(IEnumerable<TEntity>), AttachRange(TEntity[])
* Finding an Entity
    * Find(Object[]), FindAsync(Object[]), FindAsync(Object[], CancellationToken)  
* RemoveRange(IEnumerable<TEntity>), RemoveRange(TEntity[])
  
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

