## Numbers Everyone Shoud Know
CPU (Central Processing Unit):
The CPU is the "brain" of the computer. Every process on your computer is eventually handled by your CPU. This includes calculations and also instructions for the other components of the compute.

Memory (RAM):
When your program runs, data gets temporarily stored in memory before getting sent to the CPU. Memory is ephemeral storage - when your computer shuts down, the data in the memory is lost.

Storage (SSD or Magnetic Disk):
Storage is used for keeping data over long periods of time. When a program runs, the CPU will direct the memory to temporarily load data from long-term storage.

Network (LAN or the Internet):
Network is the gateway for anything that you need that isn't stored on your computer. The network could connect to other computers in the same room (a Local Area Network) or to a computer on the other side of the world, connected over the internet.

## Hardware: Memory
The CPU is the brains of a computer. The CPU has a few different functions including directing other components of a computer as well as running mathematical calculations. The CPU can also store small amounts of data inside itself in what are called registers. These registers hold data that the CPU is working with at the moment.

For example, say you write a program that reads in a 40 MB data file and then analyzes the file. When you execute the code, the instructions are loaded into the CPU. The CPU then instructs the computer to take the 40 MB from disk and store the data in memory (RAM). If you want to sum a column of data, then the CPU will essentially take two numbers at a time and sum them together. The accumulation of the sum needs to be stored somewhere while the CPU grabs the next number.

This cumulative sum will be stored in a register. The registers make computations more efficient: the registers avoid having to send data unnecessarily back and forth between memory (RAM) and the CPU.

## Hardware: Memory
It seems like the right combination of CPU and memory can help you quickly load and process data. We could build a single computer with lots of CPUs and a ton of memory. The computer would be incredibly fast. Beyond the fact that memory is expensive and ephemeral, we'll learn that for most use cases in the industry, memory and CPU aren't the bottleneck. Instead the storage and network.

## Hardware:Storage
Processing 1 hour of tweets (4.3 Gigabytes)
- Memory: 30ms
- SSD: 0.5s
- Magnetic disk: 4s

## Hardware: Network
Speed comparison: Network < SSD (20x faster than network) < Memory (15x faster than SSD) < CPU (200x faster than memory), therefore network is the biggest bottleneck when working with big data. Distributed system try to minimize the shuffling (moving data back and forth between nodes of a cluster).

## Small Data Numbers

## Big Data Numbers
When the computer start to run the program the larger data set. The CPU couldn't load data quickly from memory and the memory couldn't load data quickly from storage.

## Medium Data Numbers
If a dataset is larger than the size of your RAM, you might still be able to analyze the data on a single computer. By default, the Python pandas library will read in an entire dataset from disk into memory. If the dataset is larger than your computer's memory, the program won't work.

However, the Python pandas library can read in a file in smaller chunks. Thus, if you were going to calculate summary statistics about the dataset such as a sum or count, you could read in a part of the dataset at a time and accumulate the sum or count.


## History of Distributed and Parallel Computing
![image](/imgs/distributed_parallel_computing.png)

## The Hadoop Ecosystem