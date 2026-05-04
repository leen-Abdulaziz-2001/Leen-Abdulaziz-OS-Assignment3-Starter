# Assignment 3 - Complete Documentation

**Student Name**: [Leen Abdaluziz]  
**Student ID**: [
445052011 ID]  
**Date Submitted**: [Submission Date is 7 may ]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [2 may, 8:00]
**What I implemented**: 
Created shared resources class with Lock and Semaphore
**Challenges encountered**: 
Understanding what should be shared between threads 
**How I solved it**: 
Identified counters and execution log as shared data
**Testing approach**: Ran basic program execution

**Time spent**: 2 hour

---

### Entry 2 - [2 may,8:30]
**What I implemented**: Added ReentrantLock for protecting shared variables

**Challenges encountered**: Confusion between lock and semaphore usage

**How I solved it**: Used lock only for counters and list

**Testing approach**: Checked consistency of values

**Time spent**: 
1 hour
---

### Entry 3 - [2 may, 9:15]
**What I implemented**: Implemented Semaphore for CPU scheduling

**Challenges encountered**: Understanding acquire and release behavior

**How I solved it**: Used try-finally to avoid deadlock

**Testing approach**: Ran multiple processes concurrently

**Time spent**: 1.5 hours

---

### Entry 4 - [2 may, 10:00]
**What I implemented**: Fixed race conditions in shared counters

**Challenges encountered**: Incorrect updates in multi-thread execution

**How I solved it**: Wrapped updates with lock

**Testing approach**: Repeated runs and compared result

**Time spent**: 1 hour

---

### Entry 5 - [3 may, 7:00]
**What I implemented**: Final debugging and synchronization testing.

**Challenges encountered**: InterruptedException handling

**How I solved it**: Used try-catch for acquire and sleep

**Testing approach**: Stress testing with multiple runs.

**Time spent**: 2 hours

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected? The shared resources are contextSwitchCount and totalWaitingTime, which are accessed and modified by multiple threads.
- Why is concurrent access a problem? Concurrent access is a problem because multiple threads may read and update the same variable at the same time without synchronization, leading to inconsistent or lost updates
- What incorrect behavior could occur?Incorrect behavior includes wrong values such as an inaccurate count of context switches or incorrect total waiting time due to lost increments or overwritten updates.

**Your Answer**:

[Your answer here - 4-6 sentences with code examples
Race conditions exist in contextSwitchCount, completedProcessCount, and totalWaitingTime. These variables are shared between multiple threads. Without synchronization, two threads may update values at the same time causing lost updates. For example, two processes incrementing contextSwitchCount simultaneously may overwrite each other, resulting in incorrect final count]

---
Race conditions exist in shared variables such as contextSwitchCount and totalWaitingTime. These variables are accessed and modified by multiple threads simultaneously. Concurrent access is a problem because two threads may read the same value and update it at the same time, causing lost updates. For example contextSwitchCount++;
This operation is not atomic, so two threads incrementing it simultaneously may overwrite each other’s changes. As a result, the final value of contextSwitchCount or totalWaitingTime may be incorrect and lower than expected.

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock is used to ensure mutual exclusion, meaning only one thread can access a critical section at a time. It is mainly used to protect shared data such as contextSwitchCount, completedProcessCount, totalWaitingTime, and the execution log to prevent race conditions and Semaphore is used to control access to a limited resource. In this code, cpuSemaphore is used to limit how many processes can use the CPU simultaneously 
The key difference is that a lock is used for data safety (protecting critical sections), while a semaphore is used for resource management (controlling the number of concurrent threads).
[Your answer here - explain your implementation choices
ReentrantLock is used to protect shared data (counters and execution log) to ensure mutual exclusion. Semaphore is used to control how many processes can access the CPU simultaneously. Lock is for data safety, while semaphore is for resource limiting.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:Deadlock is a situation where two or more threads are blocked forever because each thread is waiting for a resource held by another thread. One prevention technique is using try-finally blocks to ensure that locks and semaphores are always released even if an exception occurs. Another technique is maintaining consistent locking, such as using a single lock (ReentrantLock) for shared resources to avoid circular waiting.
In this code, deadlock is prevented by always releasing the lock and semaphore inside finally blocks. For example lock.lock();
try {
    // critical section
} finally {
    lock.unlock();
}
This ensures that no thread holds a resource indefinitely, preventing deadlock situations

[Your answer here - reference try-finally blocks, lock ordering, etc.
Deadlock is when threads wait forever for resources held by each other. I prevented it using:
try-finally blocks to ensure locks are always released
consistent locking (single lock for shared counters)
Also semaphore is released in finally block to avoid blocking.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.

I used ONE coarse-grained lock for all counters. This simplifies synchronization and avoids complexity. The disadvantage is reduced concurrency because all counters are locked together. However, since updates are fast and small, performance impact is minimal. Fine-grained locking could improve concurrency but increases risk of deadlocks. Since counters are independent, fine-grained locking would theoretically give better performance, but complexity is not worth it here.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables


**Which variables**: contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: shared between threads

**Synchronization mechanism used**: ReentrantLock


**Code snippet**:
lock.lock();
try {
    contextSwitchCount++;
} finally {
    lock.unlock();
}
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: executionLog (ArrayList)

**Why it needs protection**: not thread-safe

**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
lock.lock();
try {
    executionLog.add(message);
} finally {
    lock.unlock();
}
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
limit number of running processes
**Number of permits and why**: 2 (controls concurrency)


**Where implemented**: Process.run

**Code snippet**:SharedResources.cpuSemaphore.acquire();
...
SharedResources.cpuSemaphore.release();
```java
// Paste your implementation here
```

**Effect on program behavior**: only 2 processes run at same time

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running the program multiple times to verify consistent results (multiple runs consistency).
**Testing procedure**: I ran the program 5 times to observe whether the output remains consistent across executions

# Commands used (run the program at least 5 times)
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```

**Results**: The program produced consistent final counts in all runs with no variation in the output.
**Why synchronization is necessary**: Without synchronization, race conditions could occur when multiple threads access shared variables like contextSwitchCount, completedProcessCount, and totalWaitingTime at the same time. This could lead to lost updates or incorrect final values because threads may overwrite each other’s changes. These shared resources need protection to ensure data consistency and correct scheduling result
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: synchronization prevents race conditions

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException
ConcurrentModificationException
**Testing procedure**: 

**Results**: no exception occurred

**What this proves**: executionLog is safely synchronized

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)


**Expected values**: correct totals of processes and waiting time

**Actual values**: matched expected values

**Analysis**: synchronization ensured correctness

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Increased number of processes
**Purpose**: 
To test how the system behaves under higher load and to check if synchronization still works correctly when more threads are competing for resources.
**Results**: The system executed all processes successfully with stable performance and no crashes. The scheduling worked correctly and all processes finished execution

**What I learned**: I learned that the semaphore helps control system load by limiting how many processes can access the CPU at the same time, which prevents overload and keeps the system stable
---

## Part 5: Reflection and Learning
Synchronization ensures safe access to shared resources in multi-threaded programs. Without it, data corruption occurs due to race conditions. Using locks and semaphores helped control both data integrity and CPU usage. I learned how critical thread safety is in operating systems

### What I learned about synchronization:Real world examples:
Banking systems updating balances
Operating system process scheduling
Synchronization can be explained as “many people using one bathroom — lock ensures only one inside at a time”.

[6-8 sentences about key concepts, challenges, insights]

---
Synchronization is important when multiple threads access shared resources at the same time. Without synchronization, race conditions can occur and lead to incorrect results. I learned how tools like ReentrantLock ensure that only one thread enters a critical section at a time. I also understood how semaphores control how many threads can access a resource simultaneously. One challenge is making sure locks are always released to avoid deadlocks. Using try-finally blocks helped guarantee proper release of locks and semaphores. Overall, synchronization improves data consistency and system stability in concurrent programs

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**:
Banking systems updating balances — synchronization ensures that multiple transactions don’t corrupt the account balance.



**Example 2**: Operating system process scheduling — synchronization controls how processes share CPU and system resources efficiently.

---

### How I would explain synchronization to others:
Synchronization is like organizing access to shared things so no one interferes with others. Imagine many people want to use one bathroom — a lock ensures only one person can enter at a time. Without the lock, people would collide and cause problems. In programming, threads are like those people, and shared data is like the bathroom. We use locks to protect data and semaphores to control how many threads can access a resource at once. This keeps everything running smoothly without errors
[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/leen-Abdulaziz-2001/Leen-Abdulaziz-OS-Assignment3-Starter

**Number of commits**:  19 commits

**Commit messages**: 
1. Initial code
2. Added synchronization
3. Fixed race conditions
4. Final testing

---

## Summary

**Total time spent on assignment**: 
4-6 hours
**Key takeaways**: 
1. Locks prevent race conditions
2. Semaphores control concurrency
3. try-finally prevents deadlocks

**Most challenging aspect**: 
understanding threading behavior
**What I'm most proud of**: working scheduler simulation

---

**End of Documentation**
