# dex_poalim
**1. VirtualBox & Ubuntu Installation**
  - 1 Download and Install VirtualBox
  - 2 Download the Latest Ubuntu
  - 3 Install Ubuntu on VirtualBox
    - ![install from scratch](https://github.com/user-attachments/assets/fa35e93d-fb81-4486-a2cc-021ce328d993)
    - ![image](https://github.com/user-attachments/assets/8c427f0e-32b6-4038-a666-0c0162baadc8)
      
**2. Linux Directory and File Creation**
  - 1 Create Directories and Files done
     - `ls`: This command lists the contents of the current directory. It shows all files and subdirectories within that directory.
     - `mkdir homework`: This command creates a new directory named homework. The mkdir stands for "make directory."
     - `mkdir dir1 dir2 dir3`:This creates three new directories named dir1, dir2, and dir3 in the current directory, all at once.
     - `touch file1.txt file2.txt file3.txt`:This command creates three empty files named file1.txt, file2.txt, and file3.txt in the current directory. The touch command is used to create new, empty files or update the timestamps of existing files.
    - ![image](https://github.com/user-attachments/assets/fa662d79-9530-4865-a60d-3acab832aca1)
  - 2 Add Content to Files done
    - `echo "my first text" >> ~/homework/dir1/file1.txt`: This command appends the text "My first text" to the file file1.txt located in the directory ~/homework/dir1. The >> operator adds the text to the file without deleting its previous contents.
    - `cat ~/homework/dir1/file1.txt`: This command displays the contents of the file file1.txt in the terminal. cat stands for "concatenate," but it is often used to read and display file content.
    - In this example, the path ~/homework/dir1/ is specifically used to indicate that the text search is being conducted in this particular directory. The ~ symbol represents the user's home directory, and *.txt is a pattern that covers all .txt files in the dir1 directory.
    - ![image](https://github.com/user-attachments/assets/14aa459f-766a-43e4-bcf0-71f8a7f1243d)
      
**3. Using grep and find Commands ** 
  - 1 Search for text within files
    - `grep`: this command searches for the text "my first text" in all .txt files inside the ~/homework/dir1/ directory and displays the lines where it finds that text.
    - ![image](https://github.com/user-attachments/assets/fbe50a81-1e40-484c-90e1-2a273595eb1c)
  - 2 Find files in a directory
    - `cd ../..`: This command changes the current directory to two levels up from the current directory. Each .. moves up one directory level.
    - `find ~/homework/dir1 -type f`: This command searches for all files (-type f) in the directory ~/homework/dir1 and its subdirectories. The find command is used for searching files and directories based on conditions. 
    - ![image](https://github.com/user-attachments/assets/f85d37c7-b004-4009-bca2-d25617130b19)
    - Find files modified within the last 7 days
    - `find ~/homework/dir1 -type f -mtime -7`: This command finds all files (-type f) in the directory ~/homework/dir1 that were modified in the last 7 days. The -mtime -7 specifies files modified within the last 7 days (from the current date).
    - ![image](https://github.com/user-attachments/assets/09e76918-6e94-4e4c-b36c-95331148c699)
