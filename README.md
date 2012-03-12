DKit
====

Introduction
------------

In all the work we've done over the years, nothing has been quite as liberating
as thread-safe containers. They are an amazing experience for a developer
because they allow the multi-threaded programmer to not have to worry about
locks, lock contention, deadlocks, dirty reads, and a whole host of other
issues that for years have made writing multi-threaded code difficult. These
containers and objects free the user from having to worry about locking
because they use the _compare-and-swap_ (CAS) operations in GCC to ensure
that the data accessed is done properly, and securely.

Atomic Integers
---------------

The first element of DKit is the atomic integers. The sized integers in
`stdint.h` are very useful for mixed-mode code, and yet there's nothing that
allow simple atomic access to these values. So the library has the following
classes defined in `atomic.h` :

*	`abool` a simple atomic boolean
*	`auint8_t` a simple atomic unsigned 8-bit integer
*	`aint8_t` a simple atomic signed 8-bit integer
*	`auint16_t` a simple atomic unsigned 16-bit integer
*	`aint16_t` a simple atomic signed 16-bit integer
*	`auint32_t` a simple atomic unsigned 32-bit integer
*	`aint32_t` a simple atomic signed 32-bit integer
*	`auint64_t` a simple atomic unsigned 64-bit integer
*	`aint64_t` a simple atomic signed 64-bit integer

These all are simply an `a` on the front of the types defined in `stdint.h`,
and all have atomic access and updating. They all also have casting operators
that make it very easy to get the values out, when it's necessary.

Single-Producer, Single-Consumer Containers
-------------------------------------------

Separated into a different namespace, the single-producer, single-consumer,
containers have taken advantage of the fact that they are only _filled_ by
one *and only one* thread, and that they are _emptied_ by one *and only one*
thread. This means that their safe use in the code is the responsibility of
the developer as they _must_ be sure of the threads that are using these
containers. However, when those conditions are met, these queues and other
containers are exceptionally efficient, and perform their task very fast.

# dkit::spsc::CircularFIFO

The simplest SPSC (single-producer, single-consumer) queue is a simple circular
FIFO (first-in, first-out) queue. The implementation of this is really not all
that different from any simple circular FIFO queue, with a few important
exceptions. These exceptions, and the careful use by the user of this class
make this a very nice candidate for simple pools.

One such excample, might be a `std::string` pool where a single thread is
responsible for getting data from some source, placing it into `std::string`
instances, and then when they are processed by another thread, these
`std::string` instances are _recycled_ back to the pool. There's one "filling"
thread that _reads_ from the queue, and another that "recycles" the spent
instances back to the pool.

Utility/Helper Classes
----------------------

In order to make the code in the classes a little easier to use and work with,
we've also created a few utility classes put in a separate namespace: `util`,
and located in the util directory.

# dkit::util::timer

This is a simple timer class that allows fast and accurate timing on the
different platforms (linux, Mac OS X) that this codebase is targeted for.
The reason for having this is that many of the test apps need to have accurate
and lightweight timings to test for efficiencies as well as just to gather
stats on the code.

License
-------

	Copyright 2012 by Robert E. Beaty

	Permission is hereby granted, free of charge, to any person obtaining
	a copy of this software and associated documentation files (the "Software"),
	to deal in the Software without restriction, including without limitation
	the rights to use, copy, modify, merge, publish, distribute, sublicense,
	and/or sell copies of the Software, and to permit persons to whom the
	Software is furnished to do so, subject to the following conditions:

	The above copyright notice and this permission notice shall be included
	in all copies or substantial portions of the Software.

	THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
	OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
	FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
	THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
	LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
	FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
	DEALINGS IN THE SOFTWARE.