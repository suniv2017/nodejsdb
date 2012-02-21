# Intrinsic datastores for Node.js (nodejsdb)

Experimental project. Runnable artefacts will be published as standalone npm modules, and by various authors.

Add your comments in the form of [Issues](https://github.com/ypocat/nodejsdb/issues), or contribute to [this discussion](http://groups.google.com/group/nodejs/browse_frm/thread/1ec2908cd5fafa28).

### Rationale

Few years ago, server-side JavaScript was unimaginable. Today, at the beginning of 2012, more and more businesses increasingly rely on high-performance, low-development-costs, short time-to-market, and [explosively growing ecosystem](http://search.npmjs.org/) of [libraries](https://github.com/joyent/node/wiki/modules) of the _[Node.js](http://nodejs.org/) platform_. However Node.js is not an exception, but rather a confirmation of the rule that JavaScript is the most potent environment for software evolution available today. Other notable JavaScript ecosystems with explosive growth are [Firefox Extensions](https://addons.mozilla.org/en-US/firefox/extensions/), [OS X Dashboard Widgets](http://www.apple.com/downloads/dashboard/), [Chrome Extensions](https://chrome.google.com/webstore/category/extensions), and of course the _client side_ of the web, with millions of libraries, frameworks and applications.

However, a very important area where this kind of explosive evolution is desperately needed but where it is not happening, is the area of _database development_. We only have a handful of projects to choose from, and even fewer architectural models. Problems like clustering, interfacing, query languages, persistence strategies, etc. are currently mostly in the domain of lower-level languages. _Instrinsic datastores for Node.js_ is an attempt to support this portion of the Node.js ecosystem.

What we inevitably see during the course of evolution of almost any database product, is its extension with some form of a secondary language (the query language being the primary one). This comes either in the form of stored procedures (e.g. T-SQL, PL/SQL, etc.), or a scripting language (e.g. Lua in Redis).

So the idea here is to bring datastore functionality and scripting into the same process, the same way as we see it with dedicated databases in form of stored procedures, but this time from the other way around - bring the database to the scripting environment:

#### Advantages, when building standalone database servers:

- Utilization of the Node.js platform and its ecosystem to evolve database products.

#### Advantages, when using this approach to join the application and the database layer:

- The OS will not have to process the extra TCP/IP or IPC that occurs with out-of-process databases.
- Data access latency will be lower.
- In simple implementations, data access may be synchronous.
- Simplified software stack.


### Known Efforts

#### Native

- [Rawhash](https://github.com/pconstr/rawhash) - In-memory key:value cache where keys are binary Buffers <sup>_mem, kv_</sup>
- [Node-LevelDB](https://github.com/my8bird/node-leveldb) - NodeJS bindings to [levelDB](http://code.google.com/p/leveldb/), with [SSTable](http://www.igvita.com/2012/02/06/sstable-and-log-structured-storage-leveldb/) disk storage approach <sup>_disk, kv_</sup>
- [node-cask](https://github.com/randomekek/node-cask) - [Bitcask](http://wiki.basho.com/Bitcask.html) clone for node, based on [node-mmap](https://github.com/bnoordhuis/node-mmap) <sup>_disk, kv_</sup>
- [node-gdbm](https://github.com/tokuhirom/node-gdbm) - interface to GNU GDBM <sup>_disk, kv_</sup>
- [node-sqlite3](https://github.com/developmentseed/node-sqlite3) - "Asynchronous, non-blocking SQLite3 bindings for Node.js" <sup>_disk, sql_</sup>
- [node-memcache](https://github.com/vanillahsu/node-memcache) - in-process memcached for Node.js <sup>_mem, kv_</sup>

#### In-VM

- [hive](https://github.com/Pollenware/hive) - "In memory store for Node JS"
- [nStore](https://github.com/creationix/nstore) - "uses a safe append-only data format for quick inserts, updates, and deletes. Also a index of all documents and their exact location on the disk is stored in in memory for fast reads of any document"
- [node-tiny](https://github.com/chjj/node-tiny) - "largely inspired by nStore, however, its goal was to implement real querying which goes easy on the memory"
- [BarricaneDB](https://github.com/chrisdew/barricane-db) - "a transparent object persistence mechanism"
- [chaos](https://github.com/stagas/chaos) - "we exploit the sha1 chaotic randomness to store the keys evenly in the filesystem"
- [Alfred](https://github.com/pgte/alfred) - "a fast in-process key-value store for node.js"
- [awesome](https://github.com/janl/awesome) - "A Redis implementation in node.js"
- [nedis](https://github.com/visionmedia/nedis) - "Redis server implementation written with nodejs"
- [EventVat](https://github.com/hij1nx/eventvat) - "evented in-process key/value store with an API like that of Redis"


### Scratchpad

* ~~Is v8 good for in-memory data storage? Data would be first class citizen and a lot of wheel-reinventing could be avoided. v8 translates JS directly into machine code, how to best leverage this?~~ -- A [simple test](https://gist.github.com/1869292) on [Node v0.7.4 Mac](http://nodejs.org/dist/v0.7.4/node-v0.7.4.pkg) revealed that on my 2,8GHz dual core machine, about 40M objects is where v8 starts to choke. Given the high level of optimization already done in v8, it's probably safe to assume that any significant improvement of v8's GC on the current architecture is not possible, at least not without adding significant memory usage overhead, or without significant rewrite of the current implementation. New possibilities are on the horizon in the form of GPU-supported GC, albeit patent-encumbered (see [here](http://www.google.com/patents/US20100082930) and [here](http://www.google.com/patents/US20110219204)), but we've already seen a lot worse situations where patent-free solutions were developed working-around the existing patents, e.g. WebM vs. H.264, and many others. However at the present time (2/2012), it is best not to consider node/v8 as a good storage for large number of objects.

### Plans

- decide on a good primitive data structures and operation set which would allow to model most used DB cases, including pub-sub

	- binary safe keys and data

	- map, ordered-map, deque
	
	- key timeouts
	
	- event emitter
	
	- atomic ops? transactions? (plan ahead for the concurrent impl?)

- provide drop-in replaceable implementations with varying tradeoffs:

	- all data in memory (with or without secondary storage) - fastest, RAM-limited
	
	- all keys in memory - still pretty fast, not-so RAM-limited
	
	- keys and data in memory on-demand

- provide further variations:

	- single-process - fast, but multiple cores and multiple Nodes cannot work with the same data, clustering must be applied
	
	- shared-memory implementation - certain overhead and latency but higher total performance up from a certain number of cores (atomic ops and async API necessary at this point)

### Notes

* [Mac OS X Server v10.6: Adjusting Shared memory segment values](http://support.apple.com/kb/HT4022)

* [864GB RAM for $12,000](http://37signals.com/svn/posts/3090-basecamp-nexts-caching-hardware)

* [v8 now supports big memory. Is v8 GC still slow on big data?](http://code.google.com/p/v8/issues/detail?id=847)

* [v8 design elements](http://code.google.com/apis/v8/design.html)

* [Left Leaning Red-Black Trees](http://www.cs.princeton.edu/~rs/talks/LLRB/08Penn.pdf)

* [Pattern matching on GPU](http://dcs.ics.forth.gr/Activities/papers/gpupattern.iiswc11.pdf)

* [Linux Kernel is getting GPU support](http://www.cs.utah.edu/~wbsun/kgpu.pdf)

* [Accelerating large graph algorithms on the GPU using CUDA](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.102.4206&rep=rep1&type=pdf)

* [Accelerating SQL Database Operations on a GPU with CUDA](www.cs.virginia.edu/~skadron/Papers/bakkum_sqlite_gpgpu10.pdf)

* [GPU assisted garbage collection](http://www.google.com/patents/US20100082930)

* [GPU support for garbage collection](http://www.google.com/patents/US20110219204)

* [GPU databases](http://gpgpu.org/tag/databases)

* [Revisiting Network I/O APIs](http://antirobotrobot.tumblr.com/post/16311595301/revisiting-network-i-o-apis-the-netmap-framework)

* [Netmap for Linux](http://info.iet.unipi.it/~luigi/netmap/)

* [10GbE PCI-E x8 2-port NIC for $150](http://www.compuvest.com/Desc.jsp?iid=1583174)
