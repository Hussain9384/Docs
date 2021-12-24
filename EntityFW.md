# Installation & Configuration
* Required Packages (Microsoft.EntityFrameworkCore, Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkcore.SqlServer.Design , Microsoft.EntityFrameworkcore.Tools)
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

# Separate Configuration File per entity
```
public class EmployeeConfiguration : IEntityTypeConfiguration<Employee>
{
  public void Configure(EntityTypeBuilder<Employee> builder)
  {
    builder.HasKey(e => e.EmployeeID);
  }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
  
  //Configure domain classes using modelBuilder here   
  modelBuilder.ApplyConfiguration<Employee>(new EmployeeConfiguration());
 
}

//Bulk Register Configurations
//If you have many models, you will end up with many configuration files. 
//You also have to remember to add the configuration file in the OnModelCreating method.

//You can use the ApplyConfigurationsFromAssembly extension method, 
//which uses the reflection to find all the configuration files and registers them automatically.

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   modelBuilder.ApplyConfigurationsFromAssembly(typeof(EmployeeConfiguration).Assembly);
}

```
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
   
   //Ignore DB field
   modelBuilder.Entity<Customer>().Ignore(t => t.LastName);
  
   //Table name map
   modelBuilder.Entity<Employee>()
            .ToTable("mstEmployee")
            .HasKey(e => e.EmployeeID);
   
   modelBuilder.Entity<Employee>()
                .HasKey( o=> new { o.CompanyCode, o.EmployeeCode});
   
  //Common Filter
  modelBuilder.Entity<T>().HasQueryFilter(p => p.CountryCode == _currentSession.CountryCode);  // _currentSession runtime value.
  
  //Apply MetaData
  modelBuilder.Entity<Country>(entity =>{
      entity.Property(p=>p.Id).ValueGeneratedOnAdd();
      entity.HasIndex(i => new { i.CountryCode, i.CountryName });
  });
   
  modelBuilder.Entity<Employee>()
            .Ignore("Age");
   
modelBuilder.Ignore<TempTable>();
   
modelBuilder.Entity<Employee>()
             .HasAlternateKey(e=> e.EmployeeCode);
   
//Update the database and you will see that the UNIQUE Constraint is added for the property EmployeeCode. 
//The Constraint is named as AK_<EntityName>_<PropertyName>
   
modelBuilder.Entity<Employee>()
             .HasAlternateKey(e=> new {e.EmployeeCode,e.EmployeeName});
   
 
   
//Fluent Interface gives two distinct advantageous
//Method chaining
//More readable API Code 

   
  
  ```
* Data Type

Data type	| Mapped	|Null ?
   :---|:---|:---|
string | nvarchar(max)|	Null
decimal|	decimal(18, 2)|	Not Null
decimal?	|decimal(18, 2)	|Null
double|	float	|Not Null
double?|	float	|Null
int|	int|	Not Null
int?|	int|	Null
bool|	bit|	Not Null
bool?|	bit|	Null
DateTime|	datetime|	Not Null
DateTime?|	datetime|	Null
byte[]|	varbinary(max)|	Null
byte|	tinyint|	Not Null
byte?|	tinyint|	Null
uint|	biint|	not null
uint?|	biint ?|	null
short|	smallint	|not null
ushort|	int|	not null
char|	nvarchar(1)|	not null
   
#  Data Annotation   
   * The Data annotation attributes falls into two groups depending functionality provided by them.
   * 1) Data Modeling Attributes, 2) Validation Related Attributes 
   *Data Modelling Attributes 
   * Data Modeling Attributes specify the schema of the database. These attributes are present in the namespace  System.ComponentModel.DataAnnotations.Schema.The following is the list of attributes are present in the namespace.

Attribute|Syntax
   ---|---|
Table Attribute|Table(string name, Properties:[Schema = string])
Column Attribute| [Column (string name, Properties:[Order = int],[TypeName = string])
Column Attribute| [Column (string name)] 
Key Attribute| [Key] (Identity seed & Identity Increased =1)
Key Attribute| [Key, DatabaseGenerated(DatabaseGeneratedOption.None)] (Identity seed & Identity Increased =0)
Key Attribute|[Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
Composite Primary Key|[Key] [Column(Order = 1)]  [Key][Column(Order = 2)]

* ConcurrencyCheck  
   * Concurrency Check attribute EF Core is used to handle conflicts that result when multiple users are updating (or deleting) the table at the same time. You can add the ConcurrencyCheck attribute on any property, which you want to participate in the Concurrency Check. 
   * Assume that two users simultaneously query for same data to edit from the Employee Table. One of the users saves his changes. Now, the other user is now looking at the data, which is invalid. If he also modifies the data and saves it, it will overwrite the first user’s changes. What if both users save the data at the same time. We never know which data gets saved.

   * We use the Concurrency check precisely to avoid such situations. To do that we include additional fields in the where clause apart from the primary key. For Example, by including the name field in the where clause, we are ensuring that the value in the name field has not changed since we last queried it. If someone has changed the field, then the where clause fails the Entity Framework raises the exception.
   
```
    public class Employee
    {
        public int EmployeeID { get; set; }
        [ConcurrencyCheck]
        public string Name { get; set; }
        public string Address { get; set; }
    }
   
```
   * In the above example, We decorate the Name Property with the ConcurrencyCheck attribute. When Entity Framework generates an update or delete statement, it always includes the Name column in where clause.

This attribute does not affect the database mapping in any way. This attribute is similar to timestamp attribute.

You can apply ConcurrencyCheck Attribute on any number of properties.
There is no restriction on a data type for this attribute

* Timestamp Attribute
   *  Using Timestamp Attribute in Entity Framework Core is a way to handle the Concurrency issues. The Concurrency issue arises when multiple users attempt to update/delete the same row at the same time.  This issue can be handled either by using the Timestamp column or ConcurrencyCheck attribute on the property. Timestamp columns are the preferred way of using for concurrency check.
   * You can apply Timestamp attribute to any byte array column as shown in the entity model below. The Attribute is applied to RowID Property. The entity framework automatically adds the TimeStamp columns in update/delete queries.  This attribute resides in the namespace system.componentmodel.dataannotations.schema
   ```
    public class Employee
    {
        public int EmployeeID { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        [Timestamp]
        public byte[] RowID { get; set; }
     }
   ```
   *  We can apply Timestamp attribute to only one property in the domain model.
The data type of the Timestamp must be a byte array.
This attribute affects the database as it creates the column with datatype rowversion. byte[]  array without Timestamp creates the varninary(max) column
Timestamp columns are the preferred way of using for concurrency check.
   
*  DatabaseGenerated
      * Computed, Identity, None
      * The Computed option specifies that the property's value will be generated by the database when the value is first saved, and subsequently regenerated every time the value is updated. The practical effect of this is that Entity Framework will not include the property in INSERT or UPDATE statements, but will obtain the computed value from the database on retrieval.
   ```
   public class Contact
   {
    public int Id { get; set; }
    public string FullName { get; set; }
    public string Email { get; set; } 
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime LastAccessed { get; set; }
   }
   ```
      * Foreign Key [ForeignKey("Department")]
   ```
   //Approach 1
   public class Employee
   {
      public int EmployeeID { get; set; }
      public string EmployeeName { get; set; }
 
      [ForeignKey("Department")]
      public int DeptID { get; set; }
       
      //Navigation property
      public Department Department { get; set; }
   }
   
   //Approach 2
   public class Employee
    {
        public int EmployeeID { get; set; }
        public string EmployeeName { get; set; }
        
        public int DeptID { get; set; }
 
        //Navigation property
        [ForeignKey("DeptID")]
        public Department Department { get; set; }
    }
   
    public class Department
    {
        public int DepartmentID { get; set; }
        public string DepartmentName { get; set; }
 
        //Navigation property
        public ICollection<Employee> Employeees { get; set; }
    }
   
   //Approach 3
   public class Department
    {
        
        public int DepartmentID { get; set; }
        public string DepartmentName { get; set; }
 
        //Navigation property
        [ForeignKey("DeptID")]
        public ICollection<Employee> Employeees { get; set; }
    }
   
  [MaxLength(50),MinLength(10)]
  public string Name { get; set; }
   
  [MaxLength(50,ErrorMessage="Name cannot be greater than 50")]
  public string Name { get; set; }
 
  [MinLength(10, ErrorMessage = "Name cannot be less than 10")]
  public string Address { get; set; }
   
  [StringLength(50, MinimumLength = 10, ErrorMessage="Remark must have min length of 10 and max Length of 50")]
public string Remarks { get; set; }
   
   [NotMapped] //Will not generate the column in DB
  public int Age { get; set; }
   
   [Required] // Generate the column with not null property in DB
  public string Name {get: set;} 
   ```
   *  Multiple Relations
      *  The employee and department is a single relationship. What if the employee belongs to multiple departments ?. Let us take the example of flight & airports. Flight departing from one airport and arrives at another. So the flight has multiple relationships with the Airport
      *The DepartngFlights property must map to DepartureAirport property in the airport model and ArrivingFlights property must map to ArrivalAirport property.

      * The EF Core, throws the following error, when we run the migrations

      * Unable to determine the relationship represented by navigation `Airport.DepartingFlights’ of type ‘ICollection’. Either manually configure the relationship, or ignore this property using the ‘[NotMapped]’ attribute or by using ‘EntityTypeBuilder.Ignore’ in ‘OnModelCreating’.
   
```
   public class Flight
    {
        public int FlightID { get; set; }
        public string Name { get; set; }
        public Airport DepartureAirport { get; set; }
        public Airport ArrivalAirport { get; set; }
    }
 
    public class Airport
    {
        public int AirportID { get; set; }
        public string Name { get; set; }
        public virtual ICollection<Flight> DepartingFlights { get; set; }
        public virtual ICollection<Flight> ArrivingFlights { get; set; }
    }
//Solution 1   
![image](https://user-images.githubusercontent.com/71544024/147359853-7fa4ab06-8e36-4326-b3a3-de4ce07be0e3.png)

```
//Solution 2
      public class Flight
      {
        public int FlightID { get; set; }
        public string Name { get; set; }
        [InverseProperty("DepartingFlights")]
        public Airport DepartureAirport { get; set; }
        [InverseProperty("ArrivingFlights")]
        public Airport ArrivalAirport { get; set; }
      }   
```
   
* There are several methods available in EF Core Fluent API. These methods broadly classified into the three categories

      * Model wide configuration (database)
      * Entity Configuration (table)
      * Property configuration
   * The ModelBuilder class exposes several methods to configure the model. Some of the important methods are listed below   
   
* Model-wide configuration   
Method|Description
---|---|
HasDefaultSchema|Configures the default schema that database objects should be created in, if no schema is explicitly configured.
RegisterEntityType|Registers an entity type as part of the model
HasAnnotation|Adds or updates an annotation on the model. If an annotation with the key specified in annotation already exists its value will be updated.
HasChangeTrackingStrategy|Configures the default ChangeTrackingStrategy to be used for this model. This strategy indicates how the context detects changes to properties for an instance of an entity type.
Ignore|Excludes the given entity type from the model. This method is typically used to remove types from the model that were added by convention.
HasDbFunction|Configures a database function when targeting a relational database.
HasSequence|Configures a database sequence when targeting a relational database.

* Entity Configuration
   
Method|Description
---|---|
Ignore|Exclude the Propery from the Model.
HasColumnName|Configures database column name of the property
HasColumnType|Configures the database column data type of the property
HasDefaultValue|Configures the default value for the column that the property maps to when targeting a relational database.
HasComputedColumnSql|Configures the property to map to a computed column when targeting a relational database.
HasField|Specifies the backing field to be used with a property.
HasMaxLength|Specifies the maximum length of the property.
IsConcurrencyToken|Enables the property to be used in an optimistic concurrency updates
IsFixedLength|Configures the property to be fixed length. Use HasMaxLength to set the length that the property is fixed to.
IsMaxLength|Configures the property to allow the maximum length supported by the database provider
IsReguired|Specifies the database column as non-nullable.
IsUnicode|Configures the property to support Unicode string content
ValueGeneratedNever|Configures a property to never have a value generated when an instance of this entity type is saved.
ValueGeneratedOnAdd|Configures a property to have a value generated only when saving a new entity, unless a non-null, non-temporary value has been set, in which case the set value will be saved instead. The value may be generated by a client-side value generator or may be generated by the database as part of saving the entity.
ValueGeneratedOnAddOrUpdate|Configures a property to have a value generated when saving a new or existing entity.
ValueGeneratedOnUpdate|Configures a property to have a value generated when saving an existing entity.
   
* Property Configuration

Method|Description
   ---|---|
Ignore|Exclude the Propery from the Model.
HasColumnName|Configures database column name of the property
HasColumnType|Configures the database column data type of the property
HasDefaultValue|Configures the default value for the column that the property maps to when targeting a relational database.
HasComputedColumnSql|Configures the property to map to a computed column when targeting a relational database.
HasField|Specifies the backing field to be used with a property.
HasMaxLength|Specifies the maximum length of the property.
IsConcurrencyToken|Enables the property to be used in an optimistic concurrency updates
IsFixedLength|Configures the property to be fixed length. Use HasMaxLength to set the length that the property is fixed to.
IsMaxLength|Configures the property to allow the maximum length supported by the database provider
IsReguired|Specifies the database column as non-nullable.
IsUnicode|Configures the property to support Unicode string content
ValueGeneratedNever|Configures a property to never have a value generated when an instance of this entity type is saved.
ValueGeneratedOnAdd|Configures a property to have a value generated only when saving a new entity, unless a non-null, non-temporary value has been set, in which case the set value will be saved instead. The value may be generated by a client-side value generator or may be generated by the database as part of saving the entity.
ValueGeneratedOnAddOrUpdate|Configures a property to have a value generated when saving a new or existing entity.
ValueGeneratedOnUpdate	Configures a property to have a value generated when saving an existing entity.

```

```



   
   
      
   
 
   

   



   
   

   
   
  

