Below are the code review comments for simplecache code given in task 2

1. Entries Never Expire or Get Removed. It leads to memory leak. get() checks TTL, but expired entries are never removed from the underlying ConcurrentHashMap.The cache grows unbounded and it Leads to heap pressure, eventual OutOfMemoryError.
2. get() performs no atomic fetch-and-validate. Multiple threads may read the same expired entry concurrently.TTL check is not synchronized with potential overwrites.  Timestamp is checked with a non-atomic read.
3. System.currentTimeMillis() is a calender time Can go backwards or forwards if NTP time sync happens and manual system time change. its unsafe for measurement.
4. Cache value can be mutable. After read the value, it is possible to change the value. So it leads to unpredictable behaviour.
5. We can use caffine Cache machanism to avoid the above pointed disadvandage. I have added the rewritten code as well. 
