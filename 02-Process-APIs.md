**Process APIs**

- Three main APIs -> Fork, Exec, wait from UNIX systems
  
- *Fork*:
  - Creates a new process at the same line where the fork was called. This function call returns any of the following:
    - < 0 (less than 0) if the process creation's been failed w.r.t fork
    - greater than or equal to 0 (>= 0) if the process creation is successful
      -  if the returned value is 0, then the current process that's executing is the newly created process (a child)
      -  Else, the current process that's executing is the original process (parent and this value is the PID (process identifier) of the child process that's been created)
      -  Note: This is non-deterministic
   - Every process that's been created using fork will have different PCB (process control block). They have different Program counter, different Stack pointer (values of the parent are copied completely (deep copy 🤣)). So, it takes time for the process to be created
   - 