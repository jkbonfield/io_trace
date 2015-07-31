Io_trace
========

This tool provides a simple wrapper around strace to monitor the
amount of I/O performed by a tool.

I/O can be restricted to specific files with prefix or regexp pattern
matching.  It also displays I/O on sockets and stdin/stdout/stderr,
but note that it cannot report how much I/O is performed on mmap()ed
files.



Usage
-----

Usage: io_trace [options] filename ... -- command argument ...
e.g.:  io_trace -x -p foo.bam -- samtools view foo.bam X:100-110

Options:
    -p prefix    Only output filenames matching a specific prefix
    -r regexp    Only output filenames matching a specific regexp
    -x           Suppress files with no I/O performed on them
    -S           Suppress stdin/stdout/stderr and sockets


Example
-------

Comparing a CRAM file against a BAM file with samtools range query.
We can clearly see BAM loads much more (the entirety infact) of the
index, but less data from the BAM file itself due to a smaller block
size although in 3 sections.  CRAM loads only a small amount of the
index, a larger (but single) data block from the cram file and an
unknown amount of reference sequence via mmap.

BAM:

    ./io_trace -S -p /nfs -- samtools view /nfs/sam_scratch/jkb/data/NA20800.chrom20.ILLUMINA.bwa.TSI.low_coverage.20101123.bam 20:20000000-20000000
    
    File: /nfs/sam_scratch/jkb/data/NA20800.chrom20.ILLUMINA.bwa.TSI.low_coverage.20101123.bam.bai
        Num. open           2
        Num. close          2
        Num. mmap           0
        Num. seeks          0
        Num. read           5
        Num. pread          0
        Num. sendto         0
        Num. write          0
        Num. pwrite         0
        Num. recvfrom       0
        Bytes read          179072
        Bytes written       0
    
    File: /nfs/sam_scratch/jkb/data/NA20800.chrom20.ILLUMINA.bwa.TSI.low_coverage.20101123.bam
        Num. open           1
        Num. close          1
        Num. mmap           0
        Num. seeks          3
        Num. read           7
        Num. pread          0
        Num. sendto         0
        Num. write          0
        Num. pwrite         0
        Num. recvfrom       0
        Bytes read          196636
        Bytes written       0


CRAM:

    $ ./io_trace -S -p /nfs -- samtools view /nfs/sam_scratch/jkb/data/NA20800.chrom20.ILLUMINA.bwa.TSI.low_coverage.20101123.cram 20:20000000-20000000
    
    File: /nfs/srpipe_references/cram_cache/0d/ec/9660ec1efaaf33281c0d5ea2560f
        Num. open           1
        Num. close          1
        Num. mmap           1
        Num. seeks          2
        Num. read           0
        Num. pread          0
        Num. sendto         0
        Num. write          0
        Num. pwrite         0
        Num. recvfrom       0
        Bytes read          0
        Bytes written       0
    
    File: /nfs/sam_scratch/jkb/data/NA20800.chrom20.ILLUMINA.bwa.TSI.low_coverage.20101123.cram.crai
        Num. open           2
        Num. close          2
        Num. mmap           0
        Num. seeks          0
        Num. read           3
        Num. pread          0
        Num. sendto         0
        Num. write          0
        Num. pwrite         0
        Num. recvfrom       0
        Bytes read          9725
        Bytes written       0
    
    File: /nfs/sam_scratch/jkb/data/NA20800.chrom20.ILLUMINA.bwa.TSI.low_coverage.20101123.cram
        Num. open           1
        Num. close          1
        Num. mmap           0
        Num. seeks          1
        Num. read           8
        Num. pread          0
        Num. sendto         0
        Num. write          0
        Num. pwrite         0
        Num. recvfrom       0
        Bytes read          547188
        Bytes written       0
