# **Chapter 3 chip planning** 

## introduction

### **Semiconductor intellectual property core**

In electronic design, a semiconductor intellectual property core (SIP core), IP core, or IP block is a reusable unit of logic, cell, or integrated circuit layout design that is the intellectual property of one party. IP cores can be licensed to another party or owned and used by a single party. The term comes from the licensing of the patent or source code copyright that exists in the design. Designers of application-specific integrated circuits (ASIC) and systems of field-programmable gate array (FPGA) logic can use IP cores as building blocks.



chip planning deals with large modules that have know areas ,fixed or changeable shapes, and possibly fixed locations.

when modules are not clearly specified, chip planning relies on netlist partitioning.

 enables early estimates of interconnect length, circuit delay and chip 

performance



### chip planning consists of three major stages:

Large chip modules are laid out as *blocks* or rectangular shapes (Fig. 3.1).

 (1) *floorplanning*, 

*Floorplanning* determines the locations and dimensions of these shapes                            

​                       optimize chip size, reduce interconnect and improve timing



(2) *pin assignment*, 

*Pin assignment* connects outgoing signal nets to block pins. This step is (ideally) performed before floorplanning, but locations can be updated during and after floorplanning. 



 (3) *power planning*

*Power planning* builds the *power supply network*.

## **3.1 Introduction to Floorplanning**

These blocks can be either *hard* or *soft*. The dimensions and areas of hard blocks are fixed. 

For a soft block, the area is fixed but the aspect ratio can be changed, either 

continuously or in discrete steps. 

The entire **arrangement** of blocks, including their **positions**, is called a floorplan



*The floorplanning stage ensures that (1) every chip module is assigned a shape and* *a location, so as to facilitate gate placement, and (2) every pin that has an external* *connection is assigned a location, so that internal and external nets can be routed.* 



the floorplanning stage determines **the external characteristics-** fixed dimensions and external pin locations – of each module. These characteristics are necessary for the subsequent placement (Chap. 4) and routing (Chaps. 5-7) steps, which determine **the internal characteristics** of the blocks. 



A floorplanning instance commonly includes the following parameters – (1) the area of each module, (2) all potential aspect ratios of each module, and (3) the netlist of all (external) connections incident to the module. 

![image-20210906155516651](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906155516651.png)

## **3.2 Optimization Goals in Floorplanning** 

### **Area and shape of the global bounding box.** 

global bounding box ——the minimum axis-aligned rectangle (最小轴对称矩形) that contains all floorplan blocks.

the area of the global bounding box represents the area of **the top-level floorplan(the full design)** and directly impacts circuit performance, yield, and manufacturing cost.

 another optimization objective is to keep the aspect ratio to a given target value.  For instance, due to manufacturing and package size considerations, a square chip (aspect ratio Approximately equal to 1)may be preferable to a non-square chip.

### **Total wirelength.**

- Long connections between floorplan blocks 

  ​                           ——>increase signal propagation delay 

Therefore, layout of high-performance circuits seeks to **shorten** such interconnects. 

- Switching the logic value carried by a particular net requires **energy** **dissipation** that grows with **wire capacitance**.

 Therefore, **power minimization** may also seek to **shorten** all routes.



To **simplify** computation of the total **wirelength** , one option is to connect all nets to the **centers** of the blocks. It is relatively accurate for **medium-sized** **and small blocks**.



### model connectivity

 2 common approaches to **model connectivity** within a floorplan

 (1) use a connection matrix *C* 

![image-20210906163917516](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906163917516.png)

![image-20210906163932957](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906163932957.png)

 (2) a minimum spanning tree for each net (Sec. 5.6).

Using the first model, the total connection length *L*(*F*) of the floorplan *F* is estimated as 

![image-20210906170700512](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906170700512.png)

![image-20210906170944356](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906170944356.png)

The **center-pin location assumption(假设)** may be improved by using actual pin locations .

 The Manhattan distance wiring cost approximation may be improved by **using pin-to-pin shortest paths** in a graph representation of **available routing resources.** 

 With these refinements, wiring estimation in a floorplan relies on the construction of **heuristic Steiner minimum trees in a weighted graph** (Chap. 5). 

### **Combination of area and total wirelength**

To reduce both the total area *area*(*F*) and the total wirelength *L*(*F*) of floorplan *F*, it is common to minimize

![image-20210906173443365](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906173443365.png)

where the parameter 0 <= a <= 1 gives the relative importance between *area*(*F*) and *L*(*F*). Other terms, such as the aspect ratio of the floorplan, can be added to this objective function [3.3].

- In practice, the area of the global bounding box may be a constraint rather than an optimization objective. 

​                  when the package size and its cavity dimensions are fixed.

​                  when the global bounding box is part of a higher-level 

​                                       system organization across multiple chips.       

​              **(the *fixed-outline* *floorplanning problem*)**

### **Signal delays**

A desirable quality of a floorplan is short wires connecting its blocks, such that all timing requirements are met. Often, critical paths and nets are given priority during floorplanning so that they span short distances. 

## **3.3 Terminology** 

A *rectangular dissection* is a division of the chip area into a set of *blocks* or 

non-overlapping rectangles. 

###  *slicing tree* or *slicing floorplan tree*

A *slicing tree* or *slicing floorplan tree* is a binary tree with *k* leaves and *k* – 1 internal nodes, where each **leaf** represents a **block** and each **internal node** represents a **horizontal or vertical cut line** 

 This book uses a standard notation, denoting **horizontal** and <u>vertical</u> cuts by ***H*** and *<u>V</u>*, respectively. 

![image-20210906194405240](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906194405240.png)

A *floorplan tree* is a tree that represents a hierarchical floorplan (Fig. 3.4). Each leaf node represents a block while each internal node represents either a *horizontal cut*(*H*), a *vertical cut* (*V*), **or a *wheel* (*W*).** The *order* of the floorplan tree is the number of its internal (non-leaf) nodes. 

![image-20210906200825928](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906200825928.png)

### **constraint-graph pair** 

![image-20210906203316637](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906203316637.png)

In a *vertical constraint graph* (*VCG*)

node weights represent the heights of the corresponding blocks. Two nodes vi and vj, with corresponding blocks mi and mj, are 

connected with a directed edge from vi to vj if mi is below mj. 

![image-20210906203925800](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906203925800.png)

In a *horizontal constraint graph* (*HCG*),

 node weights represent the widths of the corresponding blocks. Two nodes vi and vj, with corresponding blocks mi and mj, are connected with a directed edge fromvi to vj if mi is the left of mj.  

![image-20210906203944499](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906203944499.png)

**The longest path** in the VCG corresponds to **the minimum vertical extent** required to pack the blocks (floorplan height). Similarly, **the longest path** in the HCG corresponds to **the minimum horizontal extent** required (floorplan width). 



A *sequence pair* is an ordered pair (*S**+*,*S**í*) of block permutations

![image-20210906214158437](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906214158437.png)

![image-20210906214225475](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906214225475.png)

![image-20210906214234731](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20210906214234731.png)

## **3.4 Floorplan Representations** 

