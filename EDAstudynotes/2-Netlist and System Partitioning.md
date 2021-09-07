# 2 Netlist and System Partitioning

## **2.1 Introduction** 

- The design complexity of modern integrated circuits has reached unprecedented scale

- A common strategy is to *partition* or divide the design into smaller portions

 **manual partitioning** -------------> in the context of system-level modules

 **automated *netlist partitioning*** 

​                      ----------> can handle large netlists 

​                      ---------->can redefine a physical hierarchy of an electronic system

-  A popular approach to decrease the design complexity of modern integrated circuits is to *partition* them into smaller *modules*. 

The partitioner divides the circuit into several subcircuits (partitions or blocks) while **minimizing the number of connections** between partitions

then connections

​                     ---------->    negatively affect the overall design performance

​                                                                      such as increased circuit delay or decreased reliability

​                     ---------->     may introduce inter-block dependencies that hamper design productivity

![image-20210906100231865](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906100231865.png)

## **2.2 Terminology**

A cell is any logical or functional unit built from components.

A partition or block is a grouped collection of components and cells.

The *k-way partitioning* problem seeks to divide a circuit into *k* partitions

nodes represent cells, and edges represent connections between cells. 

![image-20210906100428633](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906100428633.png)

![image-20210906100503360](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906100503360.png)

logic circuits are more accurately represented using hypergraphs.

![image-20210906100728777](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906100728777.png)

## **2.3 Optimization Goals**

The most common partitioning objective is to minimize the number or total weight of cut edges while **balancing the sizes of the partitions**

partition area is limited

Bounded-size partitioning enforces an upper bound UB on the total area of each partition V’

That is, area(Vi) <= UBi, where Vi 包含于 V, i = 1, … , k, and k is the  number of partitions. Often, a circuit **must be divided evenly,** with

![image-20210906102311141](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906102311141.png)

![image-20210906102326862](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906102326862.png)

