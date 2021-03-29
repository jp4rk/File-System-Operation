# File-System-Operation
This is one of the tasks that I was assigned with while I was taking a technical interview at DisputeSoft company. The assignment was primarily concerned with assessing my foundational programming and scripting skills, including file system operations and regular expression usage. I used Python to perform this task. Below is the task description: 


You have been provided with a “repository” folder containing approximately 180 files without extensions, as well as a file named “data.csv,” which contains two columns. For each row in “data.csv,” the data in column A, “repository path,” is a path to a file in the repository folder, and the data in column B, “copy path,” is a path to which the file should be copied by your source code. Your overall task will be to export the data from the “repository” folder into a set of files. You will also be asked to generate a report that contains metadata for the extracted files. The files that you will extract from the repository are a collection of image (“.jpg”) and text (“.txt”) files. Some of these files have been assigned the wrong extension in column B of the data.csv file; i.e., some
images have been given the “.txt” extension, and some text files have been given the “.jpg” extension. 
Your answer to the PROGRAMMING TASKS should be returned in the form of:
1) A source code file or files you create to complete the PROGRAMMING TASKS below. Please
include comments in your code that indicate what code is directed towards which task.
2) A folder of exported files that you will create as instructed in the PROGRAMMING TASKS below.
3) A spreadsheet in CSV format called “results.csv” which contains the information requested in
the PROGRAMMING TASKS below.

The “results.csv” report you generate should include the following column headings:
1) “Repository Path”;
2) “Copy Path”;
3) “Corrected File Path”;
4) “Duplicate Files”;
5) “File Size”;
6) “Date of File Creation”;
7) “Line / Pixel Count”; and
8) “Page Number”.


PROGRAMMING TASK 1.1 (5 points): Copy each file in “data.csv” from the “repository path” to the “copy path,” and correct the extension of each of the mislabeled files. Report the correct extracted paths (including corrected file extensions) in “results.csv” in the “Corrected File Path” column.

PROGRAMMING TASK 1.2 (5 points): Certain files have been included in the repository more
than once. For each file, list any duplicates in “results.csv” in the “Duplicate Files” column. A file is a duplicate if its content is identical to that of another file. If a file has more than one duplicate, list all duplicate files in the “Duplicate Files” column as a semicolon-delimited list. List files based on their “Repository Path.”

PROGRAMMING TASK 1.3 (5 points): For each file, calculate the file size in bytes. Record this information in the “File Size” column of “results.csv.”

PROGRAMMING TASK 1.4 (5 points): For each file, determine the date on which the file in the repository was created. Record this information in the “Date of File Creation” column of “results.csv.”

PROGRAMMING TASK 1.5 (5 points): For each of the text files, calculate the number of lines per file; for each image file, calculate the number of pixels (i.e., height x width). Record this information in the “Line / Pixel Count” column of “results.csv.”

PROGRAMMING TASK 1.6 (5 points): Each text file contains a page number in the format “[Pg
xx]” where “xx” is at least one digit. List the page number for each text file (the “xx” in the sentence above) in the “Page Number” column of “results.csv”; leave this field blank for each image file.


