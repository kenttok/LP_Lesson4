Vagrantfile содержит Bash-скрипт.

Выводы команд ниже:

[root@zfs vagrant]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  512M  0 disk 
|-sda1   8:1    0  502M  0 part 
`-sda9   8:9    0    8M  0 part 
sdb      8:16   0  512M  0 disk 
|-sdb1   8:17   0  502M  0 part 
`-sdb9   8:25   0    8M  0 part 
sdc      8:32   0  512M  0 disk 
|-sdc1   8:33   0  502M  0 part 
`-sdc9   8:41   0    8M  0 part 
sdd      8:48   0  512M  0 disk 
|-sdd1   8:49   0  502M  0 part 
`-sdd9   8:57   0    8M  0 part 
sde      8:64   0  512M  0 disk 
|-sde1   8:65   0  502M  0 part 
`-sde9   8:73   0    8M  0 part 
sdf      8:80   0  512M  0 disk 
|-sdf1   8:81   0  502M  0 part 
`-sdf9   8:89   0    8M  0 part 
sdg      8:96   0  512M  0 disk 
|-sdg1   8:97   0  502M  0 part 
`-sdg9   8:105  0    8M  0 part 
sdh      8:112  0  512M  0 disk 
|-sdh1   8:113  0  502M  0 part 
`-sdh9   8:121  0    8M  0 part 
sdi      8:128  0   40G  0 disk 
`-sdi1   8:129  0   40G  0 part /

[root@zfs vagrant]# zpool list
NAME    SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
otus    480M  5.02M   475M        -         -     0%     1%  1.00x    ONLINE  -
otus1   480M  21.6M   458M        -         -     0%     4%  1.00x    ONLINE  -
otus2   480M  17.7M   462M        -         -     0%     3%  1.00x    ONLINE  -
otus3   480M  10.8M   469M        -         -     0%     2%  1.00x    ONLINE  -
otus4   480M  39.2M   441M        -         -     0%     8%  1.00x    ONLINE  -

[root@zfs vagrant]# zpool status
  pool: otus
 state: ONLINE
  scan: none requested
config:

        NAME                                 STATE     READ WRITE CKSUM
        otus                                 ONLINE       0     0     0
          mirror-0                           ONLINE       0     0     0
            /home/vagrant/zpoolexport/filea  ONLINE       0     0     0
            /home/vagrant/zpoolexport/fileb  ONLINE       0     0     0

errors: No known data errors

  pool: otus1
 state: ONLINE
  scan: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        otus1       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdb     ONLINE       0     0     0
            sdc     ONLINE       0     0     0

errors: No known data errors

  pool: otus2
 state: ONLINE
  scan: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        otus2       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdd     ONLINE       0     0     0
            sde     ONLINE       0     0     0

errors: No known data errors

  pool: otus3
 state: ONLINE
  scan: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        otus3       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdf     ONLINE       0     0     0
            sdg     ONLINE       0     0     0

errors: No known data errors

  pool: otus4
 state: ONLINE
  scan: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        otus4       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdh     ONLINE       0     0     0
            sda     ONLINE       0     0     0

errors: No known data errors

[root@zfs vagrant]# zfs get all | grep compression
otus             compression           zle                    local
otus/hometask2   compression           zle                    inherited from otus
otus/test        compression           zle                    inherited from otus
otus1            compression           lzjb                   local
otus2            compression           lz4                    local
otus3            compression           gzip-9                 local
otus4            compression           zle                    local

[root@zfs vagrant]# ls -l /otus*
/otus:
total 4
drwxr-xr-x. 102 root root 102 May 15  2020 hometask2
drwxr-xr-x.   3 root root  11 May 15  2020 test

/otus1:
total 22048
-rw-r--r--. 1 root root 40931771 May  2 08:18 pg2600.converter.log

/otus2:
total 17985
-rw-r--r--. 1 root root 40931771 May  2 08:18 pg2600.converter.log

/otus3:
total 10955
-rw-r--r--. 1 root root 40931771 May  2 08:18 pg2600.converter.log

/otus4:
total 40000
-rw-r--r--. 1 root root 40931771 May  2 08:18 pg2600.converter.log

[root@zfs vagrant]# zfs list
NAME             USED  AVAIL     REFER  MOUNTPOINT
otus            4.96M   347M       25K  /otus
otus/hometask2  1.88M   347M     1.88M  /otus/hometask2
otus/test       2.85M   347M     2.83M  /otus/test
otus1           21.6M   330M     21.6M  /otus1
otus2           17.7M   334M     17.6M  /otus2
otus3           10.8M   341M     10.7M  /otus3
otus4           39.2M   313M     39.1M  /otus4

[root@zfs vagrant]# zfs get all | grep compressratio | grep -v refotus             compressratio         1.23x                  -
otus/hometask2   compressratio         1.00x                  -
otus/test        compressratio         1.32x                  -
otus/test@today  compressratio         1.32x                  -
otus1            compressratio         1.81x                  -
otus2            compressratio         2.22x                  -
otus3            compressratio         3.65x                  -
otus4            compressratio         1.00x                  -

[root@zfs vagrant]# zfs get all otus
NAME  PROPERTY              VALUE                  SOURCE
otus  type                  filesystem             -
otus  creation              Fri May 15  4:00 2020  -
otus  used                  4.96M                  -
otus  available             347M                   -
otus  referenced            25K                    -
otus  compressratio         1.23x                  -
otus  mounted               yes                    -
otus  quota                 none                   default
otus  reservation           none                   default
otus  recordsize            128K                   local
otus  mountpoint            /otus                  default
otus  sharenfs              off                    default
otus  checksum              sha256                 local
otus  compression           zle                    local
otus  atime                 on                     default
otus  devices               on                     default
otus  exec                  on                     default
otus  setuid                on                     default
otus  readonly              off                    default
otus  zoned                 off                    default
otus  snapdir               hidden                 default
otus  aclinherit            restricted             default
otus  createtxg             1                      -
otus  canmount              on                     default
otus  xattr                 on                     default
otus  copies                1                      default
otus  version               5                      -
otus  utf8only              off                    -
otus  normalization         none                   -
otus  casesensitivity       sensitive              -
otus  vscan                 off                    default
otus  nbmand                off                    default
otus  sharesmb              off                    default
otus  refquota              none                   default
otus  refreservation        none                   default
otus  guid                  14592242904030363272   -
otus  primarycache          all                    default
otus  secondarycache        all                    default
otus  usedbysnapshots       0B                     -
otus  usedbydataset         25K                    -
otus  usedbychildren        4.93M                  -
otus  usedbyrefreservation  0B                     -
otus  logbias               latency                default
otus  objsetid              54                     -
otus  dedup                 off                    default
otus  mlslabel              none                   default
otus  sync                  standard               default
otus  dnodesize             legacy                 default
otus  refcompressratio      1.04x                  -
otus  written               25K                    -
otus  logicalused           4.57M                  -
otus  logicalreferenced     13K                    -
otus  volmode               default                default
otus  filesystem_limit      none                   default
otus  snapshot_limit        none                   default
otus  filesystem_count      none                   default
otus  snapshot_count        none                   default
otus  snapdev               hidden                 default
otus  acltype               off                    default
otus  context               none                   default
otus  fscontext             none                   default
otus  defcontext            none                   default
otus  rootcontext           none                   default
otus  relatime              off                    default
otus  redundant_metadata    all                    default
otus  overlay               off                    default
otus  encryption            off                    default
otus  keylocation           none                   default
otus  keyformat             none                   default
otus  pbkdf2iters           0                      default
otus  special_small_blocks  0                      default

[root@zfs vagrant]# zfs get available otus
NAME  PROPERTY   VALUE  SOURCE
otus  available  347M   -

[root@zfs vagrant]# zfs get readonly otus
NAME  PROPERTY  VALUE   SOURCE
otus  readonly  off     default

[root@zfs vagrant]# zfs get recordsize otus
NAME  PROPERTY    VALUE    SOURCE
otus  recordsize  128K     local

[root@zfs vagrant]# zfs get compression otus
NAME  PROPERTY     VALUE     SOURCE
otus  compression  zle       local

[root@zfs vagrant]# zfs get checksum otus
NAME  PROPERTY  VALUE      SOURCE
otus  checksum  sha256     local

[root@zfs vagrant]# cat /otus/test/task1/file_mess/secret_message
https://github.com/sindresorhus/awesome
