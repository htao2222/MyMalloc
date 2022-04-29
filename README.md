Our Malloc design used a Header struct as metadata, containing the status of the block whether it was FREE or ALLOCATED, the size of the block,
and pointers to the next and the previous blocks. Malloc aligned the memory to 8 bytes and searched for the first available free block, once found
if big enough it would be split, otherwise it would be completely allocated. When Malloc was first called it would check if it needed to be initialized,
by checking the metadata status at the beginning of the memory. If that was equal to 0 then it would call mem_init to initialize the memory array.
Mem_init initialized the array by creating a header metadata, free metadata and epilogue metadata. Header and epilogue metadata consisted of allocated blocks
of just metadata which would prevent edge cases when freeing. Free metadata would consist of the rest of the free memory in the array.

Our Free design first checked that the pointer received was a valid pointer, it checked it was given by malloc by making sure it was in the memory array.
Then it made sure it was the start of a chunk by checking the status of the struct Header and making sure it was either FREE or ALLOCATED. Then if the
status was FREE it would return an error with Double Free. Following it would check the user wasn't trying to free the header and epilogue blocks.
Then after all the checks it would free the block, then attempt to coalesce the newly freed block with the neighboring free blocks before returning.

Our last 2 stress tests consist of Test #4 which consists of allocating and immediately freeing the largest possible chunk of memory for the array.
Test #5 consists of allocating randomly 50 chunks of random size between 1 and 50 bytes before freeing them all.
Test 1 took 0.000006 seconds on average
Test 2 took 0.000026 seconds on average
Test 3 took 0.000010 seconds on average
Test 4 took 0.000003 seconds on average
Test 5 took 0.000007 seconds on average
