#
In the first Chapter, there's a section that talks about Timesharing and CPU Virtualization (Fooling the processes 🤣). There's also Memory Virtualization.

- Timesharing: Share the CPU with different processes for a certain time...
- Some notable points to consider:
    - Switching Processes will have an overhead on the performance of the system. (As the OS needs to move the data from/to registers etc)
    - There must be some control on these processes by the OS so that these will not be run forever. (This is the most important thing)
- The OS must virtualize the CPU in an efficient manner while retaining control over the system. To do so, both hardware and operating-system support will be required. The OS will often use a judicious bit of hardware support in order to accomplish its work effectively.
#
- One technique to execute the process:- Direct Execution i.e.. simply execute it and wait till its done 🤔
    - How it works ?
        - Run the process directly on the CPU
            - Create an entry into the process list
            - Load the program into the memory and clear/fill registers and start main() method
            - Once it's done, remove it from the process list and freeup the memory
    - Points to consider:
        - There's nothing like timesharing in this technique. Nothing like swtiching processes by OS 😒
        - What if the process tries to modify some content which it shouldn't ??? 😨 (OS can't control it) i.e.. A process must be able to perform I/O and some other restricted operations, but without giving the process complete control over the system. How can the OS and hardware work together to do so?
    - How do we restrict a process ???
        - Well, if we think in reverse, it'll be the case where we're not going to give any permissions to the process that may harm the system. Meaning, We're sort of setting some levels or modes here
            - User Mode
            - Kernel Mode
        - In User Mode, the process executes normally. But when ever there're some requests like reading a file (IO based), the process will be given some push so that it'll be executed in kernel mode. If a process doesn't have access to the corresponding action/operation, the system will do two things
            - If needed, it pushes the process to the kernel mode and there, the process executes till the corresponding operation is done.
            - Throws an exception and kills the process
        - In kernel mode, the process has unlimited access to the hardware. And this is where the whole OS runs at (or kernel).
    - How does a process move from Kernel mode to User mode and vice-versa ??
        - 
    
