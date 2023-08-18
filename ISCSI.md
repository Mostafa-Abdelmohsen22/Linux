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


   

## targetcli
     
6- Install Target Package.
   - yum  install -y targetcli
   - rpm -qa | grep targetcli
       - targetcli-2.1.53-1.el8.noarch
    - targetcli
    - ls

## 7-add the Storage to storage backend as block device or fileio
   - cd /backstores/block

   ### Created block storage object LUN1 using /dev/testvg/lv1.
   - backstores/block>  create dev=/dev/testvg/lv1 name=lun1
   ### Created block storage object LUN2 using /dev/testvg/lv2.
   - backstores/block>  create dev=/dev/testvg/lv2 name=lun2

## 8-Create IQN for the iSCSI target.

   - cd iscsi
### iqn.year-month.reverse_domain_lookup

  - create iqn.2023-16.com.local.srv

  - cd iqn.2023-16.com.local.srv/
    - cd tpg1/acls

    - create iqn.2023-16.com.local.client1

## 9-export block devices as luns
   - cd tpg1/luns
   
    - create /backstores/block/LUN1
