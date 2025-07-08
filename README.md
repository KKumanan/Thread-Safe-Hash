# Hash Hash Hash
Threadsafe implementations of Hash Tables in C++ using mutex locks.

## Building
```shell
make
```

## Running
```shell
./hashtable-tester -t THREAD_COUNT -s HASH_TABLE_ENTRIES_PER_THREAD
```

## First Implementation
In the `hash_table_v1_add_entry` function, I added mutex locks around the entire body of the 'hash_table_v1_add_entry()' function. This made sure any time a thread was trying to change/add data to the hash table, it would not be at the same time as any other thread. The lock was initialized when the hash table was created and there is one lock for the entire table

### Performance
```shell
./hash-table-tester -t 8 -s 50000
Generation: 56,379 usec
Hash table base: 242,721 usec
  - 0 missing
Hash table v1: 630,766 usec
  - 0 missing
```
Version 1 is a little slower than the base version (2.6x slower). This is a result of the fact only one thread can act at a time, slowing down performance for the entire system.

## Second Implementation
In the `hash_table_v2_add_entry` function, I mutex locks were put on each hash table entry, instead of the entire hash table. The locking and unlocking are in essentially the same location as version 1, but there are locks for each entry. This allows for more threads to go through the entire table at once.

### Performance
```shell
./hash-table-tester -t 8 -s 50000
Generation: 56,379 usec
Hash table base: 242,721 usec
  - 0 missing
Hash table v1: 630,766 usec
  - 0 missing
Hash table v2: 59,583 usec
  - 0 missing
```
Version 2 is  faster than the base version (4.1x slower). This is a result of the fact multiple threads can act on the table at the same time. Only if two threads are trying to access the same bucket, will threads have to wait for one another. This is not a common occurance, speeding up the entire process.


## Cleaning up
```shell
make clean
```
