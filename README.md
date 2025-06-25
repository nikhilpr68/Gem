
# README: I-Cache Profiling and Analysis in GEM5

**Course:** Computer Organisation and Architecture (COA)
**Assignment:** GEM5 Simulator - Instruction Cache Profiling

## Overview

This project extends the [GEM5](https://www.gem5.org/) simulator to provide detailed profiling of instruction cache (I-cache) behavior, specifically focusing on **hits and misses** as a function of cache size and associativity. The enhancements allow for fine-grained analysis of cache performance, including:

- Counting total hits and misses
- Profiling hits and misses within specific simulation intervals (between ticks T1 and T2)
- Recording hits per cache set and per way within sets

These modifications support in-depth architectural exploration and performance tuning for modern processor designs.

`## Scored full marks on the project 10/10`


## Motivation

Understanding cache behavior is crucial for optimizing processor performance. By instrumenting GEM5 to track not just total cache hits/misses, but also their distribution across sets and ways, we gain actionable insights into:

- The effectiveness of different cache configurations (size, associativity)
- Temporal locality and access patterns of workloads
- Potential bottlenecks and imbalances in cache utilization


## Key Modifications

### 1. `CacheMemory.cc`

- **`profileDemandHit()`**: Counts hits within a specified tick interval (T1–T2).
- **`profileDemandIHits(Addr address)`**: Tracks hits per set and per way for each I-cache access, enabling spatial profiling.
- **`profileDemandMiss()`**: Counts misses within the same tick interval.
- **`init()`**: Initializes statistics vectors for set/way hit counting.


### 2. `CacheMemory.hh`

- Declared vectors for:
    - `l1_hits_per_set`: Hit count per set
    - `hitsperway_set_way`: 2D vector for hit count per way in each set
- Declared the `profileDemandIHits` function.


### 3. `RubySlicc_Types.sm`

- Declared the `profileDemandIHits` function for use in SLICC state machines.


### 4. `MESI-Two-Level-L1Cache.sm`

- Added the `uu-profileInstHit` action, which profiles demand hits and triggers the new functions.


## Experimental Setup

### Benchmarks Used

- **Parest (B1)**
- **Leela (B8)**
*(Omnetpp was initially planned but replaced due to technical issues.)*


### Metrics Collected

- **Total L1-I hits and misses**
- **L1-I hits and misses between T1 and T2**
- **Hits per set, hits per way**


#### Example Data (Parest, B1)

| Metric | Value |
| :-- | :-- |
| Total L1-I hits | 3,126,631 |
| Total L1-I misses | 3,622 |
| L1-I hits (T1–T2) | 79,867 |
| L1-I misses (T1–T2) | 0 |

**Hits per Set (partial):**


| Set \# | Hits |
| :-- | :-- |
| 0 | 1560 |
| 1 | 230 |
| ... | ... |
| 22 | 322 |
| 23 | 422 |

## How to Use

1. **Apply the Code Modifications:**
Integrate the changes described above into your local GEM5 source tree.
2. **Rebuild GEM5:**

```
scons build/X86/gem5.opt
```

3. **Run Simulations:**
Use the desired benchmarks and cache configurations.
Example:

```
./build/X86/gem5.opt configs/example/se.py --cmd=parest --l1i_size=32kB --l1i_assoc=4
```

4. **Analyze Output:**
The enhanced statistics will be available in the simulation output, including:
    - Total hits/misses
    - Hits/misses in interval [T1, T2]
    - Hits per set and per way (see `stats.txt`)

## Insights \& Outcomes

- **Spatial Profiling:**
Identifies hot sets/ways, helping to spot imbalances or potential thrashing.
- **Temporal Profiling:**
Allows correlation of cache performance with program phases.
- **Design Exploration:**
Facilitates informed decisions on cache size and associativity for target workloads.


## References

- [GEM5 Documentation](https://www.gem5.org/documentation/)
- Course materials on cache organization and performance





