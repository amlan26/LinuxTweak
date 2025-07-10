# 📦 Mounting an Additional Disk to a Custom Directory on Linux

This guide provides step-by-step instructions for mounting a new disk (e.g., `/dev/vdb`) to a custom directory such as `/mongo-data` on a Linux system. Ideal for use cases like attaching storage for MongoDB or application data.

---

## 🛠️ Step 1: Format the Disk

> ⚠️ Only run this if the disk is unformatted. Formatting **erases all data** on the disk.

Check if the disk is formatted:

```bash
sudo file -s /dev/vdb
```

If the result says "data" or "empty", format it:

```bash
sudo mkfs.ext4 /dev/vdb
```

> Replace `/dev/vdb` with your actual disk device if different.

---

## 📁 Step 2: Create the Mount Point

Choose a directory name where you want to mount the disk. A common and clear name for MongoDB is:

```bash
sudo mkdir -p /mongo-data
```

> You may replace `/mongo-data` with any directory name of your choice like `/mnt/mongo`, `/data`, etc.

---

## 🔗 Step 3: Mount the Disk Temporarily

Mount the disk:

```bash
sudo mount /dev/vdb /mongo-data
```

Check if it’s mounted successfully:

```bash
df -h | grep /mongo-data
```

---

## 💾 Step 4: Make the Mount Persistent

Edit the `/etc/fstab` file so the disk mounts automatically after a reboot:

```bash
sudo nano /etc/fstab
```

Add this line at the bottom:

```bash
/dev/vdb  /mongo-data  ext4  defaults  0  2
```

Save and exit (`Ctrl+O`, then `Enter`, then `Ctrl+X`).

> 📌 Make sure the path `/mongo-data` matches your chosen directory.

---

## 🔁 Step 5: Reboot and Verify

Reboot the server to confirm that the disk mounts automatically:

```bash
sudo reboot
```

After reboot, verify:

```bash
df -h | grep /mongo-data
```

✅ You should see something like:

```bash
/dev/vdb        99G   1.5G   92G   2% /mongo-data
```

---

## 📌 Notes

- You can use any mount point name — just stay consistent.
- Using `/dev/vdb` in `/etc/fstab` is fine, but using `UUID` is safer across reboots if device names change.
- For production MongoDB setups, consider tuning mount options (e.g., `noatime`, `nobarrier`) and disk I/O settings.