# Transaction Isolation Levels in Distributed Systems

# Introduction

In distributed systems, transactions ensure that multiple operations are treated as a single atomic unit. This is important because it allows multiple users to access and modify data concurrently without causing conflicts or inconsistencies.

Transaction isolation is the degree to which the effects of one transaction are isolated from the effects of other transactions. In other words, it determines how much a transaction can "see" the impact of other concurrently executing transactions.

# Transaction Isolation Levels

Several different levels of transaction isolation can be achieved in a distributed system:

* **Read Uncommitted**: The read uncommitted isolation level allows a transaction to read data being modified by another transaction that has not yet been committed. This can result in dirty reads, where a transaction reads data later rolled back by another transaction.
    
* **Read Committed**: The read committed isolation level prevents a transaction from reading data being modified by another transaction that has not yet been committed. This means that a transaction can only read data already committed by another transaction.
    
* **Repeatable Read**: The repeatable read isolation level prevents a transaction from reading data being modified by another transaction and prevents other transactions from modifying data that the transaction has read. This means that once a transaction has read a piece of data, that data will not be modified by another transaction until the first transaction is complete.
    
* **Serializable**: The serializable isolation level is the most substantial level of isolation. It prevents a transaction from reading data being modified by another transaction and other transactions from modifying or inserting data that would conflict with the first transaction. This means that all transactions are executed serially as if occurring one after the other.
    

# Trade-offs between isolation and concurrency

When designing a distributed system, carefully considering the desired transaction isolation level is essential. Different isolation levels come with different trade-offs in terms of concurrency and performance. Stronger isolation levels, such as repeatable read and serializable, offer more guarantees about the system's behavior but may result in reduced concurrency or slower performance. Weaker isolation levels, such as read uncommitted, offer fewer guarantees but may result in higher concurrency and better performance.

# Factors to consider when choosing Isolation levels

* **The use case**: Different use cases may require different levels of isolation. For example, a financial system may require strong isolation to ensure the integrity of financial transactions. At the same time, a social media platform may be able to tolerate weaker isolation for certain types of data.
    
* **The system architecture**: The architecture of a distributed system can also influence the appropriate isolation level. For example, systems with high levels of concurrency may tolerate weaker isolation levels, while systems with low levels of concurrency may require more substantial isolation.
    
* **The desired concurrency level**: As mentioned above, stronger isolation levels may result in reduced concurrency, while weaker isolation levels may result in higher concurrency. When choosing an isolation level, it is essential to consider the desired concurrency level.
    

# Isolation Levels in Practice

Here are some examples of how different isolation levels are used in practice:

### **Repeatable Read - MySQL**

MySQL, by default, uses the repeatable read isolation level.

The REPEATABLE READ isolation level in MySQL provides a consistent view of the data within a transaction, meaning that a query executed multiple times within the same transaction will always return the same result, even if other transactions update the data simultaneously. This is achieved by locking the data being read so that other transactions cannot modify it until the current transaction is committed or rolled back.

However, using locks to maintain consistency can also lead to problems such as deadlocks, where multiple transactions are blocked, waiting for locks on the same data, and inability to proceed. To mitigate these issues, other isolation levels, such as READ COMMITTED and SERIALIZABLE can be used to provide different levels of consistency and isolation.

### **Read Committed - PostgreSQL & Oracle & SQL Server**

By default, PostgreSQL, Oracle, and SQL Server use the read committed isolation level. The isolation ensures that transactions only see the changes committed by other transactions before the current transaction begins.

In other words, when a transaction is executing in the READ COMMITTED isolation level, it only has visibility to the data that was committed by other transactions before the current transaction started. Any changes made by other transactions during the execution of the current transaction will not be visible to it until those changes are committed.

This isolation level provides a more up-to-date view of the data but also increases the risk of inconsistencies or dirty reads since transactions may be able to see partially completed changes made by other transactions.

The READ COMMITTED isolation level is a good choice for applications that require a balance between consistency and performance and do not require a strictly consistent view of the data across multiple transactions. In such cases, the risk of inconsistencies or dirty reads is acceptable, and increased performance due to reduced locking is desired.

### **Serializable -** Cockroach DB

Cockroach DB, by default, uses the serializable isolation level. This means that transactions in CockroachDB are executed serially as if they were occurring one after the other. This ensures that all transactions are completely isolated from each other and that no conflicts or inconsistencies can occur.

The serializable isolation level is the strongest level of isolation, and it offers the most guarantees about the behavior of the system. However, it may result in significantly reduced concurrency and slower performance, as all transactions must be executed one after the other. It is often used in systems where data integrity is the highest priority and concurrency is not as important.

# Conclusion

This article discusses the different transaction isolation levels achieved in distributed systems: read uncommitted, read committed, repeatable read, and serializable. We have also discussed the trade-offs in choosing an isolation level and provided examples of how different isolation levels are used in practice. It is essential to carefully consider the desired isolation level when designing a distributed system, as the chosen level can significantly impact the system's behavior and performance.