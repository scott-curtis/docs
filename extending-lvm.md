//////////////////////////////////////////////////////////////
/                  |    |                      |    o        /
/   ,---.,---.,---.|--- |---    ,---..   .,---.|--- .,---.   /
/   `---.|    |   ||    |    ---|    |   ||    |    |`---.   /
/   `---'`---'`---'`---'`---'   `---'`---'`    `---'``---'   /
/   MIT License                                              /
/   Date:                                                    /
//////////////////////////////////////////////////////////////

# Extending the disk

Extend the disk in your hypervisor

Extend the GPT partition
`sudo parted -l`
Type "Fix" if you're prompted.

# Extending the partition

Locate the disk with `fdisk -l`
`sudo fdisk /dev/XXXX n`
*n means New partition*
Press Enter 3 times.

- Default Partition number (new, unused)
- Default First Sector (1 past last partition)
- Default End Sector (last on disk)
    `t`
    *t means change partition Type*
    `lvm` (or 43, if no alias is available)
    `w`
    *w means write*

# Extending your Physical Volume

Create a new pv pointing to your new partition
pvcreate /dev/XXXXX
vgdisplay
pvdisplay
vgextend XXXXXXXX /dev/XXXX
lvdisplay

`lvextend -l +100%FREE LV_Path`
or
`lvextend -L +10G`
*Note the capitalization of the L for different units*

`lvextend -r -l +100%FREE /dev/LV/path`
*r Resizes the partition (to use new, empty space. Equivalent to resize2fs and xfs_growfs.)
