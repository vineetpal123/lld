#### For reference : https://www.youtube.com/watch?v=D3XhDu--uoI&list=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW&index=17

### Concurrency Control in distributed system | Optimistic & Pessimestic Concurrency lock.

### How you will handle the concurrency in case of book my show , parking lot.

### Scenerio : Many concurrent request tries to book same movie theatre seat.

### CONCURRENCY 
- A situation in which two or more persons access the same records simultaneously is call concurrency.
- Concurrency control involve the synchronization of access to the distributed database, such that 
    integretry of the database is maintained.

if three user is booking same seat at same time whose current status is free , so in normal case 
all three user get same seat and afte that status updated to booked. 

# Solution 1 

### using SYNCHRONIZED for the critical section (logic which will check seat and book seat)

######  SYNCHRONIED internally put locking. so request will go one by one and only user seat booked. But this will not work in distributed system (i.e same service on multiple m/c ).

###### we know 1 process have multiple thread, so if we have 1 process then SYNCHRONIED will works for us.

# Solution 2 
### Using Distributed Concurrency Control.
- Optimistics Concurrency Control (OCC)
- Pessimistics Concurrency Control (PCC)

# But before we learing OCC and PCC , we must know below important things first.
- What is the use of Transaction ?
- What is DB Locking ?
- What are the isolation level present ?

#### 1. What is the use of Transaction ?
    Transaction helps to achieve INTEGRITY. Means it help us to avoid INCONSISTENCY in our database.
    if anyone transaction get failed in a Transaction, it will rollback all the success statement also, so 
    that our database is in consistent state.

#### 2. What is DB Locking ?
    DB Locking, help us to make sure that no other transaction update the locked rows.   

    SHARED LOCK (S) : Generally known as read lock
    EXCLUSIVE LOCK (X) : Generally known as writing lock, 
    
    | LOCKTYPE             | ANOTHER SHARED LOCK  | ANOTHER EXCLUSIVE LOCK |
    | HAVE SHARED LOCK     | YES                  | NO                     |
    | HAVE EXCLUSIVE LOCK  | NO                   | NO                     |

#### 3. What are the Isolation level Present ?
    Event if you have multiple transaction running in parellel, each transaction feels like they are 
    working alone. thats what isolation is. So this I property of ACID tells how much concurreny your system can handle.    
     ---------------------------------------------------------------------------------------------   
    | Isolation level | Dirty Read Possible | Non Repeatable Read Possible | Phantom Read Possible |
    | Read Uncommitted| Yes                 | yes                          | yes                   | 
    | Read Commited   | No                  | yes                          | yes                   |      
    | Repeatable Read | No                  | No                           | yes                   |  
    | Serizable       | No                  | No                           | No                    |
     ----------------------------------------------------------------------------------------------     

    Consistency High -------------> Low 

    Read Uncommitted -> Read Commited -> Repeatable Read -> Serializable

    Read Uncommitted used when only read operation is required , 
    
#### Dirty Read :
    If Transaction A is reading the data , which is writing by Transaction B and not yet even committed.
    If Transaction B does the rollback, then whatever data read by Transaction A is know dirty Read.

#### Non Repeatable Read Possible : 
    If suppose Transaction A , reads the same row several times and there is a chance that it reads different value.

#### Phantom Read Possible : 
    If suppose Transaction A , executes same query several times and there is a chance that the rows
    returned are different.
     -------------------------------------------------------------------------------------------------    
    | ISOLATION LEVEL  | LOCKING STATEGY                                                              |
     -------------------------------------------------------------------------------------------------
    | Read Uncommitted | Read : No lock acquired                                                      |
    |                  | Write : No lock acquired                                                     |
    | Read committed   | Read : Shared lock acquired and Released as soon read is done                |  
    |                  | Write : Exclusive lock acquired and keep till the end of transaction.        |  
    | Repeatable Read  | Read : Shared lock acquired and Released only at the end of transaction      |
    |                  | Write :  Exclusive lock acquired and Released only at the end of transaction |
    | Serializable     | Same as Repeatable read locking strategy + apply range lock and lock is      |
    |                     release at the end of transaction                                           |
     -------------------------------------------------------------------------------------------------   

###    Repeatable Read is the default isolation level for InnoDB.

### SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

    BEGIN TRANSACTION;

    SELECT QUERY;
    UPDATE QUERY:

    If success: 
        COMMIT TRANSACTION;
    else 
        ROLLBACK TRANSACTION;

    END_TRANSACTION;

#### select isolation level based on consistency.

## Distributed Concurrency Control
- Optimistic concurrency control (OCC).
- Pessimistic Concurrnecy control (PCC).

### Optimistic concurrency control (OCC)

| Time| Transaction A                   | Transaction B                  |   DB                             | ------------------------------------------------------------------------------------------------------------
  T1 | BEGIN TRANSACTION                | BEGIN TRANSACTION               |  ID : 1, Status : Free, Version: 1
  T2 | READ Row ID:1 (row version is 1) |READ Row ID:1 (row version is 1) | ID : 1, Status : Free, Version: 1
  T3 | Select for update (version validation happens) |  | ID : 1, Status : Free (Exclusive lock by Txn A), Version: 1
  T4 | Update Row Id: 1, Status: Booked, version: 2 |  | ID : 1, Status : Booked (Exclusive lock by Txn A), Version: 2
  T5 | COMMIT                           |                                 | ID : 1, Status : Booked, Version: 2  
  T6 |       | Select for update (version validation happens)             | ID : 1, Status : Booked (Exclusive lock by Txn B), Version: 2  
  T7 |       | Rollback             | ID : 1, Status : Booked (Exclusive lock by Txn B), Version: 2  

In T7 version is already updated to V2 , version validation failed, because version is already updated with some other transaction, so it will rollback, exclusive lock is removed , after that it will try again.


### Optimistic concurrency control (OCC)
- Isolation leved used above REPEATABLE READ.
- Much Higher concurrnecy.
- No chance of Deadlock.
- In case of conflict , overhead of transaction rollback and retry logic is there.

### Pessimistic concurrency control (PCC)
-  Isolation leved used REPEATABLE READ and SERIALIZABLE.
- Less concurrnecy compared to Optimistic.
- Deadlock is possible , then transactions stuck in deadlock forced to do rollback.
- Putting a long lock, sometime timeout issue comes and need to be done.

In Pessimistic at one time only one transaction is process, and share lock also release at end, 
so update lock only applicable once share lock txn completed.

### Their was one problem in pessimistic is Dead lock. Below is the case of Dead lock.
 ---------------------------------------- 
| Transaction A      |  Transaction B    |
 ----------------------------------------
|    READ A          | READ B            |
|    UPDATE B        | UPDATE A          |
 ----------------------------------------    

So based on isolation level we can select OCC or PCC.

### Approch of pessimistic locking is 2PL (two phase locking)