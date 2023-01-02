# Benchmarking

## Reading and Writing

Stats from a Western Digital SN750, rated at 3400 MB/s sequential read, 2900 MB/s sequential write.
Only reading with io_uring comes close to using the full bandwidth of the SSD on read, while still being fairly slow on write.

Fsync also seems to have a lot of overhead when using `write()` and `aio_write()` (causing a 3x slowdown), but far less overhead with `io_uring` (20% overhead on sequential writes, 50% overhead on random writes).

**TODO: Fix benchmarks. These benchmarks are the same write speed regardless of the method used, which is most likely not correct. Need a benchmark that exploits more parallelism.**

- synchronous sequential write - fsync (1MB bs): 248 ops/s, (1041MB/s)
- synchronous sequential write - fsync (256KB bs): ~2.4k ops/s, (623MB/s)
- synchronous sequential write + fsync: ~850 ops/s, (222MB/s)
- synchronous random write - fsync: ~2.4k ops/s, (644MB/s)
- synchronous random write + fsync: ~800 ops/s, (214MB/s)
- synchronous sequential read: ~2.9k ops/s, (757MB/s)
- synchronous random read: ~2.2k ops/s, (575MB/s)

- asynchronous sequential write - fsync: ~2.3k ops/s, (599MB/s)
- asynchronous sequential write + fsync: ~800 ops/s, (212MB/s)
- asynchronous random write - fsync: ~2.3k ops/s, (592MB/s)
- asynchronous random write + fsync: ~800 ops/s, (217MB/s)
- asynchronous sequential read: ~3k ops/s, (763MB/s)
- asynchronous random read: ~2.1k ops/s, (561MB/s)

- io_uring sequential write - fsync: ~2.8k ops/s, (723MB/s)
- io_uring sequential write + fsync: ~2.5k ops/s, (653MB/s)
- io_uring random write - fsync: ~3.3k ops/s, (872MB/s)
- io_uring random write + fsync: ~2.4k ops/s, (639MB/s)
- io_uring sequential read: ~15k ops/s, (3876MB/s)
- io_uring random read: ~7.5k ops/s, (1997MB/s)

## Networks

- Net read/write TCP: ~500k ops/s (15.1Gbit/s)

## System

- Mutex lock/unlock w/ libpthread: (6ns)
- System call: (114.8ns)
- Process context switch: (~2.5μs)
- Thread context switches w/ futex: (~2.2μs)
- Thread context switches w/ sched_yield: (~374.2ns)

## Hashing

md5:              404.15 MiB/sec
crc64:            258.36 MiB/sec
crc32:            230.60 MiB/sec
crc32c:          4682.73 MiB/sec
crc16:            252.24 MiB/sec
crc7:             258.89 MiB/sec
sha1:             432.36 MiB/sec
sha256:           187.43 MiB/sec
sha512:           302.18 MiB/sec
xxhash:          1827.80 MiB/sec
murmur3:         1826.07 MiB/sec
jhash:           1542.84 MiB/sec
fnv:             3720.28 MiB/sec
sha3-224:          59.47 MiB/sec
sha3-256:          56.14 MiB/sec
sha3-384:          42.18 MiB/sec
sha3-512:          24.17 MiB/sec

## Historical

- 8-track tape (1960s-1980s): ~100B/s (Read-only)
- Cassette Tape (1970s-1980s): ~50B/s (Read-only)
- IBM 3420 magnetic tape drive (1985): ~1MB/s
- CD Drive (1980s-1990s): 150KB/s (1X) - 3.6MB/s (24X) (Read-only)
- Floppy Disk (1980s-1990s): ~100kb/s (R/W)
- USB Drive (2000s): ~20MB/s (R/W)
- HDD Drive (2000s): ~120MB/s @ 7200rps (R/W)
