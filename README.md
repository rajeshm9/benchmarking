Role Name
=========

This roles uses various benchmarking tool for benchmarking linux system

Requirements
------------
Ubuntu



Role Variables
--------------
All configured variable are define in vars/main.yml.


Dependencies
------------
ansible


IOPS Benchmarking:
------------

For IOPS benchmark we are using fio and sysbench tool to calculate the Input/Output operation par second,

Following basic command used and this will create a 4 GB file, and perform 4KB reads and writes using a 75%/25% (ie 3 reads are performed for every 1 write) split within the file, with 64 operations running at a time. The 3:1 ratio is a rough approximation of your typical database. You can change it as per your need. 

fio -randrepeat=1 -ioengine=libaio -direct=1 -gtod_reduce=1 -name=test -filename=test -bs=4k -iodepth=64 -size=10M -readwrite=randrw -rwmixread=75 --group-reporting

We can change file size, block size,read write ratio from ansible configurable parameters.

IOPS_FILESIZE: 10M
IOPS_RW : 75

CPU Benchmarking:
------------

For Cpu benchmark we are using sysbench and openssl speed commad

When running with the CPU workload, sysbench will verify prime numbers by doing standard division of the number by all numbers between 2 and the square root of the number. If any number gives a remainder of 0, the next number is calculated.Following command is used

sysbench --test=cpu --cpu-max-prime=20000 --num-threads=2 run

We can change cpu max prime parameter from ansible configurable parameters.

CPU_PRIME: 2000

While benchmarking cpu though openssl speed rsa commmand
 

Memory Benchmarking:
------------

For memory benchmark we are using sysbench command

When using the memory workload, sysbench will allocate a buffer (provided through the --memory-block-size parameter, defaults to 1kbyte) and each execution will read or write to this memory (--memory-oper, defaults to write) in a random or sequential manner (--memory-access-mode, defaults to sequential).

sysbench memory --memory-block-size={{MEM_BLK_SIZE}} --memory-total-size={{MEM_TOT_SIZE}} --memory-access-mode={{MEM_ACC_MODE}} --threads={{ansible_processor_vcpus}} run

We can change memory block size ,memory total size, memory access mode from  ansible configurable parameter

MEM_BLK_SIZE : 4K

MEM_TOT_SIZE : 100M

MEM_ACC_MODE: rnd


Network Benchmarking:
------------

Netperf  is a benchmark that can be used to measure various aspects of networking performance.  Currently, its focus is on bulk data transfer and request/response performance usingeither TCP or UDP, and the Berkeley Sockets interface.Other tool we can use are iperf

Following command is used to find the network performace of local network transfer.

netperf  -c  -f G

We can change Output unit in following format

NET_UNIT: G (Possible values are G|M|K|g|m|k)
       


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: server
      roles:
         - benchmarking

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
