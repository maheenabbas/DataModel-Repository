# ghStudentModelsRepository

Following is the list of my assessment of how to transform the eMovies Model into a modernized
eMoviesSqlServer2019 model.

# 1. Conceptual Data Model
CDM shows the critical business objects that are limited to a high-level solution and how these
business objects interact with each other relevant to that solution. It includes:
1. Business Objects
2. Business Definitions
3. Relationships to show how the business objects interact with each other.
In my case, the terms can be explained further using the modernized eMovies:
1. Business Objects:
a. Customer
b. Customer Credit
c. PaymentTransaction
d. Employee
e. Store
f. Movie
g. Movie Copy
h. Movie Rental Record
2. Business Definitions:
a. Customer: A person or organization who rented a Movie.
b. Customer Credit: a financial tool where the Customer can rent a Movie.
c. PaymentTranscation: The amount paid to the Movie Store by the Customer to rent
a Movie using the different payment methods - like Epay, credit card, or store
payment.
d. Employee: A person who works for the Movie Store to give Movies on rent.
e. Store: A place where a person (Customer) can go and rent a Movie.
f. Movie: A recorded video that is rented by a Customer from a Movie Store.
g. Movie Copy: The first copy of a Movie.
h. Movie Rental Record: A record of renting a Movie for some time in exchange for
Payment.
1. Relationships to show how the business objects interact with each other:
a. One Movie can have one or many Movie Copies whereas, one or many Movie
Copies are required for one Movie Rental.
b. One Employee can give one or many Movies for rent.
c. One Customer can have a zero, one, or many forms of Payment whereas, one or
many Payments can be made from a Customer.
d. An Employee can work in one Movie store at a time whereas, a store can have
one or many Employees
e. One or many Movies can be rented from Many Stores.

# 2. Logical Data Model
At the logical level, we are concerned with the following:
1. Entities
2. Domains
3. Attributes and Attribute Types
4. Key Groups
5. Relationships
6. Default Values
How I modernized the model:
1. Using the example of a new Employee entity that I made, I changed the
EmployeeNumber to EmployeeId as it stands for an identifier.
2. I removed the attributes such as salary of the Employee and the Address2 as they were
not required to define the Employee entity.
3. The EmployeeDateOfBirth and EmployeeLastName were added as these are important
information.
4. Each entity should represent a collection of information/attributes related to a business
object that is worth capturing. Example: Employee:
a. EmployeeId
b. Supervisor
c. EmployeeFirstName
d. EmployeeLastName
e. EmployeeAddress
f. EmployeePhone
g. EmployeeSocialSecurity
h. EmployeeHireDate
i. EmployeeDateOfBirth
j. EmployeeEmail
k. StoreId
5. Normalization:
a. I removed the duplicate attributes such as the CustomerNumber from the
Customer entity.
b. Each attribute in my eMovies is a single value and contains one piece of
information i.e. Customer entity only contains information related to a Customer
renting a Movie.
c. Foreign keys were fixed such as, MovieRentalRecord has so many Foreign keys
so I removed SocialSecurityNumber and fixed the EmployeeId Foreign key.
d. Lastly, I split some entities into smaller ones, such as PaymentTransaction to
PaymentCreditCard, PaymentChecking, PaymentElectronic and PaymentCredit.
e. Multivalued attributes were fixed, such as StoreAddress and StoreAddress2. I
removed the StoreAddress2 because each Store entity should have a unique store
address.
f. To resolve the hidden dependency in the Employee entity, I removed the
Supervisor attribute to non-foreign key
g. The Surrogate key was created on different attributes such as
PaymentTransactionId in PaymentTransaction.
h. Lastly, LDM contained names of the attributes with spaces and had no data types.

# 3. Physical Data Model
At the physical level, we are concerned with the following:
1. Tables
2. Columns
a. Column Names
b. Column Data Types
c. Column Constraints
3. Views
4. Indexes
5. Primary Keys
6. Foreign Keys
7. Relationship between tables
8. DeNormalization
How I modernized the model:
1. Table names were defined as fully qualified object names:
a. HumanResources.Employee
b. Location.Store
c. Sales.PaymentCreditCard
d. Inventory.MovieCopy
2. Fully qualified user-defined datatypes or domains were created.
3. To apply naming standard rules, the Pascal case was used.
4. Abbreviations were removed, such as CUST being changed to Sales.Customer and
MO_RENT_REC to Sales.MovieRentalRecord.
5. Underscore and spaces between the words were removed such as
HumanResources.Employee.
6. The names were made readable so that they are easy to use.
7. Dimensional Physical data modeling such as star schema was used. The fact table in our
case was the Sales.PaymentTranscation table. PaymentTransactionId was used as a key
value to provide us the information about the Sales.PaymentCreditCard,
Sales.PaymentChecking, Sales.PaymentElectronic and Sales.PaymentCredit. By doing
so, Sales.PaymentTranscation has a Parent-Child relationship with the other three tables
that makes it a supertype.
8. Column constraints were made to check several columns such as the PaymentAmount.
9. Lastly, dynamic data masking was done on columns like social security numbers and
credit card numbers to limit the sensitive data exposure by masking it to non-privileged
users.
