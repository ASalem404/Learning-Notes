- differences between Databases:

  - databases have differences. for example,

  - SQLLite have advantages:
  - small footprints --> very lightweight and deosnt have any external dependencies.
  - user friendly --> sometimes describe as "Zero-Configuration". and it doesnt run as server so it doesnt need to stop.
  - Portable --> it doesn't store as seperate files like other databases,but store data in single file anywhere. so it can be shared like media files.
  - but also have disadvantages:

  - limited concurrency --> multiple processes can access databases but one process can change database at any given time.
  - Security --> databases that use server engine provide better security than serverless SQLLite.
    so its better to use SQLLite in some cases such as embedded systems or test DBMS because it has in-memory mode.

  - MySQL have advantages:

  - popularity and ease of use: one of the most world's Databases in the world.
  - Security: support user management and access priviellages
  - Speed: by choosing not to use some features of SQL, my sql users able to prioritize speed.
  - Replication: suppert number of different types of replications

  - MySQL disadvantages:

  - Known limitations: it comes with certain functional limitations. For example, it lacks support for FULL JOIN clauses,
    MySQL doesn’t have some of the functions for modifying dates as Postgres.
  - some features needs pay before use.
    so it's better to use MySQL in case of future growth, but in case of many users will writing data at once another RDBMS like postgres will be better to use.

  - Postgres have advantages:
  - SQL compliance: alot of SQL features with full power are available in Postgres.
  - Open-Source.
  - disadvantages:

  - Memory performance: For every new client connection, PostgreSQL forks a new process. Each new process is allocated about 10MB of memory,
    which can add up quickly for databases with lots of connections. Accordingly, for simple read-heavy operations,
    PostgreSQL is typically less performant than other RDBMSs, like MySQL.
  - Popularity: there are still fewer third-party tools that can help manage a PostgreSQL database.

- DDL: Any action manipulate the database structure (create database, tabel,...)
- DML: Any action manipulate the database tables contents
- DCL: Any action control the database like users permissions

- Charactersets Vs collations

  - Charactersets:
    - Character sets : the language which the content of the tables is written in
    - Collations : the behavior of the character is the same in all the character cases or different
      => a in germany has different behaviors
  - charactersets and collations can be determined at the beginning of the table creation or at the end of each query using keyword COLLATE.

- ## Transaction Lifecycle

  The transaction lifecycle in a Database Management System (DBMS) involves a series of steps that ensure data consistency and integrity while multiple users access and modify the database concurrently. The key phases of a transaction's lifecycle are:

  1. **Begin Transaction:** The first step in a transaction's lifecycle is the initiation.
     When a user or application begins a transaction, the DBMS marks the start of the transaction and records the current state of the database.
     This is often done using a "BEGIN TRANSACTION" or "START TRANSACTION" command.

  2. **Execution:** In this phase, the user or application performs various database operations, such as INSERT, UPDATE, DELETE, or SELECT statements.
     These operations can modify the data in the database.

  3. **Commit or Rollback:** After executing the required operations, the user or application must decide whether to commit the transaction or roll it back.
     Committing means that all the changes made during the transaction become permanent and are saved in the database.
     Rolling back means that all the changes are discarded, and the database is reverted to the state it was in before the transaction started.

  4. **Commit:** If the user or application decides to commit the transaction, the DBMS ensures that all the changes made during the transaction
     are saved in a way that guarantees data consistency and integrity. Once committed, the changes are permanent and visible to other transactions.

  5. **Rollback:** If the user or application decides to rollback the transaction, the DBMS discards all the changes made during the transaction,
     ensuring that the database remains in a consistent state as it was before the transaction started.

  6. **End Transaction:** After committing or rolling back the transaction, the DBMS marks the end of the transaction.
     This releases any locks or resources held by the transaction and frees up the database for other transactions.

  7. **Concurrency Control:** Throughout the transaction lifecycle,
     the DBMS enforces concurrency control to ensure that multiple transactions can run concurrently without causing conflicts.
     Techniques like locking, timestamps, and multiversion concurrency control are used to manage this.

  8. **Isolation Levels:** Different DBMSs offer various isolation levels, such as Read Uncommitted, Read Committed, Repeatable Read, and Serializable,
     which define the visibility of data modifications made by one transaction to other concurrently executing transactions. The choice of isolation level can affect how transactions interact with each other.

  9. **Deadlock Detection and Resolution:** DBMSs implement mechanisms to detect and resolve deadlocks that may occur
     when multiple transactions compete for resources. Deadlocks can disrupt the normal execution of transactions,
     and the DBMS is responsible for managing and resolving them.

  10. **Logging and Recovery:** The DBMS logs all the changes made during a transaction in a transaction log.
      This log is essential for recovering the database in case of system failures.
      It helps the DBMS to bring the database back to a consistent state after a crash or error.

  The transaction lifecycle is crucial for ensuring data integrity, consistency, and reliability in a multi-user DBMS environment.
  It helps manage the interactions between multiple concurrent transactions and ensures that the database remains in a consistent state despite concurrent modifications.

- ## Redo and Transaction Logs

  In a Database Management System (DBMS), particularly in the context of relational database systems like Oracle, you typically encounter two types of logs: Redo Logs and Transaction Logs. These logs play a critical role in ensuring data consistency, durability, and recovery in the event of a system failure. Let's explore each of these types of logs:

  - **Redo Logs:**

    - **Purpose:** Redo Logs, often referred to as Redo Log Files or simply Redo Logs, are a type of log file used primarily in Oracle Database systems.
      Their primary purpose is to record all changes made to the database, such as INSERTs, UPDATEs, DELETEs, and other modifications, in a sequential manner.
    - **Data Changes:** Redo Logs contain a chronological record of the SQL statements or actions performed on the database, but not the actual data changes.
      For example, if an UPDATE statement changes a row's value, the specifics of the change will be logged in the Redo Log but not the row's old and new values.
    - **Recovery:** Redo Logs are crucial for database recovery.
      They allow the DBMS to reapply changes that were made but not yet written to the data files in the event of a system crash or other failure.
    - **Cyclic Usage:** Redo Logs are typically used in a cyclical manner. Once a Redo Log file is filled, the DBMS switches to the next available Redo Log file.
      This process continues in a circular fashion. Archived Redo Logs can be saved for longer-term recovery.

  - **Transaction Logs (Transaction Log Files):**

    - **Purpose:** Transaction Logs, often referred to as Transaction Log Files, are used in various database systems, including Microsoft SQL Server.
      These logs serve a similar purpose to Redo Logs in Oracle but may contain more detailed information depending on the DBMS.
    - **Data Changes:** Transaction Logs record changes made to the database.
      They may include not only the SQL statements or actions but also the specific data changes, including both the original and modified values.
    - **Recovery:** Transaction Logs are essential for database recovery,
      enabling the DBMS to replay transactions and maintain data integrity in case of system crashes or other failures.
    - **Point-in-Time Recovery:** In some DBMSs, transaction logs support point-in-time recovery,
      allowing the database to be restored to a specific moment in time by replaying transactions up to that point.

  While the terminology and implementation details may vary between different database systems,
  the fundamental concept of using logs to record and recover changes remains consistent.
  Both Redo Logs and Transaction Logs play a critical role in ensuring the consistency and durability of the database in the face of failures and for supporting features like replication, backup, and point-in-time recovery.

- ## Indexes

  - indexes are a powerful tool used in the background of a database to speed up querying.
    Indexes power queries by providing a method to quickly lookup the requested data.

    Simply put, an index is a pointer to data in a table. An index in a database is very similar to an index in the back of a book.

  - Syntax

    ```
    CREATE INDEX <index_name>
    ON <table_name> (column1, column2, ...)
    ```

  - Types

    - Clustered Index:

      - A clustered index determines the physical order of the data rows in a table.
        In other words, the data rows in a table are stored on disk in the same order as the clustered index.
      - There can be only one clustered index per table because the actual order of data rows can't be sorted in multiple ways at the same time.
      - It's important to note that not all databases support clustered indexes. They are commonly used in databases like Microsoft SQL Server.
      - The primary key of a table is often used as the clustered index because it enforces a unique constraint and helps to efficiently retrieve rows by that key.

    - Non-Clustered Index:

      - A non-clustered index is a separate data structure that contains a copy of the indexed columns and a reference to the actual data row in the table.
        It doesn't determine the physical order of the data rows.
      - Multiple non-clustered indexes can be created on a single table.
        Each index is like a separate lookup structure, helping to optimize queries that search for data based on the indexed columns.
      - Non-clustered indexes are used to speed up data retrieval for specific queries, such as searching, filtering, and sorting operations.

      - While non-clustered indexes improve query performance, they require additional storage space and maintenance
        because they store a copy of the indexed data and must be updated when data is modified.

- # SQL Tips:

  - derived Column : a new column that is a manipulation of exiting columns in your database
  - when creating database its important to know how data will be stored in.
    this is knowen as Normalization.
    Normalization is part of successful database design.
    withot Normalization database systems can be slow, inaccurate and inefficient.
    goals of Normalization: - arranging data into logical groupings such that each group describes a small part of the whole. - minimizing the amount of duplicate data stored in a database. - rganizing the data such that, when you modify it, you make the change in only one place.
    Sometimes database designers refer to these goals in terms such as DATA INTEGRITY, REFERENTIAL INTEGRITY, or KEYED DATA ACCESS.

  - Foreign Key (FK)
    A foreign key is a column in one table that is a primary key in a different table.

  - in join:
    the best practice in ON statement is to always be foreign key equaling primary key. - does SQL enforce the ON statement be on the primary key equaling the foreign key ?
    It might be nice if SQL enforced JOINs to be PK = FK,
    but the answer is NO, if you want to join company name to the last name of another column, SQL will let you do it!

  - When you query to get rows with null value in any column "= NULL" is not going to work
    because null is not a value, it's a property of the data, it's different from 0 or ' '.
    instead you must use 'IS NULL' or 'IS NOT NULL'.

  - Some SQL statements

    - UNION => Merge results of two queries together.

  - Built-in functions

    - some of these functions (May be differ from database to another)

    - ABS() => returns the absolute value.
    - ASCII() => returns the ASCII representation of the value.
    - CHAR() => returns the character representation of the ASCII value.
    - ADDDATE() => add days to the date
    - DAY() => extract days from the date
    - DAYNAME() => returns the day name
    - MONTH() => extract months from the date
    - MONTHNAME() => returns the month name
    - CURDATE() => returns the current date,
    - CONCAT() => concatenate some of strings together
    - LENTH() => returns the length of the string
    - LOWER() => returns the lowercase version of the string
    - TRIM() => remove the extra spaces or specific character from the end and start of the string
    - ISNULL() => returns true if the value is null- undefined is null, empty string is not null
    - IF(comparason, if True do, if False do)

  - ## Aggregation:

    - aggregators only aggregate vertically - the values of a column.
      If you want to perform a calculation across rows, you would do this with simple arithmetic.

    - COUNT : COUNT does not consider rows that have NULL value.
    - SUM : you can only use SUM on numeric columns. However, SUM will ignore NULL values.
    - MIN - MAX
      Functionally, MIN and MAX are similar to COUNT in that they can be used on non-numerical columns.
      Depending on the column type, MIN will return the lowest number, earliest date, or non-numerical value as early in the alphabet as possible.
      As you might suspect, MAX does the opposite—it returns the highest number, the latest date, or the non-numerical value closest alphabetically to “Z.”
    - AVG
      AVG returns the mean of the data - that is the sum of all of the values in the column divided by the number of values in a column.
      This aggregate function again ignores the NULL values in both the numerator and the denominator.

    - NOTE THAT:
      One quick note that a median might be a more appropriate measure of center for this data,
      but finding the median happens to be a pretty difficult thing to get using SQL alone
    - so difficult that finding a median is occasionally asked as an interview question.
      more than way to find median is on STACKOVERFLOW

    - GROUP BY
      - can be used to aggregate data within subsets of the data.
        For example, grouping for different accounts, different regions, or different sales representatives.
      - Any column in the SELECT statement that is not within an aggregator must be in the GROUP BY clause.
      - The GROUP BY always goes between WHERE and ORDER BY.
      - You can GROUP BY multiple columns at once, as we showed here.
        This is often useful to aggregate across a number of different segments.
        The order of column names in your GROUP BY clause doesn’t matter.
        The order of columns listed in the ORDER BY clause does make a difference.
        You are ordering the columns from left to right.
    - Having
      - HAVING is the “clean” way to filter a query that has been aggregated, but this is also commonly done using a subquery.
        Essentially, any time you want to perform a WHERE on an element of your query that was created by an aggregate,
        you need to use HAVING instead.
      - Having appears after the GROUP BY but before ORDER BY.
    - DATE

      - DATE_TRUNC:
        allows you to truncate your date to a particular part of your date-time column.
        Common trunctions are day, month, and year.
      - DATE_PART: can be useful for pulling a specific portion of a date,
        but notice pulling month or day of the week (dow) means that you are no longer keeping the years in order.
        Rather you are grouping for certain components regardless of which year they belonged in.

    - CASE
      - The CASE statement always goes in the SELECT clause.
      - CASE must include the following components: WHEN, THEN, and END.
        ELSE is an optional component to catch cases that didn’t meet any of the other previous CASE conditions.
      - You can make any conditional statement using any conditional operator (like WHERE) between WHEN and THEN.
        This includes stringing together multiple conditional statements using AND and OR.
      - You can include multiple WHEN statements, as well as an ELSE statement again, to deal with any unaddressed conditions.

  - ## JOIN:

    - ![Alt text](1694185702315.png)
    - SQL joins are used to combine data from two or more tables in a relational database based on a related column between them.
      Joins are fundamental to querying and retrieving information from databases efficiently.
      There are several types of SQL joins, including:

    - INNER JOIN:The INNER JOIN returns only the rows that have matching values in both
      tables.Syntax:SELECT column1, column2, ... FROM table1 INNER JOIN table2 ON table1.column_name = table2.column_name;

    - LEFT (OUTER) JOIN:The LEFT JOIN returns all the rows from the left table and the matched rows from the right table.
      If there is no match, NULL values are returned for columns from the right
      table.Syntax:SELECT column1, column2, ... FROM table1 LEFT JOIN table2 ON table1.column_name = table2.column_name;

    - RIGHT (OUTER) JOIN:The RIGHT JOIN returns all the rows from the right table and the matched rows from the left table.
      If there is no match, NULL values are returned for columns from the left
      table.Syntax:SELECT column1, column2, ... FROM table1 RIGHT JOIN table2 ON table1.column_name = table2.column_name;

    - FULL (OUTER) JOIN:The FULL JOIN returns all rows when there is a match in either the left or right table.
      If there is no match, NULL values are returned for columns from the non-matching
      table.Syntax:SELECT column1, column2, ... FROM table1 FULL JOIN table2 ON table1.column_name = table2.column_name;

    - CROSS JOIN:The CROSS JOIN returns the Cartesian product of two tables,
      meaning it combines every row from the first table with every row from the second table, resulting in a potentially large result
      set.Syntax:SELECT column1, column2, ... FROM table1 CROSS JOIN table2;

    - SELF JOIN:A self join is a join where a table is joined with itself.
      This can be useful when you have hierarchical data or need to compare rows within the same
      table.Syntax:SELECT column1, column2, ... FROM table1 AS t1 INNER JOIN table1 AS t2 ON t1.column_name = t2.column_name;

    - When using SQL joins, it's important to specify the columns that are used for joining
      and to understand the relationships between the tables in your database schema.
      Joining tables efficiently can significantly impact the performance of your SQL queries,
      so it's important to use the appropriate join type for your specific use case.

    - You can replace ON keyword in join by USING keyword in case of the both tables have the same name in the used join column
      ex: ON table1.ID = table2.ID => USING(ID)

  - ## VIEWS

    - A view in SQL is a saved query that acts as a virtual table.
      It does not store any data on its own but instead retrieves data from one or more underlying tables whenever it is queried.
      Views are defined by SQL statements that select specific columns and rows from one or more tables, and this result set is then treated as a new table.

    - View benifits:
      - Simplified Data Access: Views allow you to simplify complex queries by encapsulating them into a single,
        easy-to-understand virtual table. This can greatly improve code readability and maintainability.
      - Data Security: Views can restrict access to sensitive data.
        You can grant users access to a view without giving them direct access to the underlying tables, controlling what data they can see.
      - Abstraction: Views provide a layer of abstraction over the database schema.
        If the underlying tables change, you can modify the view to adapt without affecting the applications that use it.
      - Performance Optimization: In some cases, views can be used to precompute and store intermediate results,
        which can improve query performance by avoiding redundant calculations.

  - DELETE VS TRUNCATE

    - The key point to note here is that if you use 'DELETE' to remove all the rows from the table, and you have an auto-increment column, and the last value was 5.
      After the deletion, if you add a new row to the table, the auto-increment column will start from 6. However, if you use 'TRUNCATE,' it will reset to the beginning and start from 0.

  - Foreign key:

    - A foreign key is a column (or combination of columns) in a table whose values must match values of a column in some other table.
      FOREIGN KEY constraints enforce referential integrity, which essentially says that if column value A refers to column value B, then column value B must exist.
      For example, if the "department" column is a foreign key constraint in the "employees" table and a primary key in the "management" table, then you cannot insert a user into the "employees" table and assign a department that doesn't exist in the "management" table.

  - ## Sub Queries:

    - Whenever we need to use existing tables to create a new table that we then want to query again,
      this is an indication that we will need to use some sort of subquery.
    - if you use SELECT to created a table that you could then query again in the FROM statement, you must alias it.
      However, if you are only returning a single value, you might use that value in a logical statement like WHERE, HAVING, or even SELECT - the value could be nested within a CASE statement you should not include an alias.

    - WITH - Comma Table Expressions (CTE)
      - The WITH statement is often called a Common Table Expression or CTE.
        Though these expressions serve the exact same purpose as subqueries,
        they are more common in practice, as they tend to be cleaner for a future reader to follow the logic.
      - When creating multiple tables using, you add a comma after every table except the last table leading to your final query.
      - The new table name is always aliased using 'table_name AS', which is followed by your query nested between parentheses.
