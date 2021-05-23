# Organizing local MP4 videos

There are various applications than can organize and play our locally stored MP4 videos but besides that, maybe we would like to organize them in a custom way.

To easily accomplish that, we could create the neccessary HTML code and access our videos in a quick, simple and fully customizable way.

Firstly, we will need to create Python lists with the filenames of our videos. Each Python list, will contain a few videos that we consider them as one group.

To simplify things, we assume that the videos we are interestd are stored in a single folder (but not inside any subfolders) and we will create five Python lists with those videos, each one containing the filenames that we consider as an individual group.

Thus, we will split our videos into five groups and we will use buttons to select which group will be displayed on our browser.


## Creating a Python list with all the file names in a folder

Firstly, we will need all the filenames of the videos that are included in the folder we are interested to use. Obviously, we could create this list manually but using python it will take just a few seconds. 

We will only need to run this python code inside the folder:

```python

import os
video_list = os.listdir()
print(video_list)

```

Running this code, a Python list with all the filenames of the folder will appear on screen and we can copy-paste that list and use it.

Alternatively, we could also run this code which will create a file with the list of the all the file names:

```python

import os
video_list = os.listdir()
my_file = open("output_file.txt","w")
print(video_list, file=my_file)


```


