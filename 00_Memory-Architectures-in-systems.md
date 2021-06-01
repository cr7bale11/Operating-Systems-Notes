Memory is basically RAM here and all the programs/instructions are gonna be there in the memory
**Memory**:
  - Memory reads/writes are considerably slower than CPU processing (but why as 3200Mhz (speed of a ram currently) === (sort of equal) 3Ghz (speed of a processor currently))
  - A processor trying to update a cell in the memory will block that cell completely from other processors 
  - Shared vs Disributive Memory Architectures
    - Shared:
      - All the processors access the same memory thats' in the global address space i.e.. altho each processor accesses the memory independently, any updates on the memory will be known to all the processors as the memory is global
        - Example: Copying data in the memory (basically copying into two) will be known to all the processors
      - The memory may not be existing on the same physical device (like a cluster of systems)
      - The key thing is that all the processors see the changes that happens in the memory
      - UMA/NUMA (Uniform Memory Access/Non Uniform Memory Access) designs:
        - UMA: In this design, All of the processors have equal access to the memory (equal latency to access the memory)
          - Most common architecture in UMA (Symmetric Multirprocessing System/SMP) -> 
            - In this architecture, all the processors connect to the main memory via the system bus. 
              - One issue that we can have is as each processor (currently) has local cache (L1, L2 caches are at each core). 
              - So, If there's an update in the main memory for a key that's present in the cache of a processor, the cache data must also be updated... (even tho the processor sees the update, maybe it's unable to do anything), This issue is called **Cache Coherance**
        - Non Uniform Memory Access Design: 
          - It's a hub of UMAs (think of a system that has multiple UMAs)
          - So each node of NUMA will have a memory, a bus inside and also some CPUs. There can be multiple nodes. And each node is connected to a global bus
            - So, For some memory access, If it's in their local node's memory, then it'll be quick or else, it'll be slow.
            - As in shared memory, every processor in every node can still see the memory changes (lol, I donno how)
      - Doesn't scale well even tho it's easy for the programmers to write llel code. For scaling, If new processors are added, then because of Cache Coherence (one such example for downsides), to update the data in the local caches, there must be a lot of communication to happen
      - Shared Memory puts the responsibility on the programmer to synchronize the memory access for correct behavior
    - Distributed memory:
      - No concept of global memory. Each node/processor has their own local memory for access. And all of these processors are connected via a network like an Ethernet connection.
      - If there's a change in one of the node's local memory, all the other nodes are oblivious to this change
      - Communication is still tough but it's scalable as adding more nodes doesn't affect the system at all
    - Super Computers use some form of Distributed Memory Architecture/Design or a combo of both 
    - In the real world, the number of processors inside a chip (CPU) is ever growing with in a span of 3 years (from i7-5th gen to i5-8th gen), the number of processors has been more than doubled. And As ours is not a distrubuted systems based models (laptops, desktops, mobiles), the main limitation comes from is accessing the memory itself as it's global. 
      - So, adding processors doesn't really work ideally as it will lead to more communication on the memory via the bus and it'll decrease the performance instead. So, what did Intel and AMD do to overcome the memory overhead and add more cores ???