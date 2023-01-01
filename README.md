# Benchmarking

## Reading and Writing

Stats from a Western Digital SN750, rated at 3400 MB/s sequential read, 2900 MB/s sequential write.
Only reading with io_uring comes close to using the full bandwidth of the SSD on read, while still being fairly slow on write.

Fsync also seems to have a lot of overhead when using `write()` and `aio_write()` (causing a 3x slowdown), but far less overhead with `io_uring` (20% overhead on sequential writes, 50% overhead on random writes).

synchronous sequential write - fsync: ~2.4k ops/s, (623MB/s)
synchronous sequential write + fsync: ~850 ops/s, (222MB/s)
synchronous random write - fsync: ~2.4k ops/s, (644MB/s)
synchronous random write + fsync: ~800 ops/s, (214MB/s)
synchronous sequential read: ~2.9k ops/s, (757MB/s)
synchronous random read: ~2.2k ops/s, (575MB/s)

asynchronous sequential write - fsync: ~2.3k ops/s, (599MB/s)
asynchronous sequential write + fsync: ~800 ops/s, (212MB/s)
asynchronous random write - fsync: ~2.3k ops/s, (592MB/s)
asynchronous random write + fsync: ~800 ops/s, (217MB/s)
asynchronous sequential read: ~3k ops/s, (763MB/s)
asynchronous random read: ~2.1k ops/s, (561MB/s)

io_uring sequential write - fsync: ~2.8k ops/s, (723MB/s)
io_uring sequential write + fsync: ~2.5k ops/s, (653MB/s)
io_uring random write - fsync: ~3.3k ops/s, (872MB/s)
io_uring random write + fsync: ~2.4k ops/s, (639MB/s)
io_uring sequential read: ~15k ops/s, (3876MB/s)
io_uring random read: ~7.5k ops/s, (1997MB/s)

## Networks

Net read/write TCP: ~500k ops/s (15.1Gbit/s)

## System

Mutex lock/unlock w/ libpthread: (6ns)
System call: (114.8ns)
Process context switch: (~2.5μs)
Thread context switches w/ futex: (~2.2μs)
Thread context switches w/ sched_yield: (~374.2ns)

## Historical

8-track tape (1960s-1980s): ~100B/s (Read-only)
Cassette Tape (1970s-1980s): ~50B/s (Read-only)
IBM 3420 magnetic tape drive (1985): ~1MB/s
CD Drive (1980s-1990s): 150KB/s (1X) - 3.6MB/s (24X) (Read-only)
Floppy Disk (1980s-1990s): ~100kb/s (R/W)
USB Drive (2000s): ~20MB/s (R/W)
HDD Drive (2000s): ~120MB/s @ 7200rps (R/W)
