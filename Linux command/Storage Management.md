### So remember

| Term            | Usually means                                                     |     |
| --------------- | ----------------------------------------------------------------- | --- |
| **Disk**        | Physical storage device                                           |     |
| **Partition**   | A section of a disk                                               |     |
| **Drive**       | Usually a logical storage unit, but can also mean physical device |     |
| **Filesystem**  | How data is organized on a partition                              |     |
| **Mount point** | Where Linux makes that filesystem accessible                      |     |
|                 |                                                                   |     |
/mnt → commonly used for manually configured mounts 
/run/media/<user> → commonly used by desktop automounting for user/removable storage
Mount = make a filesystem accessible at a directory in Linux.
Linux uses a **mount system** because it wants to present all storage as **one unified directory tree**.
Yes — **in Linux, a partition can be represented as a device**, but technically **partition and device are not exactly the same thing**.

LVM: LVM is used to manage volume and disk on the Linux server
Logical Volume Manager allows disks to be combined together