---
layout: post
title: "Secure File Deletion with Eraser"
date: 2024-05-28
categories: Sys-Admin
---

Secure file deletion ensures that files are not only removed from the filesystem but also made irrecoverable by any data recovery tools. Simply emptying the recycle bin frees up space, but it does not actually erase the data, leaving it vulnerable to recovery. Tools like Eraser overwrite the data multiple times to prevent any possibility of recovery.

### Why Use Eraser?

When you delete files normally, they are not truly gone; the filesystem just marks the space they occupied as available for new data. Until the space is overwritten, the deleted files can be easily recovered with data recovery software. Eraser ensures that when you delete a file, it is permanently destroyed by overwriting the data multiple times using secure algorithms.

### Using Eraser on Windows

#### Steps to Securely Delete Files with Eraser:

1. **Open Eraser**: Launch the Eraser program.

2. **Create a New Task**:
   - Click on "Erase Schedule" in the menu.
   - In the Erase Schedule window, right-click in the blank space under "Task Name".
   - Select "New Task" from the context menu.

3. **Configure the Task**:
   - In the "Task" window that appears, click on the "Add Data" button.
   - In the "Select Data to Erase" window, choose "Files" if you want to add specific files, or "Folders" if you want to add entire folders.
   - Click the "Browse" button to select the files or folders you want to securely delete.

4. **Set Task Parameters**:
   - After selecting the files/folders, click "OK".
   - Back in the "Task" window, you can configure additional settings like the schedule for the task if you want it to run automatically at certain times.
   - Make sure "Run Immediately" is checked if you want the task to execute as soon as you click "OK".

5. **Execute the Task**:
   - Once you have configured the task, click "OK" to create it.
   - The task should now appear in the "Erase Schedule" window.
   - To start the task immediately, right-click on it and select "Run Now".

### Secure Move Feature in Eraser

The secure move feature in Eraser enhances data security during file transfers. When you move files from one location to another, traces of the original files can remain on the source storage device, making them potentially recoverable. The secure move feature addresses this by securely erasing the files from the original location after they have been successfully transferred.

#### How to Use Secure Move with Eraser:

1. **Open Eraser**: Launch the Eraser program.

2. **Create a New Task**:
   - Click on "Erase Schedule" in the menu.
   - In the Erase Schedule window, right-click in the blank space under "Task Name".
   - Select "New Task" from the context menu.

3. **Configure the Task**:
   - In the "Task" window that appears, click on the "Add Data" button.
   - In the "Select Data to Erase" window, choose "Secure Move".
   - Click the "Browse" button to select the files or folders you want to securely move.

4. **Select Destination**:
   - After selecting the files/folders, specify the destination where you want the files to be moved.
   - Ensure that the "Secure Move" option is enabled to securely erase the original files after moving.

5. **Set Task Parameters**:
   - Configure additional settings like the schedule for the task if you want it to run automatically at certain times.
   - Make sure "Run Immediately" is checked if you want the task to execute as soon as you click "OK".

6. **Execute the Task**:
   - Once you have configured the task, click "OK" to create it.
   - The task should now appear in the "Erase Schedule" window.
   - To start the task immediately, right-click on it and select "Run Now".

By using the secure move feature, you ensure that your sensitive data is not only moved to its new location but also securely erased from the original location, preventing any possibility of data recovery from the source.

**Additional Tips**:
- For immediate file shredding without creating a scheduled task, you can use the right-click context menu in Windows Explorer. Right-click the file you want to delete, and select "Eraser" > "Erase".

### Alternatives for Linux and Mac

#### Linux:
- **Shred**: A common command-line utility that securely deletes files by overwriting them multiple times.
  - **Usage**:
    ```bash
    shred -u -n 3 filename
    ```
    This command will overwrite the file `filename` three times and then remove it.

#### Mac:
- **Secure Empty Trash**: macOS provides an option to securely empty the trash, overwriting the files before deletion.
  - **Usage**: Hold down the Command key and right-click the Trash icon, then select "Secure Empty Trash".
- **Permanent Eraser**: A tool for Mac that uses the Gutmann Method to securely delete files.
  - **Usage**: Drag and drop files to the Permanent Eraser icon.

### Conclusion

Combining the use of tools like Eraser on Windows, Shred on Linux, and Permanent Eraser or Secure Empty Trash on Mac ensures that deleted files are securely and permanently removed from your system, safeguarding your sensitive data from recovery.