#############################################
## The Datera Deployment Check Tool (DDCT) ##
#############################################

-------
Purpose
-------
This tool is designed to be run on customer systems as an easy way to determine
if they are currently configured correctly (by the standards of the tool) for
starting a set of PoC tests.

Once the set of checks has been run a report file will be generated.  This
report file can then be fed back into the tool to have it try and fix each
encountered problem.

------
Checks
------

* ARP
* IRQ
* CPU Frequency
* Block Devices
* Multipath
* Cinder Volume Driver

-------------
Future Checks
-------------

* Cinder Backup Driver
* Glance Image Backend Driver
* Nova Ephemeral Driver
* Docker Driver
* Kubernetes Driver

-----
Usage
-----

To perform basic readiness checks:
$ ./ddct

The report will have the following format:

```
+-------+--------+---------+
| Test  | Status | Reasons |
+=======+========+=========+
```

Below is an example output.  Tests with multiple failure conditions will have
all currently check-able reasons listed.
```
+---------------+----------+-------------------------------------------------------+
| Test          | Status   | Reasons                                               |
+===============+==========+=======================================================+
| ARP           | FAIL     | net.ipv4.conf.all.arp_announce != 2 in sysctl         |
|               |          | net.ipv4.conf.all.arp_ignore != 1 in sysctl           |
+---------------+----------+-------------------------------------------------------+
| IRQ           | FAIL     | irqbalance is active                                  |
+---------------+----------+-------------------------------------------------------+
| CPUFREQ       | FAIL     | cpupower is not installed                             |
+---------------+----------+-------------------------------------------------------+ 
| Block Devices | FAIL     | Scheduler is not set to noop                          |
+---------------+----------+-------------------------------------------------------+
| Multipath     | FAIL     | Multipath binary could not be found, is it installed? |
+---------------+----------+-------------------------------------------------------+
| VIP2          | FAIL     | Could not ping vip2 ip 172.29.41.9                    |
+---------------+----------+-------------------------------------------------------+
| CONFIG        | Success  |                                                       |
+---------------+----------+-------------------------------------------------------+
| OS            | Success  |                                                       |
+---------------+----------+-------------------------------------------------------+
| MGMT          | Success  |                                                       |
+---------------+----------+-------------------------------------------------------+
| VIP1          | Success  |                                                       |
+---------------+----------+-------------------------------------------------------+
| Cinder Volume | Success  |                                                       |
+---------------+----------+-------------------------------------------------------+
```

The tool should be run until all checks show "Success"
