**Operating Systems**

- Basics:

  - In the modern CPUs, what's the meaning of cores and threads ?? Read https://www.daniloaz.com/en/differences-between-physical-cpu-vs-logical-cpu-vs-core-vs-thread-vs-socket/  
  - CPU doesn't do IO. CPU runs another process while the process that started IO works on IO operations till the IO event is done

--------------------------------------------------------------------------------
- Mechanisms vs Policies in OS:
  
  - Mechanisms tackle about how to do something and Policies tackles about which to do something. (In an LRU Cache, removing the least recently used is the eviction policy (This is basically which)). Mechanisms implement on how to tackle (in the example of LRU Cache, The implementation of removing the Least Recently used is gonna be called as a mechanism)
  
  - Decisions are gonna be in Policies where as their implementations at a low level are in mechanisms

--------------------------------------------------------------------------------
- CPU Virtualization
  - happens with TimeSharing : (All the processes (instances of a program is called as a process) are fooled by the OS by making them feel that each of them have a CPU all the time). 
  - One example of implementing timesharing is via Context Switching btw the processes/users. Context Switch is basically a mechanism here and Time Sharing can be achieved with Context Switching. 
  - Which User/Process to switch (selection of the process) is gonna be done by a policy (It's called as a scheduling policy in OS). It's a decision making process here. (For example, given a number of possible programs to run on a CPU, which program should the OS run)
    - The scheduling Policy likely uses historical information (e.g., which program has run more over the last minute?), workload knowledge (e.g., what types of programs are run), and performance metrics (e.g., is the system optimizing for interactive performance, or throughput?) to make its decision.
________________________________________________________________________________
- Memory Virtualization:
  - TBD
---------------------------------------------------------------------------------

- Processes:
  - The executable of a program (A python program with an interpreter or a c++ program with g++ compiler) when executed will create a process. It's an abstraction provided by the OS
  - The machine state of a process contains what the process reads (reading a variable), writes (creating dynamic objects) and updates (updating a variable)
  - The above things are basically instructions and these will be loaded into the memory
    - Eager Loading (Loading all the instructions)
    - Lazy Loading (Only Loading them when needed) -> Achieved through Paging and others. Modern OS do Lazy Loading
  - The OS also loads some data of the process into Registers (that are pretty close to cpu and faster than l1 cache). These include program counter (Which instruction is being run right now), stack pointer (keeps track of the call stack)
  - Parts of the memory assigned to a process:
    - Runtime Stack <- Stores Local Variables, Function Params and returning addresses (It's like a call stack)
    - Heap <- Dynamic Memory Allocations during Program execution (For Linked Lists etc) Uses malloc()/free() apis to manipulate the memory in heap
    - File Pointers <- pointer of the program file etc
  - As we discussed above that context switching can happen for CPU Virtualization:
    -  Three common states after a process is created and before it's destroyed
       -  **Ready** <- The process will be in ready state (its ready to be executed by the CPU for computation)
       -  **Blocked** <- THe process is blocked as it has some instructions that relates to IO (file read/write)
       -  **Running** <- The process is currently being run by the CPU
       -  Initialized -> |  Ready -> Running -> Blocked -> Ready
       -  The phenomenon of moving a process from Ready to Running is **Scheduling** a Process
       -  The phenomenon of movina a process from Running to Ready is **De-Scheduling** a process
       - The previous state of a **Blocked** process must be **Running**
       - A process that's **running** but not completed can move to **ready** state
       - Other states like **'Embyro'**, **'Zombie'** etc do exist
    - To make it possible for context switching to happen: A process state must be saved somewhere (Which line the function is currently executing before it got stopped i.e.. again came to ready state is an example)
    - For each process, we have something called as a PCB (process control block) that stores the state of a process
