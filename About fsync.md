Before we talk about fsync, I want to make sure that everybody knows the ACID principle in database. If you don't, feel free to look it up over the Internet. So fsync is closely related to the D(durability) od ACID. The principle of durability means that at any given time, the data should be written on disk physically in case of accidents like power outage which cause cause data loss.

On POSIX systems, durability is guaranteed through a series of sync operations(fsync(), fdatasync(), aio_fsync()).

>   The `fsync()` function is intended to force a physical write of data from the buffer cache, and to assure that after a system crash or other failure that all data up to the time of the fsync() call is recorded on the disk. Meanwhile, the `fdatasync()` function is the same as the `fsync()` except that it doesn't update meta-data associated with a file

##  File
Before we dig deeper into `fsync()`, we need to know that files in our filesystem consists of two parts, the directory entry and the file itself. For example, if you want to read a file, the filesystem would first fetch the entry for this file, and then open the associated data blob. For those who are familiar with c/c++, you could think of it as the pointer and the data that the pointer points to.

Now we know how the file is stored in our filesystem, what about it? Based on `fsync()`'s description from [here](http://man7.org/linux/man-pages/man2/fdatasync.2.html), I quote

> Calling fsync() does not necessarily ensure that the entry in the directory containing the file has also reached disk.  For that an explicit fsync() on a file descriptor for the directory is also needed.

Some filesystem like ext3 could do the whole syncing for you, but they all have their own problem based on this [article](http://blog.httrack.com/blog/2013/11/15/everything-you-always-wanted-to-know-about-fsync/). I would copy and paste them below for better readability.

*   Linux/ext3: If the underlying hard disk has write caching enabled, then the data may not really be on permanent storage when fsync() / fdatasync() return.

*   Linux/ext4: The fsync() implementations in older kernels and lesser used filesystems does not know how to flush disk caches.

*   OSX: For applications that require tighter guarantees about the integrity of their data, Mac OS X provides the F_FULLFSYNC fcntl. The F_FULLFSYNC fcntl asks the drive to flush all buffered data to permanent storage (hey, fsync was supposed to do that, no ? guys ?) (Edit: no, fsync is actually not required to do that – thanks for the clarification Florent!)

In conclusion, if you want to ensure durability, you should call `fsync()` twice, one on the file data and one on its parent directory.
