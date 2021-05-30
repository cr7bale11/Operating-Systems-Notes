**Process APIs**

Note: All the below APIs and examples are w.r.t Unix
- Three main APIs -> Fork, Exec, wait from UNIX systems
  
- ***Fork***:
  ```cpp
      int rc = fork(); // Creates a new process (clone of the process that's currently running)
      if (rc < 0) {
        // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
      } else if (rc == 0) {
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
      } else {
        // parent process that's been created by the OS to run this program.
        // this rc will be the PID of the child process
        printf("hello, I am parent of %d (wc:%d) (pid:%d)\n",
	       rc, wc, (int) getpid());
      }
  ```
  ```
      The output is not deterministic:
        - It can be the following:
          hello, I am child (pid:1729)
          hello, I am parent of 1729 (pid: 1728)
        - or:
          hello, I am parent of 1729 (pid: 1728)
          hello, I am child (pid:1729)
  ```
  - Creates a new process at the same line where the fork was called. This function call returns any of the following:
    - < 0 (less than 0) if the process creation's been failed w.r.t fork
    - greater than or equal to 0 (>= 0) if the process creation is successful
      -  if the returned value is 0, then the current process that's executing is the newly created process (a child)
      -  Else, the current process that's executing is the original process (parent and this value is the PID (process identifier) of the child process that's been created)
      -  Note: This is non-deterministic (we donno which process after forking will run all the code first and come out)
   - Every process that's been created using fork will have different PCB (process control block). They have different Program counter, different Stack pointer (values of the parent are copied completely (deep copy 🤣)). So, it takes time for the process to be created
      - Note: A parent process can't control a child process (using it's own power.) once it's created as it's a process itself and only OS level command can manipulate it

- ***Wait***:
  - Wait makes the parent process wait till the child process finishes off. (An analogy: Like thread.join() but it's with respect to the process)
  ```cpp
      int rc = fork();
      if (rc < 0) {
        // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
      } else if (rc == 0) {
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
      } else {
        // parent process that's been created by the OS to run this program.
        // this rc will be the PID of the child process
        int wc = wait(NULL); // makes the parent wait till the child process executes the complete code.
        printf("hello, I am parent of %d (wc:%d) (pid:%d)\n",
	       rc, wc, (int) getpid());
      }
  ```
  - Wait makes the output sort of deterministic as we can make sure that the child process can execute the program first and then the parent..
  - Excercise: Consider the problem of creating processes in hierarchial manner. Understand how wait works in this case

- ***Exec***:
  - Useful in running another program inside the current program. (Exercise: Try making it recursive 🤣). 

  ``In the below code, the program it is trying to execute is wc (maybe a cmd utility) and it's being passed a file on which the calculation of the no of words and lines will happen``
  ```cpp
    int rc = fork();
    if (rc < 0) {
        // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) {
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
        fflush(stdout);
        char *myargs[3];
        myargs[0] = strdup("wc");   // program: "wc" (word count)
        myargs[1] = strdup("p3.c"); // argument: file to count
        myargs[2] = NULL;           // marks end of array
        execvp(myargs[0], myargs);  // runs word count
        printf("this shouldn't print out");
    } else {
        // parent goes down this path (original process)
        int wc = wait(NULL);
        printf("hello, I am parent of %d (wc:%d) (pid:%d)\n",
	       rc, wc, (int) getpid());
    }
  ```
  - The process that starts this ``exec``/``execvp`` will be overrriden (meaning it entirely became a new process (in the above example, it became a process of ``wc``) that's for the other program that it has executed)
  - Note: ``exec`` will not return anything here.

- ``Fork`` and ``Exec`` is a powerful combo to create and execute different processes. Shell (bash/zsh etc) use these to mainly create and execute processes. 
  - An analogy of use cases of the above APIs in the shell:
    - When the user enters a command and it's params in the shell:
      - First ``fork`` will create a child process
      - This child process will use ``exec`` to execute the command/program (for example: ``wc``) and ``wait``'s till it's done

- Other APIs like **Pipe** etc do exist (yes, **Pipe** system call is the execution underneath `|` or pipe in a shell)

- **KILL** command is used to kill a process (there's **pkill** as well.). There's an API behind this
- Signals like ``SIGINT`` (to interrupt a process) and ``SIGSTP`` (to stop in the middle) that are present in the shells (bash, zsh etc) use ``Signal`` API to asynchronously send a signal to the process that's getting executed. Through these, we can control processes (process control)

#
Questions:
- Q1: How many times ``hello, I am grandchild...`` is gonna be printed on the console ?? (Think and answer this. Don't execute this code in a shell 😂)
 ```cpp
  int main(int argc, char* argv[])
  {
      printf("hello world (pid:%d)\n", (int)getpid());
      fflush(stdout);
      int rc = fork();
      if (rc < 0) { // fork failed; exit
          fprintf(stderr, "fork failed\n");
          exit(1);
      }
      else if (rc == 0) { // child (new process)
          printf("hello, I am child (pid:%d)\n", (int)getpid());
          fflush(stdout);
          fork();
          printf("hello, I am grandchild (pid:%d)\n", (int)getpid());
      }
      else { // parent goes down this path (main)
          int rc_wait = wait(NULL);
          printf("hello, I am parent of %d (rc_wait:%d) (pid:%d)\n", rc, rc_wait, (int)getpid());
      }
      return 0;
  }
```
- Q2: What's the output of the following program in the console ? (Don't execute this as well)
```cpp
  int main()
  {
      printf("Welcome to Educative!\n");
      fflush(stdout);
      int rc = fork();
      printf("Operating Systems: Three Easy Pieces\n");
      return 0;
  }
```