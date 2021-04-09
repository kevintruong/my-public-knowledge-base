# Cache (computing)

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/Cache%2Cbasic.svg/320px-Cache%2Cbasic.svg.png)

Diagram of a CPU memory cache operation

In [computing](https://en.wikipedia.org/wiki/Computing "Computing"), a **cache** is a hardware or software component
that stores data so that future requests for that data can be served faster; the data stored in a cache might be the
result of an earlier computation or a copy of data stored elsewhere. A _cache hit_ occurs when the requested data can be
found in a cache, while a _cache miss_ occurs when it cannot. Cache hits are served by reading data from the cache,
which is faster than recomputing a result or reading from a slower data store; thus, the more requests that can be
served from the cache, the faster the system
performs.[\[3\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-3)

To be cost-effective and to enable efficient use of data, caches must be relatively small. Nevertheless, caches have
proven themselves in many areas of computing, because
typical [computer applications](https://en.wikipedia.org/wiki/Application_software "Application software") access data
with a high degree
of [locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference "Locality of reference"). Such access
patterns exhibit temporal locality, where data is requested that has been recently requested already,
and [spatial](https://en.wikipedia.org/wiki/Memory_address "Memory address") locality, where data is requested that is
stored physically close to data that has already been requested.

### Motivation

There is an inherent trade-off between size and speed (given that a larger resource implies greater physical distances)
but also a tradeoff between expensive, premium technologies (such
as [SRAM](https://en.wikipedia.org/wiki/Static_random-access_memory "Static random-access memory")) vs cheaper, easily
mass-produced commodities (such as [DRAM](https://en.wikipedia.org/wiki/DRAM "DRAM")
or [hard disks](https://en.wikipedia.org/wiki/Hard_disk "Hard disk")).

The [buffering] provided by a cache benefits both [latency](https://en.wikipedia.org/wiki/Latency_(engineering) "Latency (engineering)")
and [throughput](https://en.wikipedia.org/wiki/Throughput "Throughput") ([bandwidth](https://en.wikipedia.org/wiki/Bandwidth_(computing) "Bandwidth (computing)")):

### Latency

A larger resource incurs a significant latency for access – e.g. it can take hundreds of clock cycles for a modern 4 GHz
processor to reach [DRAM](https://en.wikipedia.org/wiki/DRAM "DRAM"). This is mitigated by reading in large chunks, in
the hope that subsequent reads will be from nearby locations. Prediction or
explicit [prefetching](https://en.wikipedia.org/wiki/CPU_cache#prefetch "CPU cache") might also guess where future reads
will come from and make requests ahead of time; if done correctly the latency is bypassed altogether.

### Throughput

The use of a cache also allows for higher throughput from the underlying resource, by assembling multiple fine grain
transfers into larger, more efficient requests. In the case
of [DRAM](https://en.wikipedia.org/wiki/Dynamic_random-access_memory "Dynamic random-access memory") circuits, this
might be served by having a wider data bus. For example, consider a program accessing bytes in a
32-bit [address space](https://en.wikipedia.org/wiki/Address_space "Address space"), but being served by a 128-bit
off-chip data bus; individual uncached byte accesses would allow only 1/16th of the total bandwidth to be used, and 80%
of the data movement would be memory addresses instead of data itself. Reading larger chunks reduces the fraction of
bandwidth required for transmitting address information.

### Operation

Hardware implements cache as a [block](https://en.wikipedia.org/wiki/Block_(data_storage) "Block (data storage)") of
memory for temporary storage of data likely to be used
again. [Central processing units](https://en.wikipedia.org/wiki/Central_processing_unit "Central processing unit") (
CPUs) and [hard disk drives](https://en.wikipedia.org/wiki/Hard_disk_drive "Hard disk drive") (HDDs) frequently use a
cache, as do [web browsers](https://en.wikipedia.org/wiki/Web_browser "Web browser")
and [web servers](https://en.wikipedia.org/wiki/Web_server "Web server").

A cache is made up of a pool of entries. Each entry has associated _data_, which is a copy of the same data in some _
backing store_. Each entry also has a _tag_, which specifies the identity of the data in the backing store of which the
entry is a copy. Tagging allows simultaneous cache-oriented algorithms to function in multilayered fashion without
differential relay interference.

When the cache client (a CPU, web
browser, [operating system](https://en.wikipedia.org/wiki/Operating_system "Operating system")) needs to access data
presumed to exist in the backing store, it first checks the cache. If an entry can be found with a tag matching that of
the desired data, the data in the entry is used instead. This situation is known as a cache hit. For example, a web
browser program might check its local cache on disk to see if it has a local copy of the contents of a web page at a
particular [URL](https://en.wikipedia.org/wiki/URL "URL"). In this example, the URL is the tag, and the content of the
web page is the data. The percentage of accesses that result in cache hits is known as the **hit rate** or **hit ratio**
of the cache.

The alternative situation, when the cache is checked and found not to contain any entry with the desired tag, is known
as a cache miss. This requires a more expensive access of data from the backing store. Once the requested data is
retrieved, it is typically copied into the cache, ready for the next access.

During a cache miss, some other previously existing cache entry is removed in order to make room for the newly retrieved
data. The [heuristic](https://en.wikipedia.org/wiki/Heuristic_(computer_science) "Heuristic (computer science)") used to
select the entry to replace is known as
the [replacement policy](https://en.wikipedia.org/wiki/Cache_algorithm "Cache algorithm"). One popular replacement
policy, "least recently used" (LRU), replaces the oldest entry, the entry that was accessed less recently than any other
entry (see [cache algorithm](https://en.wikipedia.org/wiki/Cache_algorithm "Cache algorithm")). More efficient caching
algorithms compute the use-hit frequency against the size of the stored contents, as well as
the [latencies](https://en.wikipedia.org/wiki/Access_time "Access time") and throughputs for both the cache and the
backing store. This works well for larger amounts of data, longer latencies, and slower throughputs, such as that
experienced with hard drives and networks, but is not efficient for use within a CPU
cache.\[_[citation needed](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed "Wikipedia:Citation needed")_\]

### Writing policies

![](https://upload.wikimedia.org/wikipedia/commons/thumb/0/04/Write-through_with_no-write-allocation.svg/220px-Write-through_with_no-write-allocation.svg.png)

A write-through cache with no-write allocation

![](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/Write-back_with_write-allocation.svg/220px-Write-back_with_write-allocation.svg.png)

A write-back cache with write allocation

When a system writes data to cache, it must at some point write that data to the backing store as well. The timing of
this write is controlled by what is known as the _write policy_. There are two basic writing
approaches:[\[4\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-4)

- _Write-through_: write is done synchronously both to the cache and to the backing store.
- _Write-back_ (also called _write-behind_): initially, writing is done only to the cache. The write to the backing
  store is postponed until the modified content is about to be replaced by another cache block.

### write-back cache

A write-back cache is more complex to implement, since it needs to track which of its locations have been written over,
and mark them as _dirty_ for later writing to the backing store. The data in these locations are written back to the
backing store only when they are evicted from the cache, an effect referred to as a _lazy write_. For this reason, a
read miss in a write-back cache (which requires a block to be replaced by another) will often require two memory
accesses to service: one to write the replaced data from the cache back to the store, and then one to retrieve the
needed data.

Other policies may also trigger data write-back. The client may make many changes to data in the cache, and then
explicitly notify the cache to write back the data.

Since no data is returned to the requester on write operations, a decision needs to be made on write misses, whether or
not data would be loaded into the cache. This is defined by these two approaches:

- _Write allocate_ (also called _fetch on write_): data at the missed-write location is loaded to cache, followed by a
  write-hit operation. In this approach, write misses are similar to read misses.
- _No-write allocate_ (also called _write-no-allocate_ or _write around_): data at the missed-write location is not
  loaded to cache, and is written directly to the backing store. In this approach, data is loaded into the cache on read
  misses only.

Both write-through and write-back policies can use either of these write-miss policies, but usually they are paired in
this way:[\[5\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-HennessyPatterson2011-5)

- A write-back cache uses write allocate, hoping for subsequent writes (or even reads) to the same location, which is
  now cached.
- A write-through cache uses no-write allocate. Here, subsequent writes have no advantage, since they still need to be
  written directly to the backing store.

Entities other than the cache may change the data in the backing store, in which case the copy in the cache may become
out-of-date or _stale_. Alternatively, when the client updates the data in the cache, copies of those data in other
caches will become stale. Communication protocols between the cache managers which keep the data consistent are known
as [coherency protocols](https://en.wikipedia.org/wiki/Cache_coherency "Cache coherency").

### CPU cache

Small memories on or close to the [CPU](https://en.wikipedia.org/wiki/CPU "CPU") can operate faster than the much
larger [main memory](https://en.wikipedia.org/wiki/Main_memory "Main memory"). Most CPUs since the 1980s have used one
or more caches, sometimes [in cascaded levels](https://en.wikipedia.org/wiki/CPU_cache#Multi-level_caches "CPU cache");
modern high-end [embedded](https://en.wikipedia.org/wiki/Embedded_computing "Embedded computing")
, [desktop](https://en.wikipedia.org/wiki/Desktop_computer "Desktop computer") and
server [microprocessors](https://en.wikipedia.org/wiki/Microprocessor "Microprocessor") may have as many as six types of
cache (between levels and functions).[\[6\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-6) Examples of
caches with a specific function are the [D-cache](https://en.wikipedia.org/wiki/D-cache "D-cache")
and [I-cache](https://en.wikipedia.org/wiki/I-cache "I-cache") and
the [translation lookaside buffer](https://en.wikipedia.org/wiki/Translation_lookaside_buffer "Translation lookaside buffer")
for the [MMU](https://en.wikipedia.org/wiki/Memory_management_unit "Memory management unit").

### GPU cache

Earlier [graphics processing units](https://en.wikipedia.org/wiki/Graphics_processing_unit "Graphics processing unit") (
GPUs) often had limited read-only [texture caches](https://en.wikipedia.org/wiki/Texture_cache "Texture cache"), and
introduced [Morton order](https://en.wikipedia.org/wiki/Morton_order "Morton order") [swizzled textures](https://en.wikipedia.org/wiki/Swizzled_texture "Swizzled texture")
to improve 2D [cache coherency](https://en.wikipedia.org/wiki/Cache_coherency "Cache coherency")
. [Cache misses](https://en.wikipedia.org/wiki/Cache_miss "Cache miss") would drastically affect performance, e.g.
if [mipmapping](https://en.wikipedia.org/wiki/Mipmapping "Mipmapping") was not used. Caching was important to leverage
32-bit (and wider) transfers for texture data that was often as little as 4 bits per pixel, indexed in complex patterns
by arbitrary [UV coordinates](https://en.wikipedia.org/wiki/UV_coordinates "UV coordinates")
and [perspective transformations](https://en.wikipedia.org/wiki/Perspective_transformation "Perspective transformation")
in [inverse texture mapping](https://en.wikipedia.org/wiki/Inverse_texture_mapping "Inverse texture mapping").

As GPUs advanced (especially
with [GPGPU](https://en.wikipedia.org/wiki/GPGPU "GPGPU") [compute shaders](https://en.wikipedia.org/wiki/Compute_shader "Compute shader"))
they have developed progressively larger and increasingly general caches,
including [instruction caches](https://en.wikipedia.org/wiki/Instruction_cache "Instruction cache")
for [shaders](https://en.wikipedia.org/wiki/Shader "Shader"), exhibiting increasingly common functionality with CPU
caches.[\[7\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-ReferenceGCS7-7) For
example, [GT200](https://en.wikipedia.org/wiki/GeForce_200_series "GeForce 200 series") architecture GPUs did not
feature an L2 cache, while
the [Fermi](https://en.wikipedia.org/wiki/Fermi_(microarchitecture) "Fermi (microarchitecture)") GPU has 768 KB of
last-level cache, the [Kepler](https://en.wikipedia.org/wiki/Kepler_(microarchitecture) "Kepler (microarchitecture)")
GPU has 1536 KB of last-level cache,[\[7\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-ReferenceGCS7-7)
and the [Maxwell](https://en.wikipedia.org/wiki/Maxwell_(microarchitecture) "Maxwell (microarchitecture)") GPU has 2048
KB of last-level cache. These caches have grown to
handle [synchronisation primitives](https://en.wikipedia.org/wiki/Synchronisation_primitive "Synchronisation primitive")
between threads and [atomic operations](https://en.wikipedia.org/wiki/Atomic_operation "Atomic operation"), and
interface with a CPU-style [MMU](https://en.wikipedia.org/wiki/Memory_management_unit "Memory management unit").

### DSPs cache

[Digital signal processors](https://en.wikipedia.org/wiki/Digital_signal_processor "Digital signal processor") have
similarly generalised over the years. Earlier designs
used [scratchpad memory](https://en.wikipedia.org/wiki/Scratchpad_memory "Scratchpad memory") fed
by [DMA](https://en.wikipedia.org/wiki/Direct_memory_access "Direct memory access"), but modern DSPs such
as [Qualcomm Hexagon](https://en.wikipedia.org/wiki/Qualcomm_Hexagon "Qualcomm Hexagon") often include a very similar
set of caches to a CPU (
e.g. [Modified Harvard architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture "Modified Harvard architecture")
with shared L2, split L1 I-cache and D-cache).[\[8\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-8)

### Translation lookaside buffer

A [memory management unit](https://en.wikipedia.org/wiki/Memory_management_unit "Memory management unit") (MMU) that
fetches page table entries from main memory has a specialized cache, used for recording the results
of [virtual address](https://en.wikipedia.org/wiki/Virtual_address "Virtual address")
to [physical address](https://en.wikipedia.org/wiki/Physical_address "Physical address") translations. This specialized
cache is called
a [translation lookaside buffer](https://en.wikipedia.org/wiki/Translation_lookaside_buffer "Translation lookaside buffer") (
TLB).[\[9\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-9)

### Information-centric networking

[Information-centric networking](https://en.wikipedia.org/wiki/Information-centric_networking "Information-centric networking") (
ICN) is an approach to evolve the [Internet](https://en.wikipedia.org/wiki/Internet "Internet") infrastructure away from
a host-centric paradigm, based on perpetual connectivity and
the [end-to-end principle](https://en.wikipedia.org/wiki/End-to-end_principle "End-to-end principle"), to a network
architecture in which the focal point is identified information (or content or data). Due to the inherent caching
capability of the nodes in an ICN, it can be viewed as a loosely connected network of caches, which has unique
requirements of caching policies. However, ubiquitous content caching introduces the challenge to content protection
against unauthorized access, which requires extra care and
solutions.[\[10\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-10) Unlike proxy servers, in ICN the cache
is a network-level solution. Therefore, it has rapidly changing cache states and higher request arrival rates; moreover,
smaller cache sizes further impose a different kind of requirements on the content eviction policies. In particular,
eviction policies for ICN should be fast and lightweight. Various cache replication and eviction schemes for different
ICN architectures and applications have been proposed.

### Policies Time aware least recently used (TLRU)

The Time aware Least Recently Used (TLRU)[\[11\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-11) is a
variant of LRU designed for the situation where the stored contents in cache have a valid life time. The algorithm is
suitable in network cache applications, such as Information-centric networking (ICN)
, [Content Delivery Networks](https://en.wikipedia.org/wiki/Content_Delivery_Network "Content Delivery Network") (CDNs)
and distributed networks in general. TLRU introduces a new term: TTU (Time to Use). TTU is a time stamp of a
content/page which stipulates the usability time for the content based on the locality of the content and the content
publisher announcement. Owing to this locality based time stamp, TTU provides more control to the local administrator to
regulate in network storage. In the TLRU algorithm, when a piece of content arrives, a cache node calculates the local
TTU value based on the TTU value assigned by the content publisher. The local TTU value is calculated by using a locally
defined function. Once the local TTU value is calculated the replacement of content is performed on a subset of the
total content stored in cache node. The TLRU ensures that less popular and small life content should be replaced with
the incoming content.

##### Policies:  Least frequent recently used (LFRU)

The Least Frequent Recently Used (LFRU)[\[12\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-12) cache
replacement scheme combines the benefits of LFU and LRU schemes. LFRU is suitable for 'in network' cache applications,
such as Information-centric networking (ICN), Content Delivery Networks (CDNs) and distributed networks in general. In
LFRU, the cache is divided into two partitions called privileged and unprivileged partitions. The privileged partition
can be defined as a protected partition. If content is highly popular, it is pushed into the privileged partition.
Replacement of the privileged partition is done as follows: LFRU evicts content from the unprivileged partition, pushes
content from privileged partition to unprivileged partition, and finally inserts new content into the privileged
partition. In the above procedure the LRU is used for the privileged partition and an approximated LFU (ALFU) scheme is
used for the unprivileged partition, hence the abbreviation LFRU. The basic idea is to filter out the locally popular
contents with ALFU scheme and push the popular contents to one of the privileged partition.

### Software caches: Disk cache

While CPU caches are generally managed entirely by hardware, a variety of software manages other caches.
The [page cache](https://en.wikipedia.org/wiki/Page_cache "Page cache") in main memory, which is an example of disk
cache, is managed by the operating
system [kernel](https://en.wikipedia.org/wiki/Kernel_(computing) "Kernel (computing)").

While the [disk buffer](https://en.wikipedia.org/wiki/Disk_buffer "Disk buffer"), which is an integrated part of the
hard disk drive, is sometimes misleadingly referred to as "disk cache", its main functions are write sequencing and read
prefetching. Repeated cache hits are relatively rare, due to the small size of the buffer in comparison to the drive's
capacity. However, high-end [disk controllers](https://en.wikipedia.org/wiki/Disk_controller "Disk controller") often
have their own on-board cache of the hard disk
drive's [data blocks](https://en.wikipedia.org/wiki/Block_(data_storage) "Block (data storage)").

Finally, a fast local hard disk drive can also cache information held on even slower data storage devices, such as
remote servers ([web cache](https://en.wikipedia.org/wiki/Web_cache "Web cache")) or
local [tape drives](https://en.wikipedia.org/wiki/Tape_drive "Tape drive")
or [optical jukeboxes](https://en.wikipedia.org/wiki/Optical_jukebox "Optical jukebox"); such a scheme is the main
concept
of [hierarchical storage management](https://en.wikipedia.org/wiki/Hierarchical_storage_management "Hierarchical storage management")
. Also, fast flash-based [solid-state drives](https://en.wikipedia.org/wiki/Solid-state_drive "Solid-state drive") (
SSDs) can be used as caches for slower rotational-media hard disk drives, working together
as [hybrid drives](https://en.wikipedia.org/wiki/Hybrid_drive "Hybrid drive")
or [solid-state hybrid drives](https://en.wikipedia.org/wiki/Solid-state_hybrid_drive "Solid-state hybrid drive") (
SSHDs).

### Software caches: Web cache

[Web browsers](https://en.wikipedia.org/wiki/Web_browser "Web browser")
and [web proxy servers](https://en.wikipedia.org/wiki/Proxy_server "Proxy server") employ web caches to store previous
responses from [web servers](https://en.wikipedia.org/wiki/Web_server "Web server"), such
as [web pages](https://en.wikipedia.org/wiki/Web_page "Web page")
and [images](https://en.wikipedia.org/wiki/Image "Image"). Web caches reduce the amount of information that needs to be
transmitted across the network, as information previously stored in the cache can often be re-used. This reduces
bandwidth and processing requirements of the web server, and helps to
improve [responsiveness](https://en.wikipedia.org/wiki/Responsiveness "Responsiveness") for users of the
web.[\[13\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-13)

Web browsers employ a built-in web cache, but
some [Internet service providers](https://en.wikipedia.org/wiki/Internet_service_provider "Internet service provider") (
ISPs) or organizations also use a caching proxy server, which is a web cache that is shared among all users of that
network.

Another form of cache is [P2P caching](https://en.wikipedia.org/wiki/P2P_caching "P2P caching"), where the files most
sought for by [peer-to-peer](https://en.wikipedia.org/wiki/Peer-to-peer "Peer-to-peer") applications are stored in
an [ISP](https://en.wikipedia.org/wiki/Internet_service_provider "Internet service provider") cache to accelerate P2P
transfers. Similarly, decentralised equivalents exist, which allow communities to perform the same task for P2P traffic,
for example, Corelli.[\[14\]](https://en.wikipedia.org/wiki/Cache_(computing)#cite_note-14)

### Software caches: Memoization

A cache can store data that is computed on demand rather than retrieved from a backing
store. [Memoization](https://en.wikipedia.org/wiki/Memoization "Memoization") is
an [optimization](https://en.wikipedia.org/wiki/Program_optimization "Program optimization") technique that stores the
results of resource-consuming [function calls](https://en.wikipedia.org/wiki/Function_call "Function call") within a
lookup table, allowing subsequent calls to reuse the stored results and avoid repeated computation. It is related to
the [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming "Dynamic programming") algorithm design
methodology, which can also be thought of as a means of caching.

### Software caches: Other caches

The BIND [DNS](https://en.wikipedia.org/wiki/Domain_Name_System "Domain Name System") daemon caches a mapping of domain
names to [IP addresses](https://en.wikipedia.org/wiki/IP_address "IP address"), as does a resolver library.

Write-through operation is common when operating over unreliable networks (like an Ethernet LAN), because of the
enormous complexity of the [coherency protocol](https://en.wikipedia.org/wiki/Cache_coherency "Cache coherency")
required between multiple write-back caches when communication is unreliable. For instance, web page caches
and [client-side](https://en.wikipedia.org/wiki/Client-side "Client-side") [network file system](https://en.wikipedia.org/wiki/Network_File_System "Network File System")
caches (like those
in [NFS](https://en.wikipedia.org/wiki/Network_File_System_(protocol) "Network File System (protocol)")
or [SMB](https://en.wikipedia.org/wiki/Server_Message_Block "Server Message Block")) are typically read-only or
write-through specifically to keep the network protocol simple and reliable.

[Search engines](https://en.wikipedia.org/wiki/Web_search_engine "Web search engine") also frequently
make [web pages](https://en.wikipedia.org/wiki/Web_page "Web page") they have indexed available from their cache. For
example, [Google](https://en.wikipedia.org/wiki/Google "Google") provides a "Cached" link next to each search result.
This can prove useful when web pages from a [web server](https://en.wikipedia.org/wiki/Web_server "Web server") are
temporarily or permanently inaccessible.

Another type of caching is storing computed results that will likely be needed again,
or [memoization](https://en.wikipedia.org/wiki/Memoization "Memoization"). For
example, [ccache](https://en.wikipedia.org/wiki/Ccache "Ccache") is a program that caches the output of the compilation,
in order to speed up later compilation runs.

Database caching can substantially improve the throughput of Database applications, for example in the processing of
Database index Data dictionary, and frequently used subsets of data.

Distributed cache uses networked hosts to provide scalability, reliability and performance to the application.The hosts
can be co-located or spread over different geographical regions.

### Buffer vs. cache

The semantics of a "buffer" and a "cache" are not totally different; even so, there are fundamental differences in
intent between the process of caching and the process of buffering.

Fundamentally, caching realizes a performance increase for transfers of data that is being repeatedly transferred. While
a caching system may realize a performance increase upon the initial (typically write) transfer of a data item, this
performance increase is due to buffering occurring within the caching system.

With read caches, a data item must have been fetched from its residing location at least once in order for subsequent
reads of the data item to realize a performance increase by virtue of being able to be fetched from the cache's (faster)
intermediate storage rather than the data's residing location. With write caches, a performance increase of writing a
data item may be realized upon the first write of the data item by virtue of the data item immediately being stored in
the cache's intermediate storage, deferring the transfer of the data item to its residing storage at a later stage or
else occurring as a background process. Contrary to strict buffering, a caching process must adhere to a (potentially
distributed) cache coherency protocol in order to maintain consistency between the cache's intermediate storage and the
location where the data resides.

Buffering, on the other hand,

- reduces the number of transfers for otherwise novel data amongst communicating processes, which amortizes overhead
  involved for several small transfers over fewer, larger transfers,
- provides an intermediary for communicating processes which are incapable of direct transfers amongst each other, or
- ensures a minimum data size or representation required by at least one of the communicating processes involved in a
  transfer.

With typical caching implementations, a data item that is read or written for the first time is effectively being
buffered; and in the case of a write, mostly realizing a performance increase for the application from where the write
originated. Additionally, the portion of a caching protocol where individual writes are deferred to a batch of writes is
a form of buffering. The portion of a caching protocol where individual reads are deferred to a batch of reads is also a
form of buffering, although this form may negatively impact the performance of at least the initial reads (even though
it may positively impact the performance of the sum of the individual reads). In practice, caching almost always
involves some form of buffering, while strict buffering does not involve caching.

### Buffer: benefit

A [buffer](https://en.wikipedia.org/wiki/Data_buffer "Data buffer") is a temporary memory location that is traditionally
used because CPU [instructions](https://en.wikipedia.org/wiki/Instruction_(computing) "Instruction (computing)") cannot
directly address data stored in peripheral devices. Thus, addressable memory is used as an intermediate stage.
Additionally, such a buffer may be feasible when a large block of data is assembled or disassembled (as required by a
storage device), or when data may be delivered in a different order than that in which it is produced. Also, a whole
buffer of data is usually transferred sequentially (for example to hard disk), so buffering itself sometimes increases
transfer performance or reduces the variation or jitter of the transfer's latency as opposed to caching where the intent
is to reduce the latency. These benefits are present even if the buffered data are written to
the [buffer](https://en.wikipedia.org/wiki/Data_buffer "Data buffer") once and read from the buffer once.

### Cache: benefit
A cache also increases transfer performance. A part of the increase similarly comes from the possibility that multiple
small transfers will combine into one large block. But the main performance-gain occurs because there is a good chance
that the same data will be read from cache multiple times, or that written data will soon be read. A cache's sole
purpose is to reduce accesses to the underlying slower storage. Cache is also usually
an [abstraction layer](https://en.wikipedia.org/wiki/Abstraction_layer "Abstraction layer") that is designed to be
invisible from the perspective of neighboring layers.