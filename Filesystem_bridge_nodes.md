# Configuring a Filesystem for Bridge Nodes

## Introduction

During the setup of storage nodes, you might encounter filesystem warnings related to EXT4 directories, such as:

```bash
sudo dmesg | grep "ext4_dx_add_entry"
[9047890.965138] EXT4-fs warning (device sdb1): ext4_dx_add_entry:2549: Large directory feature is not enabled on this filesystem
[9047896.714567] EXT4-fs warning (device sdb1): ext4_dx_add_entry:2545: Directory (ino: 143955220) index full, reach max htree level :2
[9047896.714582] EXT4-fs warning (device sdb1): ext4_dx_add_entry:2549: Large directory feature is not enabled on this filesystem
[9047896.714636] EXT4-fs warning (device sdb1): ext4_dx_add_entry:2545: Directory (ino: 143955220) index full, reach max htree level :2
```

This typically happens when the `large_dir` feature is not enabled on an EXT4 filesystem. This guide provides steps to resolve this issue and set up a robust storage solution for your Bridge nodes.

---

## Resolving EXT4 Large Directory Issues

### Step 1: Verify Your Filesystem Type

First, confirm that your partition is using EXT4. Run the following command:

```bash
df -T
```

This will list the filesystem type for each partition. If the partition in question is EXT4, proceed to the next step.

### Step 2: Check for Large Directory Support

To check if the `large_dir` feature is enabled on your EXT4 filesystem, use:

```bash
sudo tune2fs -l /dev/sdX | grep "Filesystem features"
```

Replace `/dev/sdX` with your device name (e.g., `/dev/sdb1`).

If you do not see `large_dir` in the list of filesystem features, you need to enable it.

### Step 3: Enable the Large Directory Feature

If `large_dir` is not enabled, activate it with the following command:

```bash
sudo tune2fs -O dir_index,large_dir /dev/sdX
```

This command enables both directory indexing and the large directory feature for the EXT4 filesystem.

### Step 4: Repair the Filesystem

After enabling the feature, repair the filesystem to apply the changes. Ensure the partition is unmounted before proceeding:

```bash
sudo umount /dev/sdX
sudo fsck /dev/sdX
```

If the partition is your root filesystem, you will need to boot from a live system (e.g., SystemRescueCD) to unmount and modify it.

### Step 5: Restart and Verify

Reboot your system and check the logs to ensure the warnings no longer appear:

```bash
sudo journalctl -xe
```

---

## Setting Up ZFS for Bridge Nodes

If you have a 17.5 TB disk without any partitions, a good approach is to use it with ZFS for efficient storage management. Follow these steps to set up `/dev/sdb` for your Bridge node:

### Step 1: Create a New Partition on `/dev/sdb`

Use `fdisk` or `parted` to create a new partition on `/dev/sdb`:

```bash
sudo fdisk /dev/sdb
```

Inside `fdisk`:
1. Use the `n` command to create a new partition.
2. Create a new GPT partition table using:
   ```
   g
   ```
3. Allocate the entire disk space to the partition using the default values.
4. Save the changes by typing:
   ```
   w
   ```

### Step 2: Install ZFS

Install ZFS utilities if they are not already installed:

```bash
sudo apt update
sudo apt install zfsutils-linux
```

### Step 3: Create a ZFS Pool

Create a ZFS pool named `mypool` on `/dev/sdb1`:

```bash
sudo zpool create mypool /dev/sdb1
```

**Note**: If the command fails, reboot your system and try again.

### Step 4: Mount the ZFS Volume

ZFS typically handles mounting automatically. To manually set the mount point:

```bash
sudo mkdir /mnt/data
sudo zfs set mountpoint=/mnt/data mypool
```

Verify the mount:

```bash
df -h
ls /mnt/data
```

---

## Final Verification

Ensure that your filesystem (EXT4 or ZFS) is configured correctly and verify the setup using:

```bash
sudo zpool status
sudo tune2fs -l /dev/sdX
```
