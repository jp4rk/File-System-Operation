import os
import os.path, time
import pandas as pd
import numpy as np
import shutil
import imghdr
import re
from PIL import Image
import io
import imagehash


# Read csv files
data = pd.read_csv('/Users/JoonPark/Desktop/Disputesoft/joon.park/data.csv')


#Create column headings 
col = ["Repository Path", "Copy Path", "Corrected File Path",
          "Duplicate Files", "File Size", "Date of File Creation",
          "Line / Pixel Count", "Page Number"]


#Creating dataframe to store answers for programming tasks
df  = pd.DataFrame(index = range(0, len(data)), columns = col)

#Making sure we're in a right directory

direct = '/Users/JoonPark/Desktop/DisputeSoft/joon.park/'
retval = os.getcwd()

if(retval != direct):
    os.chdir(direct)

#1
"""
PROGRAMMING TASK 1.1 (5 points): Copy each file in “data.csv”
from the “repository path” to the “copy path,” and correct the
extension of each of the mislabeled files.Report the correct extracted
paths (including corrected file extensions) in “results.csv” in the
“Corrected File Path” column.
"""

for i,j in data.iterrows():
    
    copy_direct = os.path.dirname(j["copy path"])
    if not os.path.exists(copy_direct):
        path = os.path.join(copy_direct, direct)
        os.makedirs(path)
    # copying each file 
    shutil.copy2(j["repository path"], j["copy path"])


"""
Creating 2 dictionary files for task 1.2. The first dictionary stores the hash
value of the file as a key and the number of times such hash value appeared
in the file as value. The second dictionary stores the hash value as a key and
repository path as a value. This is done so that we can map the hash values which
appear more than once in the file into repo_path to store duplicate files. 

"""


dic = {} 
dic2 = {} 


    
for i in range(0, len(data)):
    
    #Copying repo-path and copy-path into the dataframe

    path = data.iloc[i][1]
    repo_path = data.iloc[i][0] 
    df.iloc[i][0] = repo_path
    df.iloc[i][1] = path
    filename, ext = os.path.splitext(path)

    """
    We will use try and except to see if we can open up the file.
    If yes, this means that the file is supposed to be txt, and vice versa.
    For try, we will check whether if the extension is correclty
    labeled as 'txt' file. If not, we will add to the 3rd column of dataframe
    and correct the extension.
    """

    try:
        
        file_object = open(direct + path)
        file_read = file_object.read()
        if(ext != ".txt"):
            corrected_ext = filename + ".txt"
            df.iloc[i][2] = corrected_ext
        else:
            df.iloc[i][2] = path

        key = hash(file_read)
        
        #semicolon for creating a "semicolon-delimited list"

        dic2[key] = repo_path + ";"

        if key not in dic:
            dic[key] = 1

        else:
            dic[key] += 1

        
        for key,value in dic.items():
            if value > 1:
                df.iloc[i][3] = dic2[key]

        # Task 1.3 - retrieving file size

        df.iloc[i][4] = os.path.getsize(direct+path)

        # Task 1.4 - rectrieving the date of when repo is created

        df.iloc[i][5] = time.ctime(os.path.getmtime(path))

        CoList = file_read.split("\n")
        count = 0
        for k in CoList:
            count += 1
            
        # Task 1.5 - counting the number of lines

        df.iloc[i][6] = count


        # Task 1.6 - counting a page number using regular expression.
        # First, the [Pag xx] format was extracted, then the number.
        
        x = re.findall(r'\[\w+\s\d+\]', file_read)[0]
        page_num = re.findall(r'\d+', x)[0]

        df.iloc[i][7] = page_num



    # Except is for dealing with jpg files. Most of the processes are
    # similar as above, except some.
    
    except:
        #Used Image module to access jpg files.
        
        im = Image.open(direct + path)
        if(ext != ".jpg"):
            corrected_ext = filename + ".jpg"
            df.iloc[i][2] = corrected_ext
        else:
            df.iloc[i][2] = path

        #Retrived hash value of jpg file using average_hash function
        hashVal = imagehash.average_hash(im)
        dic2[hashVal] = repo_path

        if(hashVal not in dic):
            dic[hashVal] = 1

        else:
            dic[hashVal] += 1


        for key,value in dic.items():
            if value > 1:
                df.iloc[i][3] = dic2[key]

        # Task 1.3 - retrieving file size

        df.iloc[i][4] = os.path.getsize(direct+path)

        # Task 1.4 - retrieving the time when repo is created

        df.iloc[i][5] = time.ctime(os.path.getmtime(path))

        # Task 1.5 - retrieving the pixel size of an image (width x height)

        width, height = im.size
        df.iloc[i][6] = str(width) + " x " + str(height) + " = " + str(width*height)

        # Task 1.6 - leaving this field blank

        df.iloc[i][7] = ""

df.to_csv('results.csv', encoding = 'utf-8', index=False)





# Other random codes used for testing

"""
file_object = open(direct + '/extractedFiles/3bbbbbab901e2b2cfc830971a8cccd35.txt')
print(os.path.getsize(direct + '/extractedFiles/3bbbbbab901e2b2cfc830971a8cccd35.txt'))

ans = file_object.read()
CoList = ans.split("\n")
count = 0 
for k in CoList:
    count += 1
print(count)
print(ans)
x = re.findall(r'\[\w+\s*\d\]', ans)[0]
ans = re.findall(r'\d', x)[0]
print(ans)



file_object = open(direct + '/extractedFiles/3bbbbbab901e2b2cfc830971a8cccd35.txt')
print(os.path.getsize(direct + '/extractedFiles/3bbbbbab901e2b2cfc830971a8cccd35.txt'))

im = Image.open(direct + '/extractedFiles/2ed87900baad70d95f2b\
3a4b01febaaa.jpg')
width, height = im.size
print(os.path.getsize(direct+'/extractedFiles/2ed87900baad70d95f2b\
3a4b01febaaa.jpg'))

print(width * height)



an_Image = Image.open(direct + '/extractedFiles/2ed87900baad70d95f2b\
3a4b01febaaa.jpg')
output = io.BytesIO()
an_Image.save(output, format = "png")
image_as_string = output.getvalue()

print(image_as_string)
"""
