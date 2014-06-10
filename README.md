iperf3:  A TCP, UDP, and SCTP network bandwidth measurement tool
================================================================

Summary
-------

iperf is a tool for active measurements of the maximum achievable
bandwidth on IP networks.  It supports tuning of various parameters
related to timing, protocols, and buffers.  For each test it reports
the bandwidth, loss, and other parameters.

This version, sometimes referred to as iperf3, is a redesign of an
original version developed at NLANR/DAST.  iperf3 is a new
implementation from scratch, with the goal of a smaller, simpler code
base, and a library version of the functionality that can be used in
other programs. iperf3 also a number of features found in other tools
such as nuttcp and netperf, but were missing from the original iperf.
These include, for example, a zero-copy mode and optional JSON output.
Note that iperf3 is NOT backwards compatible with the original iperf.

Primary development for iperf3 takes place on CentOS Linux, FreeBSD,
and MacOS X.  At this time, these are the only officially supported
platforms, however there have been some reports of success with
OpenBSD, Android, other Linux distributions.

iperf3 is principally developed by ESnet / Lawrence Berkleley National
Laboratory.  It is released under a three-clause BSD license.

For more information see: http://software.es.net/iperf

Source code and issue tracker: https://github.com/esnet/iperf

Obtaining iperf3
----------------

Downloads of iperf3 are available at:

    http://downloads.es.net/pub/iperf/

To check out the most recent code, do:

    git clone https://github.com/esnet/iperf.git

Building iperf3
---------------

### Prerequisites: ###
  * libuuid: this is not installed by default for Debian/Ubuntu Systems
             to install: `apt-get install uuid-dev`

### Building ###

    ./configure; make; make install

(Note: If configure fails, try running `./bootstrap.sh` first)

Invoking iperf3
---------------

iperf3 includes a manual page listing all of the command-line options.
The manual page is the most up-to-date reference to the various flags and parameters.

For sample command line usage, see: 

http://fasterdata.es.net/performance-testing/network-troubleshooting-tools/iperf-and-iperf3/

Using the default options, iperf is meant to show typical well
designed application performance.  'Typical well designed application'
means avoiding artificial enhancements that work only for testing
(such as splice()'ing the data to /dev/null).  iperf does also have
flags for 'extreme best case' optimizations but they must be
explicitly activated.

These flags include:

    -Z, --zerocopy            use a 'zero copy' sendfile() method of sending data
    -A, --affinity n/n,m      set CPU affinity

Bug Reports
-----------

Before submitting a bug report, try checking out the latest version of
the code, and confirm that its not already fixed. Then submit to:
https://github.com/esnet/iperf/issues

Changes from iperf 2.x
----------------------

New options:

    -V, --verbose             more detailed output than before
    -J, --json                output in JSON format
    -Z, --zerocopy            use a 'zero copy' sendfile() method of sending data
    -O, --omit N              omit the first n seconds (to ignore slowstart)
    -T, --title str           prefix every output line with this string
    -F, --file name           xmit/recv the specified file
    -A, --affinity n/n,m      set CPU affinity (Linux only)
    -k, --blockcount #[KMG]   number of blocks (packets) to transmit (instead 
                              of -t or -n)
    -L, --flowlabel           set IPv6 flow label (Linux only)

Changed flags:

    -C, --linux-congestion    set congestion control algorithm (Linux only)
                              (-Z in iperf2)


Deprecated options:

Not planning to support these iperf2 flags. If you really miss these
options, please submit a request in the issue tracker:

    -d, --dualtest           Do a bidirectional test simultaneously
    -r, --tradeoff           Do a bidirectional test individually
    -T, --ttl                time-to-live, for multicast (default 1)
    -x, --reportexclude [CDMSV]   exclude C(connection) D(data) M(multicast) 
                                  S(settings) V(server) reports
    -y, --reportstyle C      report as a Comma-Separated Values

Also deprecated is the ability to set the options via environment
variables.

Known Issues
------------

* UDP performance: Some problems have been noticed with iperf3 on the
ESnet 100G testbed at high UDP rates (above 10Gbps).  The symptom is
that on any particular run of iperf3 the receiver reports a loss rate
of about 20%, regardless of the -b option used on the client side.
This problem appears not to be iperf3-specific, and may be due to the
placement of the iperf3 process on a CPU and its relation to the
inbound NIC.  In some cases this problem can be mitigated by an
appropriate use of the CPU affinity (-A) option.  (Issue #55)

* The -Z flag sometimes causes the iperf3 client to hang on OSX.
(Issue #129)

* On OpenBSD, the server seems to require a "-4" argument, implying
that it can only be used with IPv4.

* When specifying the TCP buffer size using the "-w" flag on Linux, Linux 
doubles the value you pass in. (You can see this using iperf3's debug flag). 
But then the CWND does not actually ramp up to the doubled value, but only
to about 75% of the doubled value. This appears to be by design.

