# Lego_Shop
---

## a)
I start by creating four related tables: `product`, `order`, `kunde`, `transportation`, and `order_detail`.

### Product Table:
- **product_id**: Contains the product ID (auto-incremented).
- **product_name**: Contains the product name (varchar(255)).
- **product_price**: Contains the product price (decimal data type).
- **product_description**: Contains the product description (varchar(255)).

### Customer Table:
- **customer_id**: Contains the customer ID, (auto-incremented)
- **customer_name**: Contains the customer name (varchar(100)).
- **customer_email**: Contains the customer email (varchar(100)).
- **customer_phone**: Contains the customer phone number (varchar(100)).

### Order Table:
- **order_id**: Contains the order ID, (auto-incremented)
- **order_date**: Contains the order date (date data type).
- **order_status**: Contains the order status (varchar(100)).
- **order_total**: Contains the total order amount (integer data type).
- **customer_id**: Contains the customer ID (foreign key).
- **transportation_id**: Contains the transportation ID (foreign key).

### Transportation Table:
- **transportation_id**: Contains the transportation ID, (auto-incremented)
- **transportation_name**: Contains the name of the transportation company (varchar(100)).

### OrderDetail Table:
- **orderdetail_id**: Contains the order detail ID.
- **order_id**: Contains the order ID (foreign key).
- **product_id**: Contains the product ID (foreign key).
- **quantity**: Contains the total quantity of the product ordered (integer data type).
- **price**: Contains the total price for the product (decimal data type).


## Conceptual Design and Relationships:
The goal is to understand whether the relationships are **one-to-many** or **many-to-many**.

In this case, the `Order` table serves as an entity that connects `Product`, `Customer`, and `Transportation`. It is important to understand the relationships between the tables before creating them.

The schema includes **primary keys** and **foreign keys**, making it easier to understand the structure of each table and how they are related.

By properly defining primary keys and foreign keys, we ensure data integrity and enforce relationships across the tables. The `OrderDetail` table is used to link specific products to specific orders, along with their quantities and prices. This ensures that an order can contain multiple products and that each product can be part of multiple orders.

---

# b) Skriv en avancerad SQL-fråga som involverar minst två JOIN-operationer och en sub-fråga. Förklara hur frågan fungerar och varför den är strukturerad på detta sätt.

I want to know which customor have ordered more than 1 product and the total orders per each custormor 

````sql
SELECT K.customer_name, O.order_id, SUM(OD.quality) AS TotalQuantity
FROM kunde K
JOIN `order` O ON K.customer_id = O.customer_id
JOIN order_detail OD ON O.order_id = OD.order_id
GROUP BY K.customer_name, O.order_id
HAVING SUM(OD.quality) > 1; -- you can change the number to get different result. my max quantity was 2 so you cant go higher than that.
````

---

# c)
## Normalization Form

I use **3NF (Third Normal Form)** for the database structure. This ensures there are no transitive dependencies among non-key fields. Separate tables are created to represent each entity, avoiding redundancy and maintaining data integrity.

---

### Unnormalized Form (UNF) (The table are made by chatgbt would be to much work creating 4 of them)

| Order ID | Customer Name | Product Names                | Product Quantities | Order Date   |
|----------|---------------|------------------------------|--------------------|--------------|
| 1        | John Doe      | Lego Star Destroyer          | 1, 2               | 2024-12-01   |
| 2        | Jane Smith    | Lego Death Star              | 1                  | 2024-12-02   |
| 3        | Alice Johnson | Lego Batman 1in8,            | 3, 1               | 2024-12-03   |

---

### After Normalization

#### Customers Table

| Customer ID | Customer Name | Customer Email          | Customer Phone |
|-------------|---------------|-------------------------|----------------|
| 1           | John Doe      | john.doe@example.com    | +123456789     |
| 2           | Jane Smith    | jane.smith@example.com  | +987654321     |
| 3           | Alice Johnson | alice.johnson@example.com | +192837465    |

#### Orders Table

| Order ID | Customer ID | Order Date   | Order Status | Order Total |
|----------|-------------|--------------|--------------|-------------|
| 1        | 1           | 2024-12-01   | Shipped      | 5           |
| 2        | 2           | 2024-12-02   | Pending      | 15          |
| 3        | 3           | 2024-12-03   | Delivered    | 1           |

#### Order Details Table

| OrderDetail ID | Order ID | Product ID | Quantity | Price   |
|----------------|----------|------------|----------|---------|
| 1              | 1        | 1          | 1        | 800.00  |
| 2              | 1        | 2          | 2        | 459.99  |
| 3              | 3        | 3          | 1        | 5000.00 |


#### Products Table

| Product ID | Product Name             | Product Price | Product Description                       |
|------------|--------------------------|---------------|-------------------------------------------|
| 1          | Lego Star Destroyer      | 800.00        | A Lego set with 6000 pieces              |
| 2          | Lego Batman 1in8         | 459.99        | This Lego set can be used to create 8 Batman sets |
| 3          | Lego Death Star          | 5000.00       | This Lego set is from Empire Strikes Back |

---

### Justification for 3NF (This whole part in from chatgbt)

1. **First Normal Form (1NF):**
   - Data is stored in a tabular format with unique rows and no repeating groups.
   - Each cell contains atomic values.

2. **Second Normal Form (2NF):**
   - Each non-key attribute is fully functionally dependent on the primary key.
   - Composite keys are avoided by breaking down many-to-many relationships.

3. **Third Normal Form (3NF):**
   - Non-key attributes are dependent only on the primary key, avoiding transitive dependencies.
   - For example, `Customer Name` and `Product Name` are moved to separate `Customers` and `Products` tables to eliminate redundancy in the `Orders` table.

This design ensures data integrity, eliminates redundancy, and adheres to normalization principles for efficient data management.

---

# Databasdesign och Alternativa Lösningar:

## Föreslå minst två förbättringar till din databasdesign. Motivera varför dessa förbättringar skulle vara fördelaktiga.

1. **Split Address Field in Customer Table**  
   - Divide the address into separate fields, such as street and apartment number, for easier searching.

02. **Add Update Date Field to `OrderDetail`**  
   - Include a field to track the last update date for order status.  
   - Facilitates customer follow-ups by providing clear information on order updates.

## Diskutera minst ett alternativt sätt att strukturera din databas. Jämför för- och nackdelar mellan din ursprungliga design och det alternativa förslaget.

- **Create a Separate `Invoice` Table**  
  - Establish a new table for managing invoices and receipts with a relationship to `Order`.
  - Use **stored procedures** and **transactions** for improved query efficiency and error handling. this way it ehances performance and simplifies query management.
   

---

# Jämförelse mellan Relationsdatabaser och Dokumentdatabaser:

## Hur skulle din databasstruktur se ut om den implementerades i en dokumentdatabas som MongoDB istället för en relationsdatabas?

- Organize data into collections with fields stored as JSON documents.  
- Simplify schema by consolidating related information into single documents.

## Diskutera fördelar och nackdelar med att använda en dokumentdatabas jämfört med en relationsdatabas för din specifika datastruktur.

- **Advantages of Document Databases**:
  - Flexibility to add new fields without restructuring the schema.
  - Eliminates the need for JOINs by storing all relevant data in a single document.

- **Disadvantages of Document Databases**:
- Lack of strict normalization, making it harder to maintain data accuracy and consistency.

---

# Reflektion och Argumentation:

## Reflektera över dina designval i databasstrukturen. Vilka överväganden gjorde du och varför?

- Considered factors such as data integrity, relationships, and update flexibility.
- Chose a relational database structure using ***third normal form (3NF)*** for:
 - Ensuring high data quality.
- Created the `OrderDetail` table to avoid data duplication and optimize search efficiency.

## Argumentera för varför din valda design är lämplig för ändamålet. Inkludera en diskussion om potentiella konsekvenser av dina val.

- **Flexibility**:  
  - Facilitates future updates, such as adding an `Invoice` entity, by integrating new relationships seamlessly.

- **Efficiency**:  
  - Normalization optimizes queries, both simple and complex.  
  ***
---
