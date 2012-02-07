# Intrinsic datastores for Node.js (nodejsdb)

Experimental project. Runnable artefacts will be published as standalone npm modules.

### Rationale

Few years ago, server-side JavaScript was unimaginable. Today, at the beginning of 2012, more and more businesses increasingly rely on high-performance, low-development-costs, short time-to-market, and [explosively growing ecosystem](http://search.npmjs.org/) of [libraries](https://github.com/joyent/node/wiki/modules) of the _[Node.js](http://nodejs.org/) platform_. However Node.js is not an exception, but rather a confirmation of the rule that JavaScript is the most potent environment for software evolution available today. Other notable JavaScript ecosystems with explosive growth are [Firefox Extensions](https://addons.mozilla.org/en-US/firefox/extensions/), [OS X Dashboard Widgets](http://www.apple.com/downloads/dashboard/), [Chrome Extensions](https://chrome.google.com/webstore/category/extensions), and of course the _client side_ of the web, with thousands of libraries, frameworks and applications.

However, a very important area where this kind of explosive evolution is desperately needed but where it is not happening, is the area of _database development_. We only have a handful of projects to choose from, and even fewer architectural models. _Instrinsic datastores for Node.js_ is an attempt to kick-start this portion of the Node.js ecosystem.

### Scratchpad

* Is v8 good for in-memory data storage? Data would be first class citizen and a lot of wheel-reinventing could be avoided. v8 translates JS directly into machine code, how to best leverage this?

* Would shared memory managed by a native Node addon be a better data storage? Custom memory allocation could be employed. A process could be spawned to hold the shared memory segment continuously allocated across Node restarts. Multiple Nodes could share the segment. Would shared Buffers be a good mechanism to exchange data between v8 and the shared memory?

* What is the smallest set of primitive datastore operations, on top of which a good subset of e.g. SQL could be implemented?

* What visualization tools do we have available (or can develop) to enable our brain to understand existing data storage algorithms in the depth necessary for their optimized implementation in contemporary environments (OS, CPU, GPU, ASIC)?

### Plans

* Implement basic simple LLRB in vanilla JS on v8 and see how fast and memory-efficient it is, and what DB features can be built on top of it. A simple list structure may be needed too. However core API must be kept as small as possible, to allow for various implementations and optimizations.

* See what next. Perhaps try to speed up the above by implementing it in a big Buffer with custom allocation, perhaps later to be re-implemented in C or ASM.

### Notes

* [Mac OS X Server v10.6: Adjusting Shared memory segment values](http://support.apple.com/kb/HT4022)

* [864GB RAM for $12,000](http://37signals.com/svn/posts/3090-basecamp-nexts-caching-hardware)

* [v8 now supports big memory. Is v8 GC still slow on big data?](http://code.google.com/p/v8/issues/detail?id=847)

* [v8 design elements](http://code.google.com/apis/v8/design.html)

* [Left Leaning Red-Black Trees](http://www.cs.princeton.edu/~rs/talks/LLRB/08Penn.pdf)

* [Pattern matching on GPU](http://dcs.ics.forth.gr/Activities/papers/gpupattern.iiswc11.pdf)

* [Accelerating SQL Database Operations on a GPU with CUDA](www.cs.virginia.edu/~skadron/Papers/bakkum_sqlite_gpgpu10.pdf)

* [GPU databases](http://gpgpu.org/tag/databases)

* [Revisiting Network I/O APIs](http://antirobotrobot.tumblr.com/post/16311595301/revisiting-network-i-o-apis-the-netmap-framework)

* [10GbE PCI-E x8 2-port NIC for $150](http://www.compuvest.com/Desc.jsp?iid=1583174)
