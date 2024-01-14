# Snooping-Based Cache Coherence

Review:
- What is the difference between a write back and a write-through cache?(write hit)
- What about a allocate vs. write-no-allocate cache?(write miss)

Behavior of write-allocate, write-back cache on a write miss(Example: processor executes __volatile int x = 1__)
1. Processor performs write to address that is not resident in cache
2. Cache selects location to place line in cache, if there is a dirty line currently in 
this location, the dirty line is written out to memory
3. Cache loads line from memory (“allocates line in cache”)
4. 32 bits of cache line are updated
5. Cache line is marked as dirty

## Coherence Problem
Memory Coherence is a conflict between implementation and abstracion in general.
- Intuitive behavior for memory system: reading value at address X should return the last value written to address X by any processor.
- Memory coherence problem exists because there is both global 
storage (main memory) and per-processor local storage (processor caches) __implementing__ the __abstraction__ of a single 
shared address space.

## Snooping-based coherence implementations

__sequential consistency__:
1. Memory operations issued by any one processor occur in the order issued by the processor
2. The value returned by a read is the value written by the 
last write to the location as given by the serial order

In an other word, A memory system is coherent if:
1. A read by processor P to address X that follows a write by P to address X, 
should return the value of the write by P
2. A read by processor P1 to address X that follows a write by processor P2 to 
X returns the written value... if the read and write are “sufficiently 
separated” in time (assuming no other write to X occurs in between)
3. Writes to the same address are serialized: two writes to address X by any 
two processors are observed in the same order by all processors.
(Example: if values 1 and then 2 are written to address X, no processor observes X having value 2 before value 1)

- Condition 1: obeys program order (as expected of a uniprocessor system)
- Condition 2: “write propagation”
- Condition 3: “write serialization”



Snooping cache-coherence schemes'main idea: all coherence-related activity is broadcast to all processors

### Very simple implementation
Let’s assume: __Write-through caches__

Upon write, cache controller broadcasts invalidation message.As a result, the next read from other processors will trigger cache miss

But write-through policy implementation is inefficient
- Every write operation goes out to memory
- Very high bandwidth requirements

### Little complicated implementation
MSI write-back invalidation protocol:

Key ideas:
- A line in the “exclusive” state can be modified without notifying 
the other caches and processor can only write to lines in the exclusive state

Three cache line states
- Invalid (I): same as meaning of invalid in uniprocessor cache
- Shared (S): line valid in one or more caches
- Modified (M): line valid in exactly one cache (a.k.a. “dirty” or “exclusive” state)

Two processor operations (triggered by local CPU)
- PrRd (read)
- PrWr (write)

Three coherence-related bus transactions (from remote caches) 
- BusRd: obtain copy of line with no intent to modify
- BusRdX: obtain copy of line with intent to modify
- flush: write dirty line out to memory

Let us think about a lower-level choices: who should supply data on a cache miss when line is in the E or S state of another cache? Obviously, cache-to-cache transfers add complexity, but commonly used to reduce both latency of data access and reduce memory 
bandwidth required by application
(using 5 stages protocals like MESIF and MOESI)

### Invalidation-based vs. Update-based Protocols

Intuitively, update would seem preferable if other processors 
sharing data continue to access it after a write occurs, But updates are overhead if:
- Data just sits in caches (and is never read by another processor again) 
- Application performs many writes before the next read

Update can suffer from high traffic due to multiple writes before the next read by another processor. Current AMD and Intel implementations of cache coherence are invalidation based

And remember NVIDIA GPUs do not implement cache coherence!


