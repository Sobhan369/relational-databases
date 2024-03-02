---
title: 'Data Storage and Querying'
weight: 7
references:
    videos:
        - youtube:OYx9RUFwP8E
        - youtube:p8khxj6YalI
    links:
        - https://www.geeksforgeeks.org/data-storage-and-querying-in-dbms/
        - https://www.i2tutorials.com/dbms-introduction/dbms-storage-and-querying/
    books:
        - b1:
            title: Linked Data Storing, Querying, and Reasoning
            url: https://www.google.co.in/books/edition/Linked_Data/h79ODwAAQBAJ?hl=en&gbpv=0
        - b2:
            title: RDF Database Systems Triples Storage and SPARQL Query Processing
            url: https://www.google.co.in/books/edition/RDF_Database_Systems/OySOAwAAQBAJ?hl=en&gbpv=0
---

# Data Storage and Querying

A database system is partitioned into modules that deal with each of the re- sponsibilities of the overall system. The functional components of a database system can be broadly divided into the storage manager and the query processor components.

The storage manager is important because databases typically require a large amount of storage space. Corporate databases range in size from hundreds of gigabytes to, for the largest databases, terabytes of data. A gigabyte is approxi- mately 1000 megabytes (actually 1024) (1 billion bytes), and a terabyte is 1 million megabytes (1 trillion bytes). Since the main memory of computers cannot store this much information, the information is stored on disks. Data are moved be- tween disk storage and main memory as needed. Since the movement of data to and from disk is slow relative to the speed of the central processing unit, it is imperative that the database system structure the data so as to minimize the need to move data between disk and main memory.

The query processor is important because it helps the database system to simplify and facilitate access to data. The query processor allows database users to obtain good performance while being able to work at the view level and not be burdened with understanding the physical-level details of the implementation of the system. It is the job of the database system to translate updates and queries written in a nonprocedural language, at the logical level, into an efficient sequence of operations at the physical level.

## Storage Manager

The _storage manager_ is the component of a database system that provides the interface between the low-level data stored in the database and the application programs and queries submitted to the system. The storage manager is respon- sible for the interaction with the file manager. The raw data are stored on the disk using the file system provided by the operating system. The storage man- ager translates the various DML statements into low-level file-system commands.  

Thus, the storage manager is responsible for storing, retrieving, and updating data in the database.

The storage manager components include:

- **Authorization and integrity manager**, which tests for the satisfaction of integrity constraints and checks the authority of users to access data.

- **Transaction manager**, which ensures that the database remains in a consis- tent (correct) state despite system failures, and that concurrent transaction executions proceed without conflicting.

- **File manager**, which manages the allocation of space on disk storage and the data structures used to represent information stored on disk.

- **Buffer manager**, which is responsible for fetching data from disk storage into main memory, and deciding what data to cache in main memory. The buffer manager is a critical part of the database system, since it enables the database to handle data sizes that are much larger than the size of main memory.

The storage manager implements several data structures as part of the phys- ical system implementation:

- **Data files,** which store the database itself.

- **Data dictionary**, which stores metadata about the structure of the database, in particular the schema of the database.

- **Indices**, which can provide fast access to data items. Like the index in this textbook, a database index provides pointers to those data items that hold a particular value. For example, we could use an index to find the instructor record with a particular ID, or all instructor records with a particular name. Hashing is an alternative to indexing that is faster in some but not all cases.

We discuss storage media, file structures, and buffer management in Chapter 10. Methods of accessing data efficiently via indexing or hashing are discussed in Chapter 11.

## The Query Processor

The query processor components include:

- **DDL interpreter**, which interprets DDL statements and records the definitions in the data dictionary.

- **DML compiler**, which translates DML statements in a query language into an evaluation plan consisting of low-level instructions that the query evaluation engine understands.  

A query can usually be translated into any of a number of alternative evaluation plans that all give the same result. The DML compiler also performs **query optimization**; that is, it picks the lowest cost evaluation plan from among the alternatives.

- **Query evaluation engine**, which executes low-level instructions generated by the DML compiler.

Query evaluation is covered in Chapter 12, while the methods by which the query optimizer chooses from among the possible evaluation strategies are discussed in Chapter 13.

