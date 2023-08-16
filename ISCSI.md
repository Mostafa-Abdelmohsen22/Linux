# Steps
1- Add 3 Disks To SRV
2-Create Storage with RAID 5 Device.
  -  mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/nvme0n2 /dev/nvme0n3 /dev/nvme0n4
  - cat /proc/mdstat
    Create PV.
   - pvcreate /dev/md0
   - pvdisplay
4- Create VG.
   - vgcreate v1 /dev/md0
   - vgdisplay
5- Create LV.
   - lvcreate -n lv1 -L 15G v1
   - lvcreate -n lv2 -l 100%free v1
   - lvdisplay
