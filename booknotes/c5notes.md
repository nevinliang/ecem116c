# Ch. 5 Notes #  

## 5.1 Introduction ##  
temporal v spatial locality  
sram < dram < magdisk/ssd  

block/line: 	min unit of info in/not in cache  
hit rate: 		(for a level) accesses in level/total accesses  
miss rate: 		1 - hit rate  
hit time: 		time to determine hit/miss + access memory if hit  
miss penalty: 	time needed to access, move, and record block if miss  

## 5.2 Memory Technologies ##  
discusses types of memory  
calculate rotational latency/delay formula for disks  

## 5.3 The Basics of Caches ##  

*direct mapped cache*: each mem location mapped to 1 cache location  
					 (block address) mod (# blocks in cache)  
tag:
	- contains address info to identify whether word corresponds to requested val  
	- tag needs to contain UPPER PORTION of address (bits not used in index)  
		- ex: if address is 5 bits, and index is 3 bits aka 8 indices,  
			tag is 2 bits, the first two.  
valid bit: 0, ignore tag.  
cache index selects the block, and tag field compares w the address given  
(block size === data size). each block has (valid bit + tag + data) because  
index is stored as just integer aka where it is.  

ex: bits in a cache  
	- APPARENTLY A WORD IS 4 BYTES IN RISC-V/32 BITS  
	- 16 * 2^10 = 2^14 bytes = 2^12 words. 1 block = 4 words so 2^10 blocks  
	- each block is 64 bits of address, 128 bits of data, and  

## 5.4 Measuring and Improving Cache Performance ##  
CPU time = (CPU exe clock cycles + memory-stall clock cycles) * clock cycle time  
memory stall clock cycles = (read-stall cycles + write-stall cycles)  
	- read-stall cycles = reads/pgrm x read miss rate x read miss penalty  
	- write-stall cycles = writes/pgrm x write miss rate x write miss penalty  
							+ write buffer stalls  
		- in system w reasonable write buffer death, write buffer stalls = 0  
	- memory-stall cycles = memory accesses/prgm x miss rate x miss penalty  
							= instructs/prgm x misses/instructs x miss penalty  
AMAT (avg memory access time):  
	time for a hit + miss rate x miss penalty  
		- time for a hit === cache access time for a hit in clock cycles  

More flexible placement of blocks:  
	- *fully associative* (opposite of direct mapped)  
		- block can be placed anywhere in the cache  
	- *set associative* (btwn fully and direct mapped)  
		- n locations where each block of memory can be placed in each set  
			- which also means just the number of places it can be put  
		- n-way set associative  
		- # blocks in the total cache = # sets * # blocks/set  
			- in this case, the set is what was originally the index  
		- each block has (tag, data)  
			- 2 way set associative has 2 blocks/set and therefore 2 tags 2 datas  
				per set.  
ex: 0, 8, 0, 6, 0  
	- 5 direct misses, 3 associative misses, 4 2-way ossociative misses  
LRU: least recently used scheme for caches  
	- as associativity goes up, LRU gets harder to implement  
*to calculate number of m bits in a block, 2^m = # of bytes per block*  
	- *apparently m bits is the number of offset bits*...not sure if true, so  
		check w other people  
	- total number of tag bits is now 64 - m - n - 2 because 2 byte offset  
ex: cycles/insruct = 1.0  
	clock rate = 4e9 cycles/sec  
	4e9 instructs/sec  
	1 instruct = 0.25 ns = 1 cycle  
	400 clock cycles for miss ->  
		*CPI = (regular CPU clock cycles + memory-stall clock cycles)*  
			= 1 + 400 * 0.02 = 9 cycles avg  
	assume with secondary cache now -> 1 + 20 * 0.02 + 400 * 0.005 = 3.4 < 9 cycles avg  
	faster by 9/3.4 = 2.6x  

## HW 5 Review ##  
- Apparently the index has to be a power of 2...because # of index bits = n = log2(index)  
- Offset Bits = log_2(line/block size)  
- Index Bits = log_2(# of sets)  
- to calculate how good the cache is compare the total number of cycles for a  
	given set of addresses  
- calculate the total size of cache by   
	- calculating all the # of bits (tag, offset, index)  
	- multiplying the ones in the cache by the total number of blocks  
		- aka tag and valid (index and offset is not in the cache)  
	- then adding the actual data (prolly given in the problem)  

## 5.7 Virtual Memory ##  
-  
