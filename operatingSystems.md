# Operating systems 

[mcq](https://www.sanfoundry.com/operating-system-questions-answers/)

## Fork 

- well implementing fork in windows is hard, you can use fork provided in cygwin but its slow
- in cpp you need the header `unistd.h`
- On Unix-like systems, the interface defined by unistd.h is typically made up largely of system call wrapper functions such as fork, pipe and I/O primitives (read, write, close, etc.). 
- sys/types. h file defines data types used in system source code. Since some system data types are accessible to user code, they can be used to enhance portability across different machines and operating systems.
- If you only need the OS types, say for the prototypes of your functions, then just #include <sys/types.h>. However if you need the function definitions, then you #include <unistd.h> or any of the other system headers, as needed.

```
#include<iostream>
#include<unistd.h>
#include<sys/types.h>

using namespace std;

int main()
{
  //create a child process
  //thus making 2 processes running at the same time
  int pid;
  if(pid = fork()){
    cout << "child process id " << pid << "\n";//this is running in parent
  }
  else{
    cout << "parent process id " << pid << "\n";//this is running in child and in child fork returns 0
  }

  cout<<"Michael Jackson"<<endl;
  return 0;
}
//the above code runs fine without sys/types.h we are not using the types anywhere
```

- The pid_t data type is a signed integer type which is capable of representing a process ID. In the GNU C Library, this is an int.
- we could possibly get the output in different order in case one process gets to be executed before another

## Multithreading 

- Multithreading support was introduced in C+11. Prior to C++11, we had to use POSIX threads or p threads library in C. While this library did the job the lack of any standard language provided feature-set caused serious portability issues.
- C++ 11 did away with all that and gave us std::thread.
- threads and multi programs are executed like if there is one processor then through context switching concurrent execution will be carried and in shared memory mutli processor environment parallel execution can be achieved 
- [firefox vs chrome implementation of multithreading](https://levelup.gitconnected.com/how-web-browsers-use-processes-and-threads-9f8f8fa23371) basically for every tab chrome opens a new process which maximizes performance but takes a toll on memory and in firefox you have a default of 4 and upto 8 content processes, the tabs will run in these processes and new tabs will run as new threads in these processes 
- like chrome just abuses ram and opens processes for new tabs, extensions , gpu processes 

---

- To compile programs with std::thread support use `g++ -std=c++11 -pthread`
-  To start a thread we simply need to create a new thread object and pass the executing code to be called (i.e, a callable object) into the constructor of the object. Once the object is created a new thread is launched which will execute the code specified in callable.
-  A callable can be either of the three 1) function pointer 2) function object(functor) 3) lambda expression 

```
#include <iostream>
#include <thread>

using namespace std;

struct cool{
  int a;
  cool(int aa): a(aa){

  }
  void operator ()(int x, int y){
    cout << "ha " << x << " wha " << y << a << "\n";
  }
};

void foo(){
  cout << "fuck";
}

int main(){
  auto f = [](int x) -> void{
    cout << x*x << "\n";
  };

  thread obj(foo);
  thread obj1(f, 10);
  thread obj2(cool(99),10, 20 );

  obj.join();
  obj1.join();
  obj2.join();
  return 0;
}
```

- To wait for a thread use the std::thread::join() function. This function makes the current thread wait until the thread identified by *this has finished executing.
- you need to join threads before ending the program or it can error out 
- An initialized thread object represents an active thread of execution; Such a thread object is joinable, and has a unique thread id. (which can be get by threadName.get_id())
- A default-constructed (non-initialized) thread object is not joinable, and its thread id is common for all non-joinable threads.
- A joinable thread becomes not joinable if moved from, or if either join or detach are called on them
- A thread object is joinable if it represents a thread of execution.
- thread objects cannot be copied but can be moved using equal operator 
- detach : Detaches the thread represented by the object from the calling thread, allowing them to execute independently from each other. Both threads continue without blocking nor synchronizing in any way. Note that when either one ends execution, its resources are released. After a call to this function, the thread object becomes non-joinable and can be destroyed safely.
- we saw that if u don't join threads in the end then it throws error but if u just detach the thread then it won't throw error `thread obj1(foo, 10).detach()` or you could seperately detach thread `obj.detach()`

---
- threads have their own registers and stack, stack pointer everything else is shared such as heap, address space , data, code 
- An address space is simply the mapping of logical addresses to specific pieces of physical memory. So when we say that all the threads in a process share the same address space we mean that when accessing a variable foo in global scope all the threads will see the same variable.
- Most modern operating systems have added a notion of thread local storage, which are variables of global scope that are not shared.

--- 

- When a thread exits ideally these resources should be reclaimed by process automatically. But that doesn’t happens always. It depends on which mode thread is running. A Thread can run in two modes i.e. joinable mode and detached mode 
- By default a thread runs in joinable mode. Joinable thread will not release any resource even after the end of thread function until it is joined
- A Detached thread automatically releases it allocated resources on exit. No other thread needs to join it.
- you can get the number of possible hardware thread contexts using `obj.hardware_concurrency()`
