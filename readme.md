# Database Normalization Assignment

## Overview
This assignment demonstrates database normalization concepts, specifically **First Normal Form (1NF)** and **Second Normal Form (2NF)**. The goal is to reduce redundancy, remove partial dependencies, and ensure proper database structure.

## Assignment Questions and Solutions

### Question 1: Achieving 1NF 🛠️
**Task:** Transform the ProductDetail table to 1NF by ensuring each row contains a single product.

**SQL Query:**
```sql
CREATE TABLE ProductDetail_1NF (
    OrderID INT,
    CustomerName VARCHAR(100),
    Product VARCHAR(100)
);

INSERT INTO ProductDetail_1NF (OrderID, CustomerName, Product) VALUES
(101, 'John Doe', 'Laptop'),
(101, 'John Doe', 'Mouse'),
(102, 'Jane Smith', 'Tablet'),
(102, 'Jane Smith', 'Keyboard'),
(102, 'Jane Smith', 'Mouse'),
(103, 'Emily Clark', 'Phone');
```

### Question 2: Achieving 2NF 🧩
**Task:** Remove partial dependencies from OrderDetails table.

**SQL Query:**
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);

INSERT INTO Orders (OrderID, CustomerName) VALUES
(101, 'John Doe'),
(102, 'Jane Smith'),
(103, 'Emily Clark');

CREATE TABLE OrderDetails_2NF (
    OrderID INT,
    Product VARCHAR(100),
    Quantity INT,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

INSERT INTO OrderDetails_2NF (OrderID, Product, Quantity) VALUES
(101, 'Laptop', 2),
(101, 'Mouse', 1),
(102, 'Tablet', 3),
(102, 'Keyboard', 1),
(102, 'Mouse', 2),
(103, 'Phone', 1);
```

## Submission Instructions
- Save all queries in `answers.sql`.
- Test queries in MySQL Workbench or another SQL environment.
- Submit both `answers.sql` and `README.md`.

---

## GitHub Actions CI/CD Workflow
To validate SQL syntax automatically, create `.github/workflows/sql-validation.yml`:

```yaml
name: SQL Validation

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  sql-lint:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install sqlfluff
        run: |
          python -m pip install --upgrade pip
          pip install sqlfluff

      - name: Lint SQL files
        run: |
          sqlfluff lint answers.sql
```
This workflow runs **sqlfluff** on `answers.sql` whenever code is pushed or a p