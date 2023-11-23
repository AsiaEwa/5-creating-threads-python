# 5-creating-threads-python
Creating threads.
Threads allow you to create tasks that are executed simultaneously (alternately) within one process.
Threads are also called the "light" version of processes.
The _thread module is the python implementation of the POSIX pthreads library and runs on all platforms compliant with this standard.
### Attention:
Each indent corresponds to exactly one tab or the same number of spaces (according to the standard: 4 spaces), do not alternate spaces with tabs. In the python program, the scope of functions is determined solely on the basis of the indentation position - there are no {} markings and the line-ending character ;, typical of C++.
## 1. Creating threads:
[Listing 1: thread_test1.py]

After running, the _thread module is imported and three functions are defined: parent(), child() and child2().
The parent() function is then called, which creates two new threads and calls the child() and child2() functions on them.
A thread is created and started using the _thread.start_new_thread() function.
The first argument is the name of the function (task) to be performed (without brackets!), the second argument is a comma-separated list of arguments of the called function (in round brackets).
If there are no input arguments, insert empty brackets:
_thread.start_new_thread(child2, ())
The return value of child2 (return 1) is ignored.
Any errors in the operation of the child function do not interrupt the operation of the main thread, but the error content is displayed in the console
Changing the content of the child() function to simulate a runtime error.

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/9eba91bc-9cd9-40af-a903-db0c1516fc04)

[Listing 2: thread_test2.py]

In a python program, the first argument when starting a thread can be given:
1. ordinary function,
2. class instance function,
3. lambda expression.
   
[Listing 3: thread_test3.py]

The Power class defines an instance function of the action() class that raises the variable i to the power of 32. The variable i is supplied when initializing an instance of the obj class: obj = Power(2).
In a lambda expression, a function is called without giving it a formal structure (def).

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/436c02df-d0ab-438e-9507-0fa75872f094)

## 2 - thread communication and synchronization
After running twice:

[Listing 4: thread_test4.py]

The execution order of threads is random.
If we change the line:

time.sleep(6)
on:
time.sleep(3)

the main thread will terminate and some tasks of the subordinate threads will not be performed.
The above procedure is a simple method of thread synchronization, but it is not always possible to be sure that the slave thread will finish its operation within a specified time.

After modification:

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/94d4711d-ed0b-4ff9-8341-46ffd3e3cff9)

Threads have access to all global variables and objects that will be passed as input arguments.
In order to ensure exclusive access to objects shared by threads, "lock" objects are used, i.e. access locks.
For historical reasons, also called mutex (mutual exclusion).
When a lock is applied, exactly one thread at a time has exclusive access to the global resource. (global objects used between acquire() and release().

[Listing 5: thread_test5.py]

A thread can also change the value of a global variable to signal completion.
This is a simple communication mechanism made possible by creating threads within one process.

[Listing 6: thread_test6.py]

The following version of the program changes logical values (flags) instead of checking the state of the object using the locked() function.

[Listing 7: thread_test7.py]

How to modify it for better performance:
1. True while loop causes excessive CPU usage.
Adding a line e.g. time.sleep(0.25) will significantly reduce CPU usage, with a minimal deterioration in program execution time.
2. Passing arguments to the thread function will enable the script interpreter to work faster.
3. The use of the so-called run context manager:

with mutex
replaces two lines of code with
mutex.acquire()
mutex.release()
The use of the with structure also guarantees that the thread will release the resource lock even if an exception occurs in the handled part of the code.

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/990c642d-8799-45af-b8b2-459988d37d19)

[Listing 8: thread_test8.py]

#### A program 
that creates 100 threads.
Each thread is to be assigned a sequence number as a startup argument.
Each thread calculates the value of 2 raised to the power of the thread number (2**i).
**After calculation, the result is to be displayed on the terminal console.
Threads must use lock when accessing the terminal.
Simulate the thread running time for 2 seconds.
all threads were executed before the end of the program - the main thread.
Execution of threads before the end of the program - the last thread 100, i.e. until 99.
In this case, the maximum calculated value is 633825300114114700748351602688 =2^99. The value results from the number of threads used (power).
import_thread,time

stdoutmutex = _thread.allocate_lock()

numthreads = 100

exitmutexes = [_thread.allocate_lock() for i in range(numthreads)]

def counter(myId, count, mutex):

for i in range(count):

time.sleep(2)

with mutex: # with defining

print('[%s] => %S' %(myId, 2**myId))

exitmutexes(myId).acquire() #wait for completion

for i in range(numthreads):

_thread.start_new_thread(counter, (i, 1, stdoutmutex))

while not all(mutex.locked() for mutex in exitmutexes): time.sleep(0.25)

print('main thread terminates')

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/5438a4ae-5ef8-40ed-ab06-2fcc435df51e)


To check, performed similarly in an online environment, but until the end, i.e. until thread 100: Maximum value: 2^100; it results from the number of threads used, i.e. 100.

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/22d6c84c-1ce8-497b-bb9d-3788f9d7c776)

Thread value 99 consistent in both

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/c16b8af9-ea55-4a1a-baa6-85aa4883c374)

![image](https://github.com/AsiaEwa/5-creating-threads-python/assets/101841759/7c6c1368-7ba8-4d6d-a0ce-5cbfc73f2efa)


