# Supermode

Supermode is a program that allows your usermode process to read any arbitrary physical address aswell as virtual address of a process using page table manipulation

## How it works

### Physical R/W:
We insert 2 PTEs into our target process using our driver.
PTE1 points to the physical address of PTE2.
Now when we want to read any physical address all we do is use PTE1 to change the PFN of PTE2 to point to our desired physical address, now we read PTE2. Of course you need to flush TLB in between.

### Virtual R/W:
Since I couldn't get this to work using only the Physical R/W due to slow speeds I came up with another method that one can use to read from the target process.
Copy the entire PML4 of the target process and store it.
Insert another PTE into our target process that points to the DTB of our target process.
When we want to read a virtual address from the target game we write to the 3rd PTE and insert the stored PML4E.
Then we change the virtual address to use our newly inserted PML4E.
Now we use the 3rd PTE to remove the inserted PML4E again.

## Issues

Supermode is unfortunately very unstable and physical memory reads are slow since you have to wait for a TLB flush between each read
