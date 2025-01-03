+++
date = '2025-01-03T17:55:19+07:00'
draft = false
title = 'About rclone network'
+++
# First start
Rclone has been widely used for network drives because of its convenience in supporting many different drive/cloud storage platforms. You can check it out at [Rclone](https://rclone.org/).
# Fun part
Because it can supported so many drive, you can , theoretically, linking multiple drive storage into one place. So let says you have 15Gb of free storage on google drive with 10 accounts it will go up to 150Gb of lifetime vault with 0$ (Yeah it's sound good to be true). Here is some tips to do that properly:
  + You should checkout [Cloud storage provider](https://comparisontabl.es/cloud-storage/)
## Connecting the drive together
Assume you already know how to use rclone to link an account with it, we will go to other parts:
- [Rclone union](https://rclone.org/union/): This is the first thing you need to do as it will connect multiple drive together.
- **Optional:** It is recommended that you should create another folder inside both drive for this project, since you dont want your others data be F@ckup or it can make your drive look like a mess.
   + Step:Run this command based how many storage you choose: ``rclone mkdir <drive names>:/union``
   	--- Example: ``rclone mkdir Gdrive01:/union``
	+ Check again with ``rclone ls <drive names>``
### Step
1. Use rclone config to create another remotes, ``union``, choose the drive you want to connecting with each other (It was a name you give to the previous remotes that were linked to the storage drive, says i name it Gdrive01 and Gdrive02 so it will be ``Gdrive01:/ Gdrive02:/``). Again if you followed the **optional** instruction it will be ``Gdrive01:/union Gdrive02:/union``.
 	+ If you dont want to go too deep, the default settings should work Okay. But in my cases, i want to keep one storage filling up first then go to other ones so i will choose **eplfs** [Document](https://rclone.org/union/#policy-descriptions).
2. Check if it work properly by ``rclone mount <remotes name>: Z:`` 	 
## Chunker
 This section is for paranoid people. Like how to **optimized** all the storage used.
- Example: You want to store 2Gb .zip file on both rusty 14/15Gb storage used, in theory it would be left 2Gb free on Union right. But how ?
Rclone have something call chunker where in there if the file sizes is larger than the configure (Default 3Gb) it will split the files into smaller chunk and stored them. You can configure the chunk_size at ``rclone config``.
 1. Create new remotes, ``chunker``, choose the drive you want use for chunked (should be ``union`` if you want to solve the last problem).
     - **Optional** It's recommended to create ``chunk`` folder on selected disk like the previous ones, see the last [Optional](#connecting-the-drive-together).
 2. Just edit the chunk_size, the other works fine at default settings.
 3. Mount the chunker with ``rclone mount chunker-drive:/chunk X:``

On my real project i use 2Gb to *make it fit more*.
If you follow all the step the Cloud storage may look something like this:

	Drive
	|-rclone
		|-union
			|-chunk
#### THANKS FOR READ TO THIS POINT.