*02-09-2025 16:07*
### Status: 
Finished Working Frozen
### Tags: [[Linux]], [[Learning]]


# Linux LVM extending

So , if we want to extend our **vg group**
`myvg` , we need to initialize `pv` on `sdc`
```bash
sdb            8:16   0     1G  0 disk 
├─sdb1         8:17   0   200M  0 part 
│ └─myvg-lv1 252:0    0   196M  0 lvm  /mydata
└─sdb2         8:18   0   400M  0 part 
sdc            8:32   0     1G  0 disk 
```

> If cannot create physical volume on sdc, try to wipe all data  

```bash 
wypefs -a /dev/sdc
```



# References:

- 
  