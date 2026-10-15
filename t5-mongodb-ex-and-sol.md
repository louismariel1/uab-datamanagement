# T5 — MongoDB Hands-On Solutions and Exam Practice

**Topic:** MongoDB — E-Commerce Platform\
**Purpose:** Practical exercise solutions, aggregation pipelines, and exam preparation\
**Environment:** MongoDB Shell (`mongosh`)\
**Suggested GitHub filename:** `t5-mongodb-hands-on-solutions-and-exam-practice.md`

---

# Part 1 — Database Design and Sample Data

## 1\. Create the database

```
use ecommerce
```

MongoDB creates the database when data is first written to it. Selecting a database with `use` alone does not necessarily persist a new database.

## 2–3. Create products and insert at least 11 products

Each product contains `name`, `category`, `price`, `stock`, `brand`, and `rating`.

```
db.products.insertMany([
  {
    name: "Laptop Pro",
    category: "Electronics",
    price: 1299,
    stock: 12,
    brand: "TechNova",
    rating: 4.7
  },
  {
    name: "Smartphone X",
    category: "Electronics",
    price: 899,
    stock: 20,
    brand: "TechNova",
    rating: 4.5
  },
  {
    name: "Wireless Mouse",
    category: "Electronics",
    price: 35,
    stock: 3,
    brand: "ClickPro",
    rating: 4.2
  },
  {
    name: "Gaming Console",
    category: "Gaming",
    price: 499,
    stock: 8,
    brand: "GameBox",
    rating: 4.8
  },
  {
    name: "Gaming Laptop",
    category: "Gaming",
    price: 1599,
    stock: 4,
    brand: "TechNova",
    rating: 4.6
  },
  {
    name: "Gaming Headset",
    category: "Gaming",
    price: 120,
    stock: 15,
    brand: "SoundMax",
    rating: 4.4
  },
  {
    name: "Office Chair",
    category: "Furniture",
    price: 220,
    stock: 7,
    brand: "ComfortCo",
    rating: 4.1
  },
  {
    name: "Desk Lamp",
    category: "Furniture",
    price: 45,
    stock: 2,
    brand: "BrightHome",
    rating: 4.0
  },
  {
    name: "Running Shoes",
    category: "Sports",
    price: 95,
    stock: 18,
    brand: "FastStep",
    rating: 4.3
  },
  {
    name: "Yoga Mat",
    category: "Sports",
    price: 30,
    stock: 25,
    brand: "FlexFit",
    rating: 4.2
  },
  {
    name: "Coffee Machine",
    category: "Home Appliances",
    price: 180,
    stock: 6,
    brand: "BrewMaster",
    rating: 4.6
  }
])
```

Verify the inserted products:

```
db.products.find()
```

**Explanation:** MongoDB stores documents in collections. Unlike relational tables, documents in a collection do not have to share an identical schema, although a consistent schema is beneficial for reliable queries and aggregation.

---

# Part 2 — Basic Product Queries

## 4\. Find all Electronics products

```
db.products.find({
  category: "Electronics"
})
```

**Explanation:** `find()` accepts a filter document. MongoDB returns documents whose `category` field equals `"Electronics"`.

## 5\. Find products with a price greater than €500

```
db.products.find({
  price: { $gt: 500 }
})
```

**Explanation:** `$gt` means _greater than_. Other comparison operators include:

- `$gte`: greater than or equal to.
- `$lt`: less than.
- `$lte`: less than or equal to.
- `$ne`: not equal to.

## 6\. Display only product name and price

```
db.products.find(
  {},
  {
    _id: 0,
    name: 1,
    price: 1
  }
)
```

**Explanation:** The second argument is the projection.

- `name: 1` includes the name.
- `price: 1` includes the price.
- `_id: 0` excludes MongoDB's default identifier field.

In a normal inclusion projection, you cannot mix inclusion and exclusion fields except for `_id`.

## 7\. Increase Gaming product prices by 10%

```
db.products.updateMany(
  { category: "Gaming" },
  { $mul: { price: 1.10 } }
)
```

**Explanation:** `$mul` multiplies the existing value by a specified number. Multiplication by `1.10` increases prices by 10%.

An alternative is:

```
db.products.updateMany(
  { category: "Gaming" },
  { $inc: { price: 0 } }
)
```

The alternative above does not increase prices; it is shown only to emphasize that `$inc` adds a fixed amount rather than applying a percentage. For a percentage increase, use `$mul`.

**Exam tip:** `$set` assigns a value, `$inc` adds a value, and `$mul` multiplies a value.

## 8\. Mark products with stock below 5 as low stock

```
db.products.updateMany(
  { stock: { $lt: 5 } },
  { $set: { lowStock: true } }
)
```

**Explanation:** The filter identifies products with fewer than five units in stock. `$set` adds the `lowStock` field or updates it if it already exists.

To verify:

```
db.products.find(
  { lowStock: true },
  { _id: 0, name: 1, stock: 1, lowStock: 1 }
)
```

Note that this operation marks matching products but does not reset `lowStock` to `false` for products whose stock later increases.

---

# Part 3 — Customer Management

## 9\. Create customers and insert sample documents

```
db.customers.insertMany([
  {
    name: "Alice Martin",
    email: "alice@example.com",
    country: "Spain",
    registrationDate: ISODate("2026-01-15T10:00:00Z"),
    loyaltyPoints: 1500
  },
  {
    name: "Bob Smith",
    email: "bob@example.com",
    country: "France",
    registrationDate: ISODate("2025-06-20T12:00:00Z"),
    loyaltyPoints: 700
  },
  {
    name: "Carlos Garcia",
    email: "carlos@example.com",
    country: "Spain",
    registrationDate: ISODate("2026-03-08T09:30:00Z"),
    loyaltyPoints: 2400
  },
  {
    name: "Diana Lee",
    email: "diana@example.com",
    country: "Germany",
    registrationDate: ISODate("2024-11-11T08:00:00Z"),
    loyaltyPoints: 3200
  },
  {
    name: "Eva Rossi",
    email: "eva@example.com",
    country: "Italy",
    registrationDate: ISODate("2026-07-04T15:00:00Z"),
    loyaltyPoints: 1100
  },
  {
    name: "Frank Wilson",
    email: "frank@example.com",
    country: "Spain",
    registrationDate: ISODate("2025-12-30T17:00:00Z"),
    loyaltyPoints: 400
  }
])
```

**Explanation:** `ISODate()` stores dates as BSON Date values, not ordinary strings. This allows date comparisons, range queries, and date aggregation operators to work correctly.

## 10\. Find customers with more than 1,000 loyalty points

```
db.customers.find({
  loyaltyPoints: { $gt: 1000 }
})
```

**Explanation:** This uses the same comparison operator as the product-price exercise.

## 11\. Mark Spanish customers as premium

```
db.customers.updateMany(
  { country: "Spain" },
  { $set: { premiumCustomer: true } }
)
```

**Explanation:** `updateMany()` updates every matching customer. `$set` creates the field when it does not exist.

## 12\. Find customers who registered during the current year

Use a dynamic date range so the query remains correct in future years.

```
const now = new Date();

const yearStart = new Date(
  Date.UTC(now.getUTCFullYear(), 0, 1)
);

const nextYearStart = new Date(
  Date.UTC(now.getUTCFullYear() + 1, 0, 1)
);

db.customers.find({
  registrationDate: {
    $gte: yearStart,
    $lt: nextYearStart
  }
})
```

**Explanation:**

- `$gte` includes the start of the year.
- `$lt` excludes the beginning of the following year.
- Using a half-open interval avoids end-of-year timestamp problems.

This example uses UTC calendar years. If the business defines its reporting year in a particular local time zone, adapt the boundaries accordingly.

---

# Part 4 — Orders

## 13–14. Create the orders collection and insert sample orders

For this exercise, each order embeds a snapshot of the purchased product's name, category, quantity, and unit price. This allows revenue calculations to use the price at purchase time instead of a product's current price.

The `customerId` references the `_id` of a document in `customers`.

```
const alice = db.customers.findOne({
  email: "alice@example.com"
});

const bob = db.customers.findOne({
  email: "bob@example.com"
});

const carlos = db.customers.findOne({
  email: "carlos@example.com"
});

const diana = db.customers.findOne({
  email: "diana@example.com"
});

const laptop = db.products.findOne({
  name: "Laptop Pro"
});

const phone = db.products.findOne({
  name: "Smartphone X"
});

const mouse = db.products.findOne({
  name: "Wireless Mouse"
});

const consoleProduct = db.products.findOne({
  name: "Gaming Console"
});

const headset = db.products.findOne({
  name: "Gaming Headset"
});

const shoes = db.products.findOne({
  name: "Running Shoes"
});

db.orders.insertMany([
  {
    customerId: alice._id,
    orderDate: ISODate("2026-01-20T11:00:00Z"),
    products: [
      {
        productId: laptop._id,
        name: laptop.name,
        category: laptop.category,
        quantity: 1,
        unitPrice: 1299
      },
      {
        productId: mouse._id,
        name: mouse.name,
        category: mouse.category,
        quantity: 2,
        unitPrice: 35
      }
    ],
    totalAmount: 1369,
    status: "Pending"
  },
  {
    customerId: bob._id,
    orderDate: ISODate("2026-02-05T14:30:00Z"),
    products: [
      {
        productId: phone._id,
        name: phone.name,
        category: phone.category,
        quantity: 1,
        unitPrice: 899
      }
    ],
    totalAmount: 899,
    status: "Shipped"
  },
  {
    customerId: carlos._id,
    orderDate: ISODate("2026-02-15T09:00:00Z"),
    products: [
      {
        productId: consoleProduct._id,
        name: consoleProduct.name,
        category: consoleProduct.category,
        quantity: 1,
        unitPrice: 499
      },
      {
        productId: headset._id,
        name: headset.name,
        category: headset.category,
        quantity: 1,
        unitPrice: 120
      }
    ],
    totalAmount: 619,
    status: "Shipped"
  },
  {
    customerId: diana._id,
    orderDate: ISODate("2026-03-12T16:45:00Z"),
    products: [
      {
        productId: shoes._id,
        name: shoes.name,
        category: shoes.category,
        quantity: 2,
        unitPrice: 95
      }
    ],
    totalAmount: 190,
    status: "Pending"
  }
])
```

**Explanation:**

- `customerId` is a reference to the customer document.
- `products` is an array of embedded documents.
- `quantity` records the number of units purchased.
- `unitPrice` records the price per unit at purchase time.
- `totalAmount` records the order total.
- `status` records the order's current state.

The total for the first order is $1299 + 2 \times 35 = 1369$.

**Data integrity note:** In a production system, validate that `totalAmount` matches the sum of the line items. Do not assume MongoDB automatically enforces this business rule.

## 15\. Find all Pending orders

```
db.orders.find({
  status: "Pending"
})
```

## 16\. Change an order from Pending to Shipped

```
db.orders.updateOne(
  {
    _id: db.orders.findOne({
      status: "Pending"
    })._id,
    status: "Pending"
  },
  {
    $set: { status: "Shipped" }
  }
)
```

The query above updates the first pending order returned by the query. For an explicit, reproducible example, select a specific order instead:

```
const orderToShip = db.orders.findOne({
  customerId: alice._id,
  status: "Pending"
});

if (orderToShip) {
  db.orders.updateOne(
    {
      _id: orderToShip._id,
      status: "Pending"
    },
    {
      $set: { status: "Shipped" }
    }
  );
}
```

**Explanation:** Including the expected previous status in the update filter helps prevent changing an order that has already moved to another status.

## 17\. Find customers who have placed more than five orders

```
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      orderCount: { $sum: 1 }
    }
  },
  {
    $match: {
      orderCount: { $gt: 5 }
    }
  },
  {
    $lookup: {
      from: "customers",
      localField: "_id",
      foreignField: "_id",
      as: "customer"
    }
  },
  {
    $unwind: "$customer"
  },
  {
    $project: {
      _id: 0,
      customerId: "$_id",
      name: "$customer.name",
      email: "$customer.email",
      orderCount: 1
    }
  },
  {
    $sort: {
      orderCount: -1
    }
  }
])
```

**Explanation:**

1. `$group` counts orders for each customer ID.
2. `$match` keeps customers with more than five orders.
3. `$lookup` joins customer information.
4. `$unwind` converts the one-element customer array into an object.
5. `$project` selects the output fields.
6. `$sort` orders the results by order count.

The sample data contains only four orders, so this query will initially return no results. That is expected; insert more orders to test it.

---

# Part 5 — Aggregations and Analytics

For the revenue exercises, use this reporting assumption:

**Recognized sales revenue includes orders whose status is****`Shipped`****or****`Delivered`****.** Pending and cancelled orders are excluded.

This is an example business rule, not a universal accounting definition.

## 18\. Calculate sales revenue by product category

```
db.orders.aggregate([
  {
    $match: {
      status: { $in: ["Shipped", "Delivered"] }
    }
  },
  {
    $unwind: "$products"
  },
  {
    $group: {
      _id: "$products.category",
      totalRevenue: {
        $sum: {
          $multiply: [
            "$products.quantity",
            "$products.unitPrice"
          ]
        }
      }
    }
  },
  {
    $project: {
      _id: 0,
      category: "$_id",
      totalRevenue: 1
    }
  },
  {
    $sort: {
      totalRevenue: -1
    }
  }
])
```

**Explanation:**

- `$match` filters eligible orders.
- `$unwind` produces one document per purchased product line.
- `$multiply` calculates each line's revenue.
- `$group` adds line revenues for each category.
- `$project` renames the grouping key.
- `$sort` places the highest-revenue categories first.

**Why use****`$unwind`****?** Each order can contain multiple products, potentially from different categories. Unwinding makes it possible to calculate revenue separately for each product line.

## 19\. Identify the top five best-selling products

Define “best-selling” as the **largest number of units sold**, rather than the highest revenue.

```
db.orders.aggregate([
  {
    $match: {
      status: { $in: ["Shipped", "Delivered"] }
    }
  },
  {
    $unwind: "$products"
  },
  {
    $group: {
      _id: "$products.productId",
      productName: { $first: "$products.name" },
      unitsSold: {
        $sum: "$products.quantity"
      },
      revenue: {
        $sum: {
          $multiply: [
            "$products.quantity",
            "$products.unitPrice"
          ]
        }
      }
    }
  },
  {
    $sort: {
      unitsSold: -1,
      productName: 1
    }
  },
  {
    $limit: 5
  },
  {
    $project: {
      _id: 0,
      productId: "$_id",
      productName: 1,
      unitsSold: 1,
      revenue: 1
    }
  }
])
```

**Explanation:**

1. Filter eligible orders.
2. Unwind product lines.
3. Group by product ID.
4. Calculate units sold and revenue.
5. Sort by units sold, with product name as a tie-breaker.
6. Keep the first five products.

**Exam distinction:** The best-selling product by quantity may differ from the product generating the highest revenue.

## 20\. Generate a report using one `$facet` aggregation pipeline

Required metrics:

- Total customers.
- Total orders.
- Total sales revenue.
- Average order value.

Because these metrics originate from two collections, begin with the `customers` collection and use `$facet` to calculate the customer and order metrics in parallel.

```
db.customers.aggregate([
  {
    $facet: {
      customerMetrics: [
        {
          $count: "totalCustomers"
        }
      ],

      orderMetrics: [
        {
          $lookup: {
            from: "orders",
            pipeline: [
              {
                $match: {
                  status: {
                    $in: ["Shipped", "Delivered"]
                  }
                }
              }
            ],
            as: "eligibleOrders"
          }
        },
        {
          $unwind: "$eligibleOrders"
        },
        {
          $replaceRoot: {
            newRoot: "$eligibleOrders"
          }
        },
        {
          $group: {
            _id: null,
            totalOrders: { $sum: 1 },
            totalRevenue: { $sum: "$totalAmount" },
            averageOrderValue: { $avg: "$totalAmount" }
          }
        },
        {
          $project: {
            _id: 0,
            totalOrders: 1,
            totalRevenue: 1,
            averageOrderValue: 1
          }
        }
      ]
    }
  },
  {
    $project: {
      _id: 0,
      totalCustomers: {
        $ifNull: [
          {
            $arrayElemAt: [
              "$customerMetrics.totalCustomers",
              0
            ]
          },
          0
        ]
      },
      orderMetrics: {
        $ifNull: [
          {
            $arrayElemAt: [
              "$orderMetrics",
              0
            ]
          },
          {
            totalOrders: 0,
            totalRevenue: 0,
            averageOrderValue: 0
          }
        ]
      }
    }
  }
])
```

**Explanation:**

- `$facet` executes multiple independent sub-pipelines against the same input.
- `customerMetrics` counts the customers.
- `orderMetrics` looks up eligible orders and calculates order count, revenue, and average order value.
- `$ifNull` handles cases where a sub-pipeline returns no document.
- `$arrayElemAt` extracts a value from the arrays produced by `$facet`.

**Important interpretation:** In this solution, `totalOrders` means the number of recognized sales orders, and `averageOrderValue` is calculated over those same orders. If your lecturer expects the total number of all orders regardless of status, remove the status filter in the relevant branch. The total number of customers includes all customer documents, including customers who have not placed an order.

---

# Part 6 — Optional Exercises

## A1. Use `$lookup` to display each order with customer information

```
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer"
    }
  },
  {
    $unwind: {
      path: "$customer",
      preserveNullAndEmptyArrays: true
    }
  },
  {
    $project: {
      _id: 1,
      orderDate: 1,
      products: 1,
      totalAmount: 1,
      status: 1,
      "customer.name": 1,
      "customer.email": 1,
      "customer.country": 1
    }
  }
])
```

**Explanation:** `$lookup` performs a left outer join. It returns matching customer documents in an array. `$unwind` converts the array to an individual customer object. `preserveNullAndEmptyArrays: true` preserves orders that have no matching customer.

## A2. Analyse which products generate the most sales

This query calculates both units sold and revenue for each individual product.

```
db.orders.aggregate([
  {
    $match: {
      status: { $in: ["Shipped", "Delivered"] }
    }
  },
  {
    $unwind: "$products"
  },
  {
    $group: {
      _id: "$products.productId",
      name: { $first: "$products.name" },
      unitsSold: { $sum: "$products.quantity" },
      revenue: {
        $sum: {
          $multiply: [
            "$products.quantity",
            "$products.unitPrice"
          ]
        }
      }
    }
  },
  {
    $sort: {
      revenue: -1
    }
  }
])
```

**Explanation:** The grouping key is the product ID, ensuring that two products with the same name are not accidentally merged.

## A3. Create a compound index for category and price

```
db.products.createIndex({
  category: 1,
  price: 1
})
```

Example query:

```
db.products.find({
  category: "Electronics",
  price: { $gt: 500 }
})
```

**Explanation:**

- `category: 1` indexes category in ascending order.
- `price: 1` indexes price in ascending order.
- Compound index field order matters.

The index is well suited to queries that filter by category and then by a price range. It may also support queries filtering only by category, because category is the leading field. It is not generally as useful for a price-only query without a category condition.

Verify index usage:

```
db.products.find({
  category: "Electronics",
  price: { $gt: 500 }
}).explain("executionStats")
```

Look for an index scan such as `IXSCAN`, rather than assuming an index will always be selected.

## A4. Implement a transaction that creates an order and decreases stock

A transaction can ensure that the order and inventory changes succeed or fail together. MongoDB transactions require a suitable deployment, such as a replica set or supported sharded cluster; a standalone server does not support multi-document transactions.

The example below assumes the `products` collection and the order schema defined above. It creates an order for one product and one unit.

```
const session = db.getMongo().startSession();

try {
  session.withTransaction(() => {
    const txdb = session.getDatabase("ecommerce");

    const product = txdb.products.findOne({
      name: "Wireless Mouse"
    });

    if (!product) {
      throw new Error("Product not found");
    }

    const quantity = 1;

    const stockUpdate = txdb.products.updateOne(
      {
        _id: product._id,
        stock: { $gte: quantity }
      },
      {
        $inc: { stock: -quantity }
      }
    );

    if (stockUpdate.matchedCount !== 1) {
      throw new Error("Insufficient stock");
    }

    const customer = txdb.customers.findOne({
      email: "alice@example.com"
    });

    if (!customer) {
      throw new Error("Customer not found");
    }

    const unitPrice = product.price;

    txdb.orders.insertOne({
      customerId: customer._id,
      orderDate: new Date(),
      products: [
        {
          productId: product._id,
          name: product.name,
          category: product.category,
          quantity: quantity,
          unitPrice: unitPrice
        }
      ],
      totalAmount: quantity * unitPrice,
      status: "Pending"
    });
  });

  print("Transaction completed successfully.");
} catch (error) {
  print("Transaction failed: " + error.message);
} finally {
  session.endSession();
}
```

**Explanation:**

1. Start a session.
2. Begin a transaction using `withTransaction()`.
3. Read the product and customer.
4. Decrease stock only if enough stock exists.
5. Abort the transaction if the stock update matches no document.
6. Insert the order.
7. Commit on success or abort on failure.

**Why use****`stock: { $gte: quantity }`****?** It checks stock as part of the update itself, preventing the application from blindly decrementing inventory below zero.

**Important:** `withTransaction()` may retry the callback after certain transient errors. Avoid non-idempotent external side effects, such as sending a payment or email, directly inside a retryable transaction callback. In production, use appropriate retry handling, idempotency keys, and validation for all purchased items.

## A5. Monthly order count, revenue, and average order value

```
db.orders.aggregate([
  {
    $match: {
      status: { $in: ["Shipped", "Delivered"] }
    }
  },
  {
    $group: {
      _id: {
        year: {
          $year: {
            date: "$orderDate",
            timezone: "UTC"
          }
        },
        month: {
          $month: {
            date: "$orderDate",
            timezone: "UTC"
          }
        }
      },
      numberOfOrders: { $sum: 1 },
      revenue: { $sum: "$totalAmount" },
      averageOrderValue: { $avg: "$totalAmount" }
    }
  },
  {
    $sort: {
      "_id.year": 1,
      "_id.month": 1
    }
  },
  {
    $project: {
      _id: 0,
      year: "$_id.year",
      month: "$_id.month",
      numberOfOrders: 1,
      revenue: 1,
      averageOrderValue: 1
    }
  }
])
```

**Explanation:**

- `$year` and `$month` extract the year and month from `orderDate`.
- `$group` creates one group per year-month combination.
- `$sum: 1` counts orders.
- `$sum: "$totalAmount"` calculates monthly revenue.
- `$avg` calculates average order value.
- `$sort` produces chronological ordering.

**Exam trap:** Group by both year and month. Grouping only by month combines January from different years into a single group.

---

# Part 7 — Potential Exam Practice Questions

These questions are additional practice questions inferred from the exercises. They are not claimed to be actual lecturer-provided exam questions.

## Q1. What is the difference between `find()` and `aggregate()`?

**Answer:** `find()` retrieves documents using filters and projections. `aggregate()` processes documents through a sequence of pipeline stages, enabling joins, grouping, calculations, reshaping, and analytics.

**Explanation:** Use `find()` for straightforward retrieval and `aggregate()` when the query requires multiple processing steps.

## Q2. What does `$gt` do, and how does it differ from `$gte`?

**Answer:** `$gt` matches values strictly greater than the specified value. `$gte` matches values greater than or equal to it.

Example:

```
db.products.find({
  price: { $gte: 500 }
})
```

This includes products priced exactly at 500.

## Q3. What is the difference between `$set`, `$inc`, and `$mul`?

**Answer:**

- `$set` assigns a field a value.
- `$inc` adds a specified amount.
- `$mul` multiplies a numeric field by a specified factor.

Example:

```
db.products.updateOne(
  { name: "Gaming Console" },
  {
    $set: { featured: true },
    $inc: { stock: 2 },
    $mul: { price: 1.10 }
  }
)
```

This marks the product as featured, adds two units to stock, and increases the price by 10%.

## Q4. What is the purpose of `$match` in an aggregation pipeline?

**Answer:** `$match` filters documents according to a condition.

Example:

```
db.orders.aggregate([
  { $match: { status: "Shipped" } }
])
```

**Explanation:** Only shipped orders proceed to later pipeline stages. Placing an appropriate `$match` early can reduce the amount of data processed downstream.

## Q5. What is the purpose of `$group`?

**Answer:** `$group` groups documents by a key and calculates aggregate values.

Example:

```
db.orders.aggregate([
  {
    $group: {
      _id: "$status",
      count: { $sum: 1 }
    }
  }
])
```

This counts orders for each status.

**Explanation:** In `$group`, `_id` defines the grouping key. Use `_id: null` when calculating one aggregate over all input documents.

## Q6. Why is `$unwind` important when analysing order products?

**Answer:** `$unwind` converts an array into separate pipeline documents, one per array element.

**Explanation:** An order containing three product lines becomes three pipeline documents. This makes it possible to calculate product-level quantities and revenue.

Without unwinding, an aggregation grouped by product line cannot straightforwardly treat each array element as an independent document.

## Q7. What is the difference between `$lookup` and `$unwind`?

**Answer:** `$lookup` joins matching documents from another collection and places the matches in an array. `$unwind` expands an array into individual pipeline documents.

**Explanation:** They are often used together when combining an order with customer information, but they serve different purposes.

## Q8. Why should revenue calculations use the unit price recorded in an order?

**Answer:** Because the current product price may change after the order was placed.

**Explanation:** An order should preserve the agreed price at purchase time. Otherwise, historical sales reports can become inaccurate when product prices are updated.

## Q9. Why is a compound index useful?

**Answer:** A compound index can improve queries that filter or sort on multiple fields, provided the query pattern matches the index structure.

Example:

```
db.products.createIndex({
  category: 1,
  price: 1
})
```

**Explanation:** The leading field is important. This index supports category-based queries and can support price-range filtering within a category.

## Q10. What is the difference between embedding and referencing documents?

**Answer:**

- **Embedding:** Store related data inside the same document.
- **Referencing:** Store an identifier pointing to a document in another collection.

**Explanation:** Embedding is useful for related data that is commonly accessed together, such as order line items. Referencing is useful for shared entities, such as customers who can place many orders.

A practical order schema often combines both approaches: reference the customer, embed order items, and preserve the purchased product's name and price as a historical snapshot.

## Q11. Why are transactions useful in e-commerce?

**Answer:** Transactions allow multiple database operations to commit together or abort together.

**Explanation:** Creating an order and reducing inventory are logically related. Without a transaction or another carefully designed consistency mechanism, one operation could succeed while the other fails.

## Q12. Why is storing dates as BSON Date values preferable to storing them as strings?

**Answer:** BSON Date values support proper date comparisons, date-range filtering, and aggregation operators.

**Explanation:** Strings can be sorted lexicographically rather than chronologically if their formats are inconsistent. Proper date values avoid this problem and allow date calculations.

## Q13. How would you find the three highest-revenue categories?

**Answer:**

```
db.orders.aggregate([
  { $match: { status: { $in: ["Shipped", "Delivered"] } } },
  { $unwind: "$products" },
  {
    $group: {
      _id: "$products.category",
      revenue: {
        $sum: {
          $multiply: [
            "$products.quantity",
            "$products.unitPrice"
          ]
        }
      }
    }
  },
  { $sort: { revenue: -1 } },
  { $limit: 3 }
])
```

**Explanation:** Calculate category revenue, sort descending, and limit the output to three categories.

## Q14. What does `$facet` do?

**Answer:** `$facet` runs multiple independent aggregation sub-pipelines over the same input documents and returns their results in separate arrays in one output document.

**Explanation:** It is useful for dashboards that need several related metrics from the same input stream. When metrics originate from different collections, consider how to bring the required data into the pipeline without accidentally duplicating counts.

## Q15. How would you find products that have never been ordered?

**Answer:** Use `$lookup` to join products to order items and retain products with no matching order line.

One approach is:

```
db.products.aggregate([
  {
    $lookup: {
      from: "orders",
      let: { pid: "$_id" },
      pipeline: [
        { $unwind: "$products" },
        {
          $match: {
            $expr: {
              $eq: ["$products.productId", "$$pid"]
            }
          }
        },
        { $limit: 1 }
      ],
      as: "matchingOrders"
    }
  },
  {
    $match: {
      matchingOrders: { $eq: [] }
    }
  },
  {
    $project: {
      _id: 0,
      name: 1,
      category: 1,
      price: 1
    }
  }
])
```

**Explanation:** `$lookup` searches for at least one matching order line. The final `$match` retains products whose joined array is empty.

This query considers all orders, regardless of status. To define “never sold” as “never included in a shipped or delivered order,” add the relevant status filter inside the lookup pipeline.

---

# Part 8 — Final Revision Checklist

Before the exam, make sure you can explain and implement the following without copying the solutions.

- [ ]Create a database and insert documents.
- [ ]Query documents with `find()`.
- [ ]Filter with `$gt`, `$lt`, `$gte`, and `$in`.
- [ ]Select fields using projections.
- [ ]Update documents with `$set`, `$inc`, and `$mul`.
- [ ]Query date ranges using BSON Date values.
- [ ]Reference customers from orders.
- [ ]Aggregate with `$match`, `$group`, `$sort`, and `$limit`.
- [ ]Calculate revenue using `$multiply` and `$sum`.
- [ ]Analyse arrays using `$unwind`.
- [ ]Join collections using `$lookup`.
- [ ]Produce dashboard reports using `$facet`.
- [ ]Create and verify compound indexes.
- [ ]Explain embedding versus referencing.
- [ ]Explain why and when transactions are needed.
- [ ]Identify the difference between unit sales and revenue.
- [ ]Handle empty results and missing joined documents.

---

## Key Takeaways

1. **CRUD operations:** `insertMany()`, `find()`, `updateOne()`, and `updateMany()` are fundamental MongoDB operations.
2. **Aggregation:** A pipeline processes documents through stages to generate analytical results.
3. **Arrays:** `$unwind` is especially important when calculating metrics from individual order items.
4. **Joins:** `$lookup` combines documents across collections.
5. **Indexes:** Compound indexes should reflect actual query patterns and field order.
6. **Transactions:** Use transactions or another robust consistency strategy when order creation and inventory updates must remain consistent.
7. **Data modelling:** Choose embedding and referencing based on access patterns, consistency requirements, and data relationships.
8. **Reporting:** Define which order statuses count as sales before calculating revenue.
9. **Historical accuracy:** Preserve unit prices in order items to ensure that future price changes do not alter historical revenue calculations.
10. **Exam strategy:** Be able to explain why each stage is needed, not just reproduce the query.

---

**Source scope:** This document is based on the supplied MongoDB e-commerce hands-on exercise. The solutions and additional practice questions are educational interpretations of the exercise requirements, not verified lecturer-provided answers or predictions of the actual exam.

GitHub filename: `t5-mongodb-hands-on-solutions-and-exam-practice.md`

One practical suggestion: keep this file separate from your Spark review document. When you have all five chapters ready, submit the compression notes, review questions, and hands-on solutions together so the final practice exam can distinguish lecturer-explicit questions from additional exam-style practice.
