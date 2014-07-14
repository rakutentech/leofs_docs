.. LeoFS documentation

.. index::
   pair: XFS; Installation

XFS-related
------------

.. note:: We highly recommend using an XFS partition, as it is the file system that shows the better results with LeoFS. This section describes the installation instructions related to XFS. If you are deploying LeoFS on a **DEV environment**, you do NOT need to perform this operation.

Install required libraries for XFS with yum (CentOS 6.5)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. index::
   pair: CentOS-6.5; Installation

::

   $ sudo yum --enablerepo=centosplus install kmod-xfs xfsprogs xfsprogs-devel


Create an XFS Partition (Volume)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. index::
   pair: XFS; Installation

Only the servers running **LeoFS storage nodes** will benefit from using XFS (unix local file system). XFS provides particularly efficient I/O for large files. **LeoFS-Storage** is implemented on top of files stored in a single file system created on top of the a few TB volume.

Start fdisk
"""""""""""""""""

::

   $ sudo fdisk /dev/sda

   The number of cylinders for this disk is set to 8908.
   There is nothing wrong with that, but this is larger than 1024,
   and could in certain setups cause problems with:
   1) software that runs at boot time (e.g., old versions of LILO)
   2) booting and partitioning software from other OSs
   (e.g., DOS FDISK, OS/2 FDISK)

   Command (m for help): p

   Disk /dev/sda: 73.2 GB, 73272393728 bytes
   255 heads, 63 sectors/track, 8908 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes

      Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1        1951    15671376   83  Linux
   /dev/sda2            1952        2472     4184932+  82  Linux swap / Solaris

Create partition
""""""""""""""""

::

   Command (m for help): n
   Command action
      e   extended
      p   primary partition (1-4)
   p
   Partition number (1-4): 3
   First cylinder (2473-8908, default 2473):[Enter]
   Using default value 2473
   Last cylinder or +size or +sizeM or +sizeK (2473-8908, default 8908):[Enter]
   Using default value 8908

   Command (m for help): w
   The partition table has been altered!

   Calling ioctl() to re-read partition table.

   WARNING: Re-reading the partition table failed with error 16: Device or  resource busy.
   The kernel still uses the old table.
   The new table will be used at the next reboot.
   Syncing disks.

Confirm
""""""""

::

   $ sudo fdisk /dev/sda

   The number of cylinders for this disk is set to 8908.
   There is nothing wrong with that, but this is larger than 1024,
   and could in certain setups cause problems with:
   1) software that runs at boot time (e.g., old versions of LILO)
   2) booting and partitioning software from other OSs
   (e.g., DOS FDISK, OS/2 FDISK)

   Command (m for help): p

   Disk /dev/sda: 73.2 GB, 73272393728 bytes
   255 heads, 63 sectors/track, 8908 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1        1951    15671376   83  Linux
   /dev/sda2            1952        2472     4184932+  82  Linux swap / Solaris
   /dev/sda3            2473        8908    51697170   83  Linux

Reboot
"""""""

::

   $ sudo reboot

Format the partition
"""""""""""""""""""""""""""

* `Reference(EN) <http://www.ibm.com/developerworks/linux/library/l-fs10/index.html>`_
* `Reference(JP) <http://www.ibm.com/developerworks/jp/linux/library/l-fs10/index.html>`_

::

   $ sudo mkfs.xfs -d agcount=4 -l size=32m {TARGET_PARTITION}

Modify the "/etc/fstab" file
""""""""""""""""""""""""""""

::

   $ sudo vi /etc/fstab
   /dev/sda3   /mnt/xfs   xfs   noatime,nodiratime,osyncisdsync 0 0

Mount the partition
"""""""""""""""""""""""""""""""""""""""""""""""

::

   $ sudo mkdir /mnt/xfs
   $ sudo mount -a

Confirm
"""""""""

::

   $ df
   Filesystem           1K-blocks      Used Available Use% Mounted on
   /dev/sda1             15180256   2153492  12243196  15% /
   tmpfs                  2025732         0   2025732   0% /dev/shm
   /dev/sda3             51664400      4

