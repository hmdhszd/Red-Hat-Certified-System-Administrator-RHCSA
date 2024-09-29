The RHCSA exam topic **"hard links"** is important because it tests your understanding of file management, linking, and inode structure in Linux. **Hard links** allow multiple filenames to refer to the same file content (inode). Unlike symbolic links, hard links are direct references to the file's inode, and they share the same data, metadata, and permissions.

### What You Need to Know:
- **Hard links** create additional names for the same file content.
- **All hard links share the same inode number** (which identifies the actual data on disk).
- Deleting a hard link **does not affect the file** until all links to the file are deleted.
- Hard links **cannot span different file systems** or partitions and **cannot link to directories** (except for root directories).

### **1. Create a Hard Link Using `ln`**

#### **Example Task 1: Create a Hard Link**

**Task:** Create a hard link for the file `/home/examuser/original.txt` in the `/home/examuser/docs/` directory, naming the hard link `link.txt`.

**Command:**
```bash
ln /home/examuser/original.txt /home/examuser/docs/link.txt
```

**Explanation:**
- `ln` is the command to create a hard link.
- `/home/examuser/original.txt` is the source file.
- `/home/examuser/docs/link.txt` is the new hard link being created.

After this, both `original.txt` and `link.txt` refer to the same inode on disk. Changes to either file will be reflected in both since they share the same data.

---

### **2. Verify the Hard Link**

You may be asked to verify whether a file has hard links. This is done by checking the **inode number** and the **link count** of a file using `ls` or `stat`.

#### **Example Task 2: Verify a Hard Link**

**Task:** Verify that `link.txt` and `original.txt` point to the same inode.

**Commands:**
```bash
ls -li /home/examuser/original.txt /home/examuser/docs/link.txt
```

**Explanation:**
- `-l`: Shows detailed information about the file.
- `-i`: Displays the **inode number**.

Both files should display the same inode number. The inode number is the unique identifier for the file’s data on disk. If the inode numbers match, it means both files are hard links to the same inode.

#### **Example Output:**
```bash
123456 -rw-r--r-- 2 examuser examgroup  1234 Sep 21 14:35 /home/examuser/original.txt
123456 -rw-r--r-- 2 examuser examgroup  1234 Sep 21 14:35 /home/examuser/docs/link.txt
```

- The `123456` indicates that both files share the same inode.
- The number `2` (in the second column) shows the **link count**, meaning there are two links pointing to the same file.

---

### **3. Delete a Hard Link**

When you delete a hard link, the actual data is not removed from the disk until all hard links to that inode are deleted. Each hard link is treated as a regular file, and the data persists as long as there is at least one link left.

#### **Example Task 3: Delete a Hard Link**

**Task:** Delete the hard link `link.txt` and check if the file content is still accessible through `original.txt`.

**Commands:**
```bash
rm /home/examuser/docs/link.txt
cat /home/examuser/original.txt
```

**Explanation:**
- `rm` removes the hard link `link.txt`.
- `cat` is used to display the contents of `original.txt`. Even after deleting `link.txt`, the content of the file is still available through `original.txt` because the file's data on disk remains intact until the last hard link is removed.

---

### **4. Check the Number of Hard Links Using `stat`**

The **`stat`** command provides detailed information about a file, including the number of hard links to it.

#### **Example Task 4: Check the Number of Hard Links**

**Task:** Display the number of hard links for the file `/home/examuser/original.txt`.

**Command:**
```bash
stat /home/examuser/original.txt
```

**Explanation:**
- `stat` gives detailed information about a file, including its inode number, permissions, owner, size, and the **number of links** (hard links) pointing to it.

#### **Example Output:**
```bash
  File: /home/examuser/original.txt
  Size: 1234       Blocks: 8          IO Block: 4096   regular file
Device: 803h/2051d Inode: 123456      Links: 1
```

- The **`Links`** field shows the number of hard links. If the file has multiple hard links, this number will increase.

---

### **5. Create Multiple Hard Links**

#### **Example Task 5: Create Multiple Hard Links**

**Task:** Create two additional hard links for `/home/examuser/original.txt` named `link1.txt` and `link2.txt` in the `/home/examuser/docs/` directory.

**Command:**
```bash
ln /home/examuser/original.txt /home/examuser/docs/link1.txt
ln /home/examuser/original.txt /home/examuser/docs/link2.txt
```

**Explanation:**
- Both `link1.txt` and `link2.txt` will refer to the same inode as `original.txt`.
- Use `ls -li` or `stat` to verify that all three files have the same inode number and link count.

---

### **6. Hard Links and File Deletion**

#### **Example Task 6: Delete the Original File and Access Data from the Hard Link**

**Task:** Delete `original.txt` and verify that the file content is still accessible through `link1.txt`.

**Commands:**
```bash
rm /home/examuser/original.txt
cat /home/examuser/docs/link1.txt
```

**Explanation:**
- **Deleting `original.txt`** does not delete the file content, as the data is still referenced by `link1.txt`.
- The data will remain available as long as there is at least one hard link pointing to it.

---

### Key Differences Between Hard Links and Symbolic (Soft) Links:
- **Hard links** point directly to the inode, meaning they share the same data, metadata, and permissions as the original file.
- **Symbolic (soft) links** are just pointers to the original file’s path and do not share the inode. If the original file is deleted, symbolic links become **broken**.
- **Hard links cannot be created for directories** or across different filesystems, while **symbolic links can**.
- **Symbolic links** are easier to identify, as their `ls -l` output shows the link pointing to the original file.

---

### Summary of Skills for Hard Links on the RHCSA Exam:
1. **Create hard links** using the `ln` command.
2. **Verify hard links** using `ls -li` and `stat` to check inode numbers and link counts.
3. **Delete hard links** and understand how file data persists as long as one hard link remains.
4. **Check the number of hard links** using the `stat` command.
5. **Understand the difference** between hard links and symbolic links.