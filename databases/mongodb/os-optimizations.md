When setting up a new mongodb instance or cluster, you can do a few things on the os level to make it run better (according to https://www.mongodb.com/blog/post/performance-best-practices-hardware-and-os-configuration)

Format your filesystem to XFS, as wirdtiger might have some issues with EXT4

Add `transparent_hugepage=never` to grub.conf (can check if its already disabled at /sys/kernel/mm/transparent_hugepage/enabled)

Add noatime on the parameters at fstab, this disables writing metadata to files whenever they're accessed. Its a small overhead, but an overhead nonetheless

Set cache-settings according to [this file](cache-settings.md)

Configure readahead via `blockdev --setra <value> <device>", with a value between 8 and 32

Set swappiness to 1 or 0, via adding `vm.swappiness = <value>` to /etc/sysctl.conf, and apply via sysctl -p

