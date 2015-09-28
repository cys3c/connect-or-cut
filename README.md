# connect-or-cut

Stateless LD_PRELOAD based poor man's firewall.

## What it is

connect-or-cut is a small library to interpose with LD_PRELOAD to a
program to prevent it from connecting where it should not.

This is similar to a firewall, except that:

 * you do not need to be root to use it
 * only processes launched after LD_PRELOAD is set are affected, not
   the full system

## Installation

Provided you have a C compiler, GNU make on a Unix box:

   $ git clone https://github.com/tgg/connect-or-cut.git
   $ cd connect-or-cut
   $ make

## Using it

   $ LD_PRELOAD=./libconnect-or-cut.so COC_ALLOW='212.27.40.240:53;212.27.40.241:53;*.google.com;*.1e100.net' COC_BLOCK='*' iceweasel

This invokes chromium, allowing only outgoing connection to my ISP
provided name servers, to all google.com and 1e100.net addresses.

Everything else will be blocked, with connection blocking messages
displayed on stderr.


## Environment variables

 * `COC_ALLOW` is a comma separated list of addresses to allow
 * `COC_BLOCK` is a comma separated list of addresses to block
 * `COC_LOG_LEVEL` defines the level of log, from `0' (silent) to `4' (debug)
 * `COC_LOG_TARGET` is a bitwise between the following values:
   * `1` log to stderr
   * `2` log to syslog
   * `4` log to a file

## Limitations

 * Only outgoing connection using TCP over IPv4 work for now.
 * Tested only with gcc and clang on Debian GNU/Linux. Portability
   patches welcome!

## Roadmap

 * Add a wrapper script to ease invocation
 * Complete IPv6
 * Make filtering algorithm configurable. For now it's always:
   * check against ALLOW list and ALLOW connection if it's in it;
   * else check against BLOCK list and BLOCK connection if it's in it;
   * else ALLOW by default
 * Implement accept() filtering
 * Port on Windows