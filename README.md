# Hardware
Information about all parts of hardware

## SSDs
Information about memory and storage

### Resources
Great Arcticle on HMB in NVME Drives: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7051071/ <br/>
Game Loading Compared: https://www.techspot.com/review/2116-storage-speed-game-loading/ <br/>
Newmaxx SSD Sheet: https://docs.google.com/spreadsheets/d/1B27_j9NDPU3cNlj2HKcrfpJKHkOf-Oi1DbuuQva2gT4/edit#gid=0 <br/>
Real Life Span of SSDs Tested (6 years old): https://techreport.com/review/27909/the-ssd-endurance-experiment-theyre-all-dead/ <br/>

 
### Hardware

Solid state drives, or SSDs, are non-volatile storage devices made up of a few basic components. The choice and combination of these components determines the drive's performance and its intended role. Basic structure of an SSD-

### Controller

Controllers are basically specialized RISC devices - reduced instruction set, in contrast to complex - that are optimized for real-time, low latency operations and lots of them (IOPS). These type of embedded devices, as made by ARM for example, are in everything - your car, your smartphone, embedded devices, etc. Most SSD controllers are based on the Cortex-R5 specifically which you can google for more information.

Controller is short for "microcontroller" (which is synonymous with RISC) but they're actually ASICs because they had other contained functions like error correction (ECC), a DRAM controller (if there's DRAM on the drive), etc. You can get block diagrams of them if you're curious, just look for the PDF for SMI'S SM2262/EN for example. In short there is a bus between the controller and flash/NAND where commands and data are transferred, signaling in a "hertz" or cycle way as with other electronics.

If you're asking about the difference in controllers, as in SM2258 vs. SM2262 for example, typically you have different core counts and clock speeds. More cores and higher clocks generally mean higher performance - in this case, the NVMe-based SM2262 has twice the cores of the SATA SM2258.

Components of a controller-

1. Host interface- The host interface of a controller is typically designed to one specific interface specification. There are several interfaces made for different system and design requirements.

2. SMART- The SMART function, available in some controllers, monitors data regarding attributes of the SSD and memory. 

3. Wear leveling- Wear Leveling is the ability to even out the number of write cycles throughout the present NAND

4. Encrypt & Decrypt Engine- For security applications, a hardware encryption and decryption engine is built into the silicon of the controller for increasing speed for encrypting/decrypting. The most popular one being AES256.

5. Buffer/Cache- Controllers generally have a high speed SRAM or DRAM cache buffer used for buffering the read and write data of the SSD. Since this cache uses volatile memory, all data is lost if power is lost. It is common to see both internal caches in the controller chip itself as well as external cache chips.

6. CPU/RISC- The heart of every SSD is the main processing core. The size and performance of the processor determines how capable the controller can be.

7. Write Abort- Write Abort is the when power to the SSD is lost during a write to the NAND flash. Without a battery or backed cache, this data will be lost. The important aspect of this is to ensure the SSD’s internal firmware remain uncorrupted

8. NAND Memory Interface- Depending on the controller there can be a single NAND channel up to 10 or more. Each channel can have one or more NAND chips.

9. Defect Management- Every controller needs a method to deal with bad blocks of memory and new defects. At the point a NAND block becomes unusable. In some cases a spare sector replaces the failed block. In a poor controller design, the SSD fails.

10. ECC Engine- Error Checking & Correction are a key part of today’s SSD. ECC will correct up to a certain number of bits per block of data

### Volatile memory (SRAM/DRAM)

Memory is volatile when it loses its data or contents on power loss. In the context of SSDs volatile memory is utilized to temporarily cache the controller's firmware, store information from read-only memory for debugging, manage controller functions, temporarily store boot code, and store various metadata (data about data) for use with the FTL. Enterprise drives often have power loss protection but it is not often found in consumer drives.
https://en.wikipedia.org/wiki/Static_random-access_memory <br/>
To sum it up, SRAM uses latching circuitry to store each bit, it is volatile as well.
DRAM is a type of random-access semiconductor memory that stores each bit of data in a memory cell consisting of a tiny capacitor and a transistor, both typically based on MOS technology, SSD controllers will also  have access to DRAM which is several orders of magnitude faster to access than the flash. This DRAM is mostly used for storing metadata.
DRAM tends to be DDR3 or DDR4 currently, often with a low-power variant.
DRAM/SRAM reduces write amplification by deferring. https://www.romexsoftware.com/en-us/primo-cache/terms-configuration.html <br/>
Some NVMe controllers can also use system memory as an external DRAM cache using a method known as host memory buffer (HMB)

DRAM-less SSDs are often relatively inexpensive (with a few exceptions) and are attractive to many people, largely because they are cheap, and come from a large/known company. Take crucial (bx500) and Kingston (A400) as examples.

These SSDs do not have a DRAM cache beyond the minimum required by the controller itself and therefore save the index of files on NAND flash, which is slower. And can therefore often drop rapidly at their speed, making it feel a lot slower, at worst (in combination with a slow controller and slow/low quality NAND) almost equivalent to a modern CMR 7200rpm hdd. 

This is mainly a problem with some 2.5'' SATA SSDs, as some DRAM-less NVMe SSDs use HMB as mentioned above, which uses part of the internal RAM as DRAM cache for the SSD. It is also possible that an SLC write cache is used, where the SSD instead of 3 bits/layer writes only 1 and then transfers it to a 3 bit system when not in use. Because of this, the writing will be (almost) as fast as a good DRAM Sata or sometimes even NVMe. Some good examples of this are SN550, Z330, MP33 and A60.

The only reason to buy a DRAM-less  SSD should be to upgrade an older system for faster boot times, as secondary storage or in other tasks where the speed of a good SSD is not necessarily needed. Ideally not at all since low capacity NVME drives using HMB are often very cheap.

--more info here https://www.anandtech.com/show/12819/the-toshiba-rc100-ssd-review/2

Configuration Terms
PrimoCache Help Documents - Terminology - Configuration Terms
The Toshiba RC100 SSD Review: Tiny Drive In A Big Market

The FTL with regard to DRAM will have two parts: the allocator, which focuses on addressing, and a separate collector for garbage collection at the block level. The amount of DRAM required is dependent on the type of workload with random operations requiring more metadata accesses/updates than sequential
Elements that can be stored and tracked within a SSD's DRAM cache--http://borecraft.com/files/w9TGR4MYH3.png

Nand Flash

https://www.silicon-power.com/blog/index.php/guides/nand-flash-memory-technology-basics/ <br/>
TL;DR- 

Flash memory is a non-volatile solid-state storage medium that relies on electric circuits to store and retrieve your data. <br/>
https://www.silicon-power.com/blog/wp-content/uploads/2018/04/NAND-Flash-Chip-Layout-2.jpg


NOR Flash vs. NAND Flash- <br/>
https://www.silicon-power.com/blog/wp-content/uploads/2018/03/NOR-Flash-Memory-Grid-1024x819.png <br/>
https://www.silicon-power.com/blog/wp-content/uploads/2018/03/NAND-Flash-Memory-Grid-1024x819.png <br/>


In NOR flash, each cell requires a word line connector and bit line connector. NAND Flash links an entire cell column by running the bit line through each cell.
Floating Gate Transistors-
Each NAND Flash memory cell contains a Floating Gate Transistor, which is where the Program/Erase cycle of a NAND Flash cell takes place.
An SSD’s capacity is determined by how many bits it can store per cell, hence the terms Single Level Cell (SLC), Multi Level Cell (MLC), Triple level cell (TLC) and Quad level Cell (QLC)
SLC has the least cell density, so operations like programming and erasing are less complex. For example, when a cell contains one bit, its bit state can only read as a 1 or 0.
By introducing multiple bits per cell you create a larger margin for error.

https://www.reddit.com/r/NewMaxx/comments/gcbaxi/ssd_resource_compilation/ <br/>
https://www.reddit.com/r/NewMaxx/comments/dhvrdm/ssd_guides_resources/ <br/>
https://ssd.borecraft.com/ <br/>


## Coolers

One of the most important decisions when building your PC, is choosing the right cooler for your needs. Your cooler choice can and will also make a substantial difference in noise output/ Thermals. Buying a cooler that can handle your CPU is critical to avoiding throttling and achieving your system’s full potential. CPU Coolers fall into one of three primary categories: air, closed-loop or all-in one (AIO) coolers, or custom /open-loop cooling setups. Today we are going to be looking at which cooler you should be taking that fit's your needs and what to look for in a cooler.

### [  What To Look for In a Cooler Pt. 1 -Compatibility ]  

Whether you’re opting for air, an AIO, or custom water loop, you need to make sure it is compatible with the rest of your system and is optimal for your needs. Several factors come into play here such as but not limited to :-
-CPU socket
-Cooler height
-Radiator Size

Typically coolers include mounting adapters for several sockets, Threadripper processors have their own mounting and larger cold plate areas therefore support for those is limited to select coolers which tend to have "TR4" in their product name. 
it’s important to look at specifications for what size heatsink or radiator is supported and the amount of clearance under the cooler for the RAM slots. If you plant to use sticks with tall heat spreaders they you must make sure that your cooler allows enough clearance for memory

For liquid cooling, amount of clearance under the cooler for the RAM slots is crucial in picking the right cooler. Case manufactures tend to list the radiator mounting location and sizes

### [ What To Look for In a Cooler Pt. 2 - "Should I go for a liquid cooling or Air cooling?" ]

If price and ease of installation are your main factors then I suggest you should move onto looking for an Air Cooler. However, if you want a quieter cooler (may vary) or/and a jazzy looking cooler, then I suggest you head onto liquid cooling. Really, it's that simple. For relatively high overclocks I'd also recommend going with liquid, due to the way water transfers and absorbs heat compared to fins in a heatsink. You would be looking at 240/280mm AIO's for 6-8 cores, and 360mm/custom loop/chiller setup for 10+ cores. Note that there are CPU's with an enormous amount of cores that clock fairly high, even a very good AIO won't keep these cool. So do research in advance.

There are also expandable kits out there which allow you to expand your CLC (Custom Loop Configuration) to other components like a custom loop.

### [  What To Look for In a Cooler Pt. 3 - Budget ]  

This is an important factor in deciding the right cooler for you. If you plan on spending less than $80 or so on a cooler then might as well forget the thought of liquid cooling. Air is the best option out there for you. If you plan on paying mid range prices, let's say $80+ then you can either look at a high end air cooler or you can go and buy yourself an AIO. Mid-range AIOs are plenty capable of keeping most processors within safe temperature ranges, including when overclocking. If you plan on going down the Custom Loop road and have a budget of of around the 100-200 mark then really, forget it. Custom Loops of high quality cost upwards of 500 bucks (prices may vary). Now that budget is out of the way let's move onto the next factor.

### [  What To Look for In a Cooler Pt. 4 - Heatsinks ]  
A larger chunk of grooved/finned metal provides more area for the heat to distribute itself. Big being better, in this case - just make sure you choose something that makes sense for your system. Grabbing the heaviest heatsink out there won't matter if it doesn't fit in the case and puts too much strain on the CPU or motherboard. Just grabbing any massive aluminum heatsink is probably not for the best, of course, given the importance of heatpipes, surface smoothness, and copper's place in the world. Surface roughness is a measurement of the base plate's smoothness (measured in microinches) and overall ability to connect directly with the surface of the CPU. In a perfect world, there would be no thermalpaste and the copper base plates would come in direct, flush, perfectly smooth contact with the CPU. The reason we even need thermalpaste is because microscopic divets (what are divets) in the surface of the connecting materials create air pockets. Air gets trapped in these pockets at high temperatures, causing uneven thermal distribution and resulting in hotter core temps. A thermal interface, while significantly lower thermal conductivity than pure copper or aluminum, provides an air-tight sealant between the divets that allows heat to cleanly migrate from the CPU surface to the cooler base plate. Smoother is better. Thermalpaste's thermal conductivity will impact the temperature moderately, but not normally enough where it's justifiable to spend lots of money on thermal compound


## Motherboards 101

### How To Pick A Motherboard 
There are three steps to pick a motherboard
1. Remove all the garbage motherboards. This includes Biostar, motherboards not compatible (socket wise), motherboards that will melt with your CPU in it, and motherboards with no rear IO (Laptop tier).
2. Go down the below checklist laid out below and determine what features you need
3. Sort by price, and pick a motherboard

[ What you can figure out ]

- Rear IO (USB plus other ports) selection
- Front internal connectors like USB Type C
- Number and placement of expansion and M.2 slots
- I guess looks too
- Price
What most people need help on
- Quality of VRMs and heatsinks (relating to your specific CPU and how much you're overclocking it. Advice here, get a motherboard that won't thermal throttle your processor or easily degrade, and after that, it doesn't matter if you're not overclocking)
- Memory topology (or how well you can overclock your RAM if you want to)
- RGB compatibility and header amount/type
- Chipset itself

The notation is that you should use a checklist and search by the motherboards that fit your criteria and then ask for help on your unknowns. A lot of knowledge can be very granular and not easily accessible if you don't know what you'e looking for. All you need to know is most people don't need a ridiculously expensive motherboard to get the features they want. Remember, motherboards do not DIRECTLY impact performance! You shouldn't be compromising on the CPU choice in order to get a motherboard you won't take advantage of.

### [ General Resources ]
MOTHERBOARD CULTIST (Z590 + TR4 + AM4): https://docs.google.com/spreadsheets/d/1OFGHnF4_LF_JBR8K2bjvzzgvRG0mIwwWDHl5A-4D8xQ/edit?usp=sharing <br/>
LGA1200 VRMs: https://docs.google.com/spreadsheets/d/1yPS3hj_K7EPT4RBWCyjdKNP56pnwDz-IgBc0975-FUg/edit#gid=0 <br/>
B550 VRMs: https://docs.google.com/spreadsheets/d/1PuUWroxA0HvSSipsXlB8hnYkshxD8LdeO5EA6WLdOQw/edit#gid=0 <br/>
AM4 Vcore (Missing B550): https://docs.google.com/spreadsheets/d/1d9_E3h8bLp-TXr-0zTJFqqVxdCR9daIVNyMatydkpFA/edit#gid=611478281 <br/>
Geeking Out on Every Single Motherboard's VRMs: https://docs.google.com/spreadsheets/d/1Smj5dh97n32wJqm5dkdDcQt8ID7vH52-lKzaaXUUQx8/edit#gid=0 <br/>
Z490 Lineup: https://docs.google.com/spreadsheets/d/1TWJGQY8HaIF-iwfSeKLIFD9WNMsdyHkgGBa312gE5mE/edit#gid=0 <br/>
AM4 Lineup: https://docs.google.com/spreadsheets/d/1wmsTYK9Z3-jUX5LGRoFnsZYZiW1pfiDZnKCjaXyzd1o/edit#gid=2112472504 <br/>


### [ Learning about Concepts ]
Motherboard Memory Topology: https://youtu.be/3vQwGGbW1AE <br/>
Motherboard Transient Response: https://youtu.be/ml02z3NsHfE <br/>
Load Line Calibration (LLC): https://youtu.be/bUaP0r5-xhY <br/>
VRM Doublers: https://youtu.be/tQjY9ni8uu4 <br/>
How VRMs actually work: https://youtu.be/oDRHV3qtSWc <br/>

### [ Specific Notes ]
While I use HWU's videos on VRM temps to gauge how bad of a board I am looking at, there is a big asterisk next to it:

1. Higher layer PCB boards dissipate heat better, so the backside VRM temp would appear to be lower (since we are measuring there)
2. Since we can't actually measure the temp inside of the MOSFETs, we can only measure the ceramic casing (or not in this case since we're on the back side of the board). 
3. Not necessarily optimal operating temperatures
4. A lot of FETs don't have internal temperature sensors (voiding software)
5. Doesn't actually tell us transient response, voltage regulation, nor ripple supression

Some safe operating temperatures (not recommended temperatures) for junctions can be 150C. This is for the internal of the MOSFET. Since ceramic is an insulator, when we take a temperature probe and stick it on or next to the FET itself, our temperature will be lower than the FET actually is which means if we measure 150C on the outside, we're out of specification. If we use a datasheet on our specific FET we have, we can find the ratio between these two temperatures (called the Thermal resistance between junction and case). Assuming a ratio of 1.6 C/W (for every 1.6 watt of heat our FET produces, we can measure 1 outside) on average (power loss curve is exponential for FETs so only derivatives are really applicable here), we will on average see a lower temperatures outside. Very high thermal resistance is not good (above 4-6).

If we resort to a SiC639 50A power stage (which is very common among B550 motherboards), let's estimate we push 30 amps through it normally. We have 4W of heat loss. Multiply that by our thermal resistance of 1.6, we're around 6.5 C higher on the inside of the MOSFET than what we can measure externally. 120C is still safe for our maximum operating temperature (still not optimal though) and with our 6.5 degree increase, because heat is dissipated, our PCB linearly decreases its current output (not good) and we're past what recommended temperatures are (even though we're still near 86% efficient).

Even looking back at the power loss vs current output, our 50A rated power stage doesn't even have 50A labelled on the graph, it stops at 45A (continuously drains). While this is safe, the FET may be dead before its rated lifetime if we max out our temperature rating.

### Conclusion
VRMs temperatures are hard to decipher if you don't know what you are looking for. Blindly stating VRMs are absolutely irrelevant loses all credibility in that person's statement. My suggestion is to get a motherboard that won't thermal throttle your processor or easily degrade, and after that, it doesn't matter if you're not overclocking. If you are, then we start looking at configurations, amperage ratings, input/output filters, and heatsinks.

 

https://www.youtube.com/watch?v=zDxFbAhu4Bo&&ab_channel=Steve%27sHardware <br/>


https://www.youtube.com/watch?v=Viitg4Yoy2Y&t=3s&ab_channel=Steve%27sHardware <br/>


https://www.youtube.com/watch?v=6Fl1iFtOLKU&t=7s&ab_channel=Steve%27sHardware <br/>


Extremely outdated components and a few outdated concepts, plus a shitty mic, but this is an EXTREMELY good document for learning how VRMs work
https://en.wikichip.org/wiki/voltage_regulator_module

Wikichip article on VRMs

https://vtechworks.lib.vt.edu/bitstream/handle/10919/26395/Chapter1.pdf?sequence=1&isAllowed=y

https://www.powerelectronics.com/content/article/21855804/interleaved-vrm-cuts-ripple-improves-transient-response

BIOS Flashback instructions: https://www.youtube.com/watch?v=oqq5O4wUmAA
(use this video instead of the MSI one because Luumi is based)

https://www.overclock.net/threads/asrock-z590-oc-formula-overclocking-thread.1778037/
Overclock.net
ASRock Z590 OC Formula Overclocking Thread
Even though it's not in retail stores yet, I wanted to get the thread started for the new ASRock Z590 OC Formula board!! This is the first mainstream 'OC Formula' board since the legendary Z170M OC Formula, so I figured we need a place to talk about it and share information. I also wanted to...

## RAM 101

RAM Frequency or Speed: https://www.tomshardware.com/reviews/pc-memory-ram-frequency-timings,6328.html <br/>
RAM on Ryzen 3000: https://www.tomshardware.com/reviews/amd-ryzen-3000-best-memory-timings,6310-2.html <br/>
RAM OC on Zen Explained: https://www.reddit.com/r/overclocking/comments/ahs5a2/demystifying_memory_overclocking_on_ryzen_oc/ <br/>
What is Gear Down Mode (GDM)? https://www.linkedin.com/pulse/what-ddr4-memory-gear-down-mode-barbara-aichinger <br/>
TL;DR
Gear-Down mode is a Reliability, Availability and Serviceability (aka RAS) feature more clearly documented in the new JEDEC DDR4 Rev B spec. Gear-down mode allows the DRAM Address/Command and Control bus to use every other rising clock of the DDR4 Memory bus clock.

### [ Resources for Monitoring and Stress Testing Memory ]
- For checking current timings on Zen: https://zentimings.protonrom.com/ 
- For checking current timings on Comet Lake +: https://asrock-timing-configurator.software.informer.com/download/
- For benchmarking frametimes: https://www.capframex.com/


1.  Prime95 800k (https://www.mersenne.org/download/) + Aida 64 Cache (https://www.aida64.com/downloads) -- simultaneously
2. For hardcore stress testing: http://testmem.tz.ru/tm5.rar with profile (https://drive.google.com/file/d/1uegPn9ZuUoWxOssCP4PjMjGW9eC_1VJA/)
3. OCCT testing (not just memory): https://www.ocbase.com/
3. MemTest (not x86) -- Open up a couple installations each with 2000 MBs allocated: https://hcidesign.com/memtest/

### [ Basic Things to Remember ]
Every manufacturer that sells RAM wants you to be able to slot your RAM in, click the power button, and then it works. So the easiest way to do this is to set looser timings and really low frequency because this ensures stability and compatibility with the rest of your system, also known as JEDEC specification. What XMP or DOCP does is it just takes that slow speed and then runs the kit at whatever the package you bought online says it was rated for (most of the time usually). For example, our RAM kit may run 2400c15 out of the box, and when we enable XMP, it runs at 3200c16. Though there is a technology called PnP (Plug-'N-Play) by Kingston, but it's not on grand scale for consumers at all.
Usually only DDR4-3466 CL19-23-23 and DDR4-3200 CL18-21-21, which both are reasonably slower compared to DDR4-3200 16-18-18 on XMP/DOCP.

Manually overclocking is where you go into the BIOS and, by hand, enter individual voltages, frequencies, and timings to maximize the performance out of your RAM kit. This can take a lot of hours to tune the timings and voltages to stable levels and then stress testing the system--not recommended for beginners.
 
### There are three types of users that buy RAM:
1. No manual overclock, yes XMP -> Regular 3000/3200C15/16 kits (SpecTek, MFR, AFR, etc. -- can be a host of different dies, doesn't really matter in most cases)
2. Yes, little manual overclock, no XMP -> Rev. E /SR Rev. B / CJR / DJR / Cheap B-die
3. People who want to overclock a lot and get the most performance -> Properly binned B-die such as 3600 14-14-14-34 or even higher bins.

People sometimes overestimate the performance of RAM and the gains therefrom. You can either get slow or fast RAM (or even leave it at JEDEC values). Stick to a cheaper kit, and if you have the budget, the want, and time to manually overclock, then consider it. 3600c16 Doesn't fit in here at all. Once we start overclocking, we care about the memory ICs used.
 
You can put two different RAM kits together; however, depending on the specific model from the specific manufacturer, you can get varying chips--which can cause some problems stability wise. To ensure complete compatibility, you just have to know the specific dies, ranks, and PCB revisions which may be almost impossible. It is just easier to buy the same exact kit if you currently have another one or just get a larger kit that has more sticks rated to run together with each other. Though this does not work for Corsair and G.skill, since they use different versions within the same SKU (Moostly 3200 MT/s CL16 and 3600MT/s CL18 kits). Examples and version numbers stated below.
 
SK Hynix, Samsung, etc. couldn't care less about you as a consumer. They don't care what memory chip quality goes into a memory stick as long as it meets JEDEC. For overclocking, even a single bad chip out of eight can stop you in your tracks. This is why you should probably buy your RAM from Corsair (Yes they do have some proper kits), G.skill, etc. that do their own binning processes.
 
So when people ask about what brand of RAM is the most "reliable", I like to tell them this...None of them. At the 3000c15, 3200c16, 3600c18 there can be a ton of different dies. From Nanya C to reject Samsung B, we can get everything. A lot of it is just what the "brand" had on the manufacturing floor that day (because brands like Corsair and G.skill don't actually make the DRAM that actually make RAM what it is). Most of these (besides the Crucial Ballistix kits because Micron just likes to shove Rev.E in every 3000-3600 kit [or 4000 if 2x8]) dies are just set XMP and never-touch-it-again-type-of-thing. You don't know what you're going to get and most of them won't overclock well at all.

Brands like Corsair, G.skill, etc. take the memory ICs they get from Samsung, change or completely redesign a PCB for their sticks like adding extra capacitors or SMT (Surface-mount technology) on the back or making room for a RGB circuit, so the layout varies from module to module. In fact, the dominators are custom PCBs that allow the heatspreader to screw directly into the PCB. Others just copy the JEDEC PCB, and with the dies they bought, slap it on memory sticks and bin them to those speed bins.

 
### [ Advanced Topics ]
Motherboard Memory Topology and RAM Configuration Optimization: https://youtu.be/3vQwGGbW1AE <br/>
XMP Performance Tested: https://www.youtube.com/watch?v=25NW8cHNrgA <br/>
Memory Ranks Tested (properly): https://youtu.be/1ZbxMh5xUOg <br/>
Response Video: https://youtu.be/M7fDNBaQ7og <br/>
Dealing with Temperature: https://youtu.be/iCD0ih4qzHw <br/>
Memory Channels and Ranks: https://youtu.be/tySToFdLV2g <br/>
DDR4 PCBs: https://docs.google.com/spreadsheets/d/1O9oWdgE4-1Y09l3HozwCf46nqqUHRL-w9u57KU_KMnw/edit?usp=sharing <br/>
PCBs by Buildzoid: https://youtu.be/ZJDXsoYKZaY <br/>
BZ on FCLK on Ryzen 3000: https://youtu.be/nugwAOvijHQ <br/>
BZ Pt2: https://youtu.be/10pYf9wqFFY <br/>

### [ RAM Overclocking/Improvement Benefits ]
Testing by KingFaris (Highly recommended as main source, since it shows full timings and methodology): https://kingfaris.co.uk/ram/7 <br/>
Best DDR4 overclocking guide out there in my opinion: https://github.com/integralfx/MemTestHelper/blob/master/DDR4%20OC%20Guide.md <br/>

GamerNexus simple (Frequency and tCL) testing: https://youtu.be/9IY_KlkQK1Q <br/>
RAM on Zen+: https://www.computerbase.de/2019-03/amd-ryzen-cpu-ddr4-ram/2/ <br/>
3900X/5900X testing:< https://imgur.com/A7CYHrj%3E <br/>
9900k Fortnite: https://images-ext-2.discordapp.net/external/gzHSFVD40LrZf1ZqK0RLMCh-PIIcs8DgfQ2fn2mdM50/%3Fwidth%3D1009%26height%3D670/https/media.discordapp.net/attachments/363379361681899523/790784054231957544/unknown.png <br/> 
Different RAM speeds and timings tested across some games: https://docs.google.com/spreadsheets/d/1xTJUq4R3CeEooYdHsdTARmBaw-po8NGoRrbL333BQBc/edit?usp=sharing <br/>

### [ General Resources ]

 DDR4 revisions of PCBs: https://cdn.discordapp.com/attachments/757967245988331654/818819141804228648/ddr4pcbs-v1-prev.png

B-Die XMP Predictions: https://docs.google.com/spreadsheets/d/1Xz_rQgNFQF3Dm0yHJBzldVkal5jmfI9Ug9tnpjky5Bc/edit#gid=0

### [ Notes about Certain Kits ]

- Samsung C-die scales negatively past 1.35v, and sometimes even degrades the silicon
- Crucial does not use any Hynix memory ICs
- 8Gbit Rev.B tRAS is very loose. tRCDRD must be loose to overclock too -- doesn't scale with voltage on majority of timings
- 8Gbit Rev.E tRCD is still loose. tRFC is loose too
- S8B can do very very low tRCD and tRFC


RAM Tierlist for Single Rank Dimms
https://media.discordapp.net/attachments/788973277916692490/803468502358294538/unknown.png

### [ Notable 2x8 Kits ]
Micron Rev.E - Crucial Ballistix 2x8GB @ 3000c15 -- You bought an B460 board... <br/>
Micron Rev.E - Crucial Ballistix 2x8GB @ 3200c16 -- Cheap kit for overclocking your frequency a little bit -- around 4000MT/s with loose CL16/17 <br/>
Micron Rev.E - Crucial Ballistix 2x8GB @ 3600c16 -- Higher bin of the 3200c16 variant, consider this if it is on sale or if you want a very decent XMP (3600MT/s 
16-18-18-38) <br/>
Samsung B - Team T-Force DARK PRO 2x8GB @ 3466c16 -- Cheap and non-reject kit of Samsung B-die (better than above kits) <br/>
Samsung B - Team T-Force DARK PRO 2x8GB @ 3200c14 -- Tightest bin at this frequency <br/>
Samsung B - OLOy Blade RGB 2x8GB @ 3600c14 -- Very tight RGB bin, doesn't scale with voltage as well as dark pro but does reach 4000c14 at 1.58V on average <br/>

### [ Notable 2x16 Kits ]
Low: https://pcpartpicker.com/product/BZwqqs/gskill-ripjaws-v-series-32-gb-2-x-16-gb-ddr4-3200-cl14-memory-f4-3200c14d-32gvk <br/>
RGB: https://pcpartpicker.com/product/Y9jNnQ/gskill-trident-z-rgb-32-gb-2-x-16-gb-ddr4-4000-cl17-memory-f4-4000c17d-32gtzrb <br/>
Non-RGB https://www.newegg.com/g-skill-32gb-288-pin-ddr4-sdram/p/N82E168203740214 <br/>
 
the breakdown of DDR4 Nanya is as follows basically <br/>
4Gb B that barely does 3100 and has a suspicious Micron-ish Z80B code in IBIS. EOL. <br/>
8Gb A that is pretty old and not overly consistent but usually does 3200 <br/>
8Gb B clocks like medium bin S8B but worse performance in the real world. <br/>
8Gb C that exists but hasn't been confirmed so to speak in the wild <br/>
4Gb D that exists and unless Thaiphoon now has trouble recognising memory manufacturers, does 3333 <br/>
and a great many partially marked units that are all over the place, with the most recent ones going pretty high and pretty tight (B-die-ish tRFC, almost flat  primaries, scales up to 1.45V w/o any degradation info as of now)


## Identify your Corsair sticks:


  | Version        | IC                                                           | Confirmed/presumed?              |
  | -------------- | ------------------------------------------------------------ | -------------------------------- |
  |  3.20          |   Micron 4Gbit Rev.A                                         |   Presumed                       |
  |  3.21          |   Micron 4Gbit Rev.B                                         |   Confirmed                      | 
  |  3.22          |   Micron 4Gbit Rev.E*                                        |   Speculated                     |                      
  |  3.22          |   Micron 4Gbit Rev.F*                                        |   Confirmed                      |
  |  3.31          |   Micron 8Gbit Rev.B                                         |   Confirmed                      |
  |  3.31          |   Micron 8Gbit Rev.D                                         |   Presumed                       |
  |  3.31          |   Micron 8Gbit Rev.E                                         |   Confirmed                      |
  |  3.32          |   Micron 8Gbit Rev.H                                         |   Confirmed                      |
  |  3.32          |   Micron ??? wk27 '17 2x8GB 2666 16-18-18-36 1.2V            |   ?                              |            
  |  3.32          |   Micron ??? wk46 '19 2x8GB 3000 15-17-17-35 1.35V           |   ?                              |
  |  3.40          |   Micron 16Gbit Rev.B (2133 bin)                             |   Confirmed                      |
  |  3.41          |   Micron ???  wk44 '20 2x16GB 3600 18-22-22-42 1.35V         |   ?                              |
  |  3.43          |   Micron ??? wk43 '20 2x16GB 3200 16-19-19-36 1.35V          |   ?                              |
  |  3.44          |   Micron 16Gbit Rev.B (2666 bin)                             |   Confirmed                      |
  |  4.14          |   Samsung 4Gbit D-die (4x16)                                 |   Confirmed                      |
  |  4.23          |   Samsung 4Gbit D-die                                        |   Confirmed                      |
  |  4.24          |   Samsung 4Gbit E-die                                        |   Confirmed                      |
  |  4.21          |   Samsung 8Gbit B-die (4x16)                                 |   Presumed                       |
  |  4.31          |   Samsung 8Gbit B-die                                        |   Confirmed                      |
  |  4.32          |   Samsung 8Gbit C-Die                                        |   Confirmed                      |
  |  4.49          |   Samsung 16Gbit M-die                                       |   Presumed                       |
  |  4.40          |   Samsung 16Gbit A-die                                       |   Speculated                     |
  |  5.29          |   Hynix 4Gbit MFR                                            |   Confirmed                      |
  |  5.20          |   Hynix 4Gbit AFR                                            |   Confirmed                      |
  |  5.21          |   Hynix 4Gbit BJR                                            |   Speculate                      |
  |  5.22          |   Hynix 4Gbit CJR                                            |   Presumed                       |
  |  5.39          |   Hynix 8Gbit MFR                                            |   Confirmed                      |
  |  5.30          |   Hynix 8Gbit AFR                                            |   Confirmed                      |
  |  5.31          |   Hynix 8Gbit "BFR"???                                       |   Speculated                     |
  |  5.32          |   Hynix 8Gbit CJR                                            |   Confirmed                      |
  |  5.33          |   Hynix 8Gbit DJR                                            |   Presumed                       |
  |  5.38          |   Hynix 8Gbit JJR                                            |   Presumed                       |
  |  5.49          |   Hynix 16Gbit MJR                                           |   Presumed                       |
  |  8.20          |   Nanya 4Gbit Rev.A                                          |   Speculated                     |
  |  8.21^         |   Nanya 4Gbit Rev.B                                          |   Presumed                       |
  |  8.23^         |   Nanya 4Gbit Rev.D                                          |   Presumed                       |
  |  8.30          |   Nanya 8Gbit Rev.A                                          |   Presumed                       |
  |   8.31         |   Nanya 8Gbit Rev.B                                          |   Confirmed                      |
  
  
*Rev.F is confirmed to come in ver3.22 sticks, but that doesn't leave a gap for Rev.E. It's wildly guessed that they may both appear under 3.22.
^Version number seen in the wild, IC unconfirmed.

## PSUs 101


### How Power Supplies Work for Noobs
The PSU takes AC 120~240V from the socket, electricity goes through one side of the PSU and comes out the other side, turns it into 12VDC. There's also 5V & 3.3V rails for other components, but those are not really important. The motherboard can then take 12V and uses its VRMs to turn it into DC ~1.1-1.375V (depends on what you set it to, these are common safe voltages) for the CPU.

120V 10A gets transformed into 12V 100A, then on the motherboard into 1.2V 1000A. (Current ratings are dependable. Remember, the PSU will only draw the power it needs).
⠀⠀
### A Too Long Didn't Read on How to Pick a Power Supply

If a manufacturer or brand didn't bother to even get an 80 Plus or ETA efficiency certification, it's not worth looking at for consumers. After that, there are very very few passable 80+ (white/no color) units so usually skip that too (MWE V2 is "acceptable" versus group regulated). Additionally, some manufacturers just slap a 80+ sticker on it without approval (or fake testing). At Bronze level it starts getting harder to discern what is and isn't a piece of crap. General baseline you want them to rate full or near full power on the 12V rail so a 650w power supply should be able to provide ~649w on the 12v rail and so on (generally means it's not group regulated). After that look for professional reviews/testing.
⠀⠀
### How to Learn about Power Supplies
Power supplies are very complicated, and there's not a lot of modern day circuit diagrams of the exact units you're buying that you could reverse-think about how it works. Resistors, inductors, transformers, operational amplifiers, swtiching MOSFETs, capacitors, etc. can be all arranged in different ways or "topologies". Once you learn more in-depth about the units, you may realize that it's just the same thing over and over again, and the only fun comes when there is a weird unit that is made. (Yes Sohoo, I'm talking about you) 

### Note before reading this, if you want to read an explaination on PSUs by Aris, one of the most respectable and knowledgable guru's out there. Read this https://www.tomshardware.com/reviews/power-supplies-101,4193-14.html. Otherwise, have fun.

### [ What To Look for In a Power Supply Pt. 1 - Wattage ]
First thing we are going to tackle is the easiest metric to look for in PSUs is wattage. It is what determines how much power it takes from the wall, converts it to DC, and then sends to the rest of the components. You definitely do not want to be right up against what it is continuously rated for. Sometimes you will trip OPP (over-power protection) and cause the power supply to shut down. If the PSU is not good quality, you can get far worse problems than just shutting down. 

Power supplies are rated in continuous operation (as they should be). Never get a power supply only rated in peak wattage (there are standards for what it means to continuously output power)--which means they can technically draw more power from the wall than what the box says--you want to get more wattage. And what happens if you upgrade to more power hungry parts down the road or want to overclock? Don't skimp out on wattage.

So, you want more wattage than what you system actually needs bare-minimum, but how can I find how much I need? Some people point to the estimated wattage on PCPartPicker or some calculator on the internet. Don't do this. Firstly, the parts on PCPP are rated by their TDP (Thermal Design Power), which is not actually the rating of power consumption. While the number can be in a ballpark of what the component actually use, it is not tied to wattage at all.  Secondly, those calculators are just going to estimate based on a preset range of values, and a lot of them don't disclose where they get those numbers from. My recommendation on where to find what power supply wattage you need is to look up third-party reviewers [Igorlab, Gamernexus, Hardware Unboxed (Techspot), Tom's Hardware, Techpowerup, AnandTech] and see the power consumption of the CPU and GPU under max load, factor in overclocking power draw, the power for your storage devices, and then add a little more overhead to that number.
⠀
So you know how many watts your power supply should be rated for minimum, but how much is too much and does that matter? And to answer that latter question, yes it does. Not only does it cost more for more wattage, it actually makes your system less efficient (though, not by much if you're not doubling the wattage). Looking at a power vs efficiency graph, we see that at very low load, the system is not that efficient and then rapidly scales up to near peak efficiency around 35-60% (It's a wide range) use and then slowly trails off as we increase the load. This is disregarding very specific models (like the Cooler Master MWE Bronze and the Corsair RM (Non -x) lineup)) where the controller switches to a different modulation mode to boost efficiency that while it does boost efficiency at the higher loads (Burst operation, absolutely plummets your efficiency during idle or low loads. 

### [ What To Look for In a Power Supply Pt. 2 - Efficiency ]
Let's get this out of the way first...POWER SUPPLY EFFICIENCY IS NOT "CAUSATED" TO QUALITY OF A PARTICULAR UNIT. Sorry for that. Many people are under the misguided belief that a gold efficiency rated unit is better than a bronze unit straight up; they couldn't be anything further than the truth. There are platinum rated units that are garbage, and there are bronze units that are really decent.

Coincidentally though, you shouldn't be really looking to anything under 80+ (White) efficiency. Some may say that higher efficiency power supplies are usually better quality. In some regards, they can be right, but the more pressing trend is the swell of bad quality units out there. You can buy readily available Gold rated PSUs that have positive reviews on distributors sites that shouldn't be bought because of the glaring issues. It is safer to never assume anything based on efficiency, and just go by model. If it just so happens that you (or someone that helped you) find a good quality unit, and it is higher efficiency, awesome. Depending on your electric bill costs per month, you might even save money over the long term. A higher efficiency unit may also be worthwhile if the cost per month is so high that you will gain money back over the long run, but like I said before, efficiency does not equate quality. We still have to look for a good quality unit.

### [ What To Look for in a Power Supply Pt. 3 - Modularity ]
Modularity in a power supply refers to how the connections or cables are or aren't secured to a power supply. Simply putting, we have three types of modularity: none, semi, and fully-modular. Like how efficiency wasn't that much relevant, modularity isn't either. There are good and bad modular power supplies. Modularity only refers the cables, not the quality. Here is an infographic on it if you prefer visuals: https://i.imgur.com/F5i8APH.png

In a non-modular power supply, every cable is permanently connected to the power supply. This is often the cheapest option, but it could make harder to cable manage because you have every single cable with often time splitters on some cables.
In a semi-modular power supply, the main power cables (24 motherboard pin, 8 pin CPU EPS, 8 pin PCIE cable, etc.) are attached permanently while peripherals, SATA, molex aren't attached and can be found in the box.
In a fully-modular power supply, every cable is not attached and can be found in the bag or box. This is usually more expensive than the other two methods, is easier to cable management, and plays nice if you ever decide to get custom sleeved cables or want to change cable lengths.

All in all, there is no performance or quality difference if it is modular or not--it is just how the cables are connected.

### [ What to Look for in a Power Supply Pt. 4 - Protections ]
Protections in a power supply well are...protections. In the case of failure, or a transient event, or something abnormal, it is the presence and proper setting of this features that both protect the power supply and the rest of the system. It is a good idea to have all the protections and have them all properly set. Each unit is different and without careful analysis, you won't know that even though a unit like while the EVGA G3 is a good unit, its protections are set very very poorly to the point where it can melt. You can read about the different types of protections here: https://linustechtips.com/topic/1154199-psu-protections-what-do-they-help-against-and-how-do-they-work

For my own list, everything single protection is required on a higher budget (and set properly!), but on lower budgets, OCP on the 12v main rail can be dismissed if the unit has a good enough set OPP. However, base line is usually all the protections set correctly.

### [ What to Look for in a Power Supply Pt. 5 - Topologies ] 

There's 3 commonly seen topologies in modern day psu's. Double Forward, Active Clamp Reset Forward (ACRF) and LLC Resonant.

Double forward or two switch forward is a single forward configuration with 2, rather than 1 MOSFET to keep the core from running into saturation. Upsides of this is that it's cheap, but that's about it. The downsides are it only scaling up to 750w, the fact it's not meant for high efficiency PSUs, as it generally only goes up to 80+ bronze and due to hard switching the unit is more likely to whine. Worse transient response compared to LLC.

Active Clamp Reset Forward (ACRF) is a topology close to Double forward, but unlike Double forward is able to continue switching without load being applied, making it more efficient, but still use hard switching. mostly produced by FSP and seen in units like the Pure Power 11 350w =>. Upsides to this is that it's cheaper than LLC. But unlike DF can scale up to 1000w. Downsides are that it is more expensive than DF, efficient enough only to meet 80+ gold, due to hard switching likely to whine, but less than Double forward and mediocre design cause worse transient response.

LLC stands for L (inductor), L (transformer primary which is an inductor, too) and C (capacitor). There are two inductors (LL) and a capacitor (C) used which form a resonant circuit . It's made out of 5 parts, in case of a Half-bridge (two switching FETs, transformer, inductor and capacitor).  Upsides are that LLC is efficient enough to meet up to 80+ Titanium, low chance of whining unless the unit has very poor build quality, high scaling in wattage; up to 2000w+.

### [ What to Look for in a Power Supply Pt. 6 - Regulations ]

 [Group Regulation]

Luke Savenije went into group regulation and why it's a problem before in the link below. It uses two coils, a big and a smaller one. The big one will regulate 12v and 5v, while the smaller one will regulate 3.3v. Thus, because the controller tracks both 12v and 5v rails as a whole, in crossload situations (if the load on one of them is high, while the other is low) voltages can go out of nominal (5% tolerance by ATX specifications). Specifically, This is common situation with modern PCs that, first, support C6/C7 sleep states, in which 5V rail get almost no load while 12V rail still loads relatively high, and second, modern PCs generally don’t load 5V rail much even when not in standby, because the only hardware that still uses it are HDDs and SATA SSDs, while 12V rail can be loaded very high, especially with high-end GPUs. This is especially troublesome with fast peaks of modern GPUs, which switch between 50 and 450 Watt multiple times per second. If the output capacitors can not buffer that (particularly in older units), the main regulator has to follow those peaks - altering also the 5 Volt output voltage with it. This leads to strong 5 Volt fluctuations even if there is little load on the rail.
The only upside is that it's cheap to produce, but on the other hand voltage can easily get out of spec due to regulating 12v and 5v together, generally doesn't meet c6/c7 sleep states or can’t keep voltages in specs in crossload situations associated with them, not recommended for anything beyond an APU system .

![afbeelding](https://user-images.githubusercontent.com/76516169/115608984-42651d00-a2e7-11eb-92ac-5f742e6f4c05.png)

https://linustechtips.com/main/topic/1122694-why-group-regulated-units-shouldnt-be-boughtsold-in-2019-and-on/

[Double Mag Amp]

Double mag amp is one of the two ways of an "independent" regulation, in this case regulated from the secondary winding, using an inductor to step down the current to either 5v or 3.3v. This is relatively uncommon with the introduction of DC-DC, since this is less efficient. An example where this is still used would be Seasonic's S12iii. Also, it can not work with an unloaded output (luckily a situation that doesn’t occur in a normal PC). The picture below marks the 3 coils compared to two on group regulation, by which you can see it's a double mag amp (in this case the s12 based corsair TX 80+) 
Upsides are that it's cheap, and unlike GR is individually regulated. Downsides are it needing needs more load to work, hence generally not coming higher than 80+ bronze, low efficiency compared to DC-DC.

![afbeelding](https://user-images.githubusercontent.com/76516169/115609103-66286300-a2e7-11eb-9e7d-4dd11349145a.png)

[DC-DC]

DC-DC uses a similar, yet quite different technique to double mag amp. it does share that it uses independent regulation, but does it in a different way. It uses buck step-down converters to lower the voltage directly from 12v to 5v or 3.3v. This is more efficient, and needs less load to function. LLC PSUs can even work properly without any hardware attached on a rail, if necessary. On the picture below i marked a dc-dc converter, in this case on a Seasonic Focus PX.
Upsides are it also being individually regulated, very efficient, since it can function with less load, most common in modern (often higher quality) PSUs.
The only downside is it being expensive.

![afbeelding](https://user-images.githubusercontent.com/76516169/115609450-d505bc00-a2e7-11eb-8893-83735c563063.png)

[Conclusion]

In the most ideal situation you get a DC-DC unit with an LLC Resonant Converter, but due to budget this might not always be possible.
APU system: preferably DC-DC, any topology
Low-end gaming system: DC-DC, ACRF or LLC
midrange-high end gaming system: DC-DC with LLC 

[Sources:]
https://www.ti.com/seclit/ml/slup263/slup263.pdf
https://www.vishay.com/docs/91616/twoswitch.pdf
https://www.tomshardware.com/reviews/power-supplies-101,4193-14.html
https://www.techpowerup.com/articles/overclocking/psu/160/5
https://www.anandtech.com/show/2450/3
https://www.relaxedtech.com/reviews/seasonic/focus-plus-ssr-850px/1
http://www.ti.com/lit/ml/slup129/slup129.pdf
https://linustechtips.com/main/topic/1122694-why-group-regulated-units-shouldnt-be-boughtsold-in-2019-and-on/
https://linustechtips.com/topic/1158795-topologies-and-regulations-what-should-i-look-for/

### [ What to Look for in a Power Supply Pt. 7 - Rails ]
Rails are just how the traces or wires connecting to different connectors. Most units have a single 12v main rail and an extra one for standby power plus the minor rails. I am going to put this bluntly....Unless you pair and overclock a CPU to the max and max out the power delivery on a single PCIe cable, multi-rail power supplies are no way inferior to single rail (and if you did, well then, just add some extra PCIe cables). You can read more about it here: http://www.jonnyguru.com/forums/showthread.php?3990-Single-vs-Multiple-12V-rails-The-splitting-of-the-12V-rail

### [ What to Look for in a Power Supply Pt. 8 - Reviews ]
If you don't have an oscilloscope plus an absurd amount of testing equipment, you cannot fully determine if a power supply is good or not. You can take it apart and examine each component; however, you need to look at real world performance to see if the electrolytic capacitors and such really live up to what the box says. For example, even though the RMx is on the same platform as Revolution DF (CWT GPU), it is heavily modified to the point where it literally performs different.

On that note, DO NOT attempt to dissect, disassemble, or otherwise interact with the internal components of a power supply unless you REALLY know what you're doing. Power supplies have capacitors in them which are powerful enough to almost kill you, even when not powered (because they can hold electrons in idle). If you don't know how to safely discharge a power supply, do not even open it.

This brings us to how do you know if one is good, and you don't know what to look for or if you don't have enough time--reviews. The average consumer, like on PCPartPicker or Amazon, won't be looking at things like voltage ripple, inrush current, transient response, voltage regulation, thermals, quality of capacitors, viability of protections, and these are the things actually affect longevity and reliability. If we relied on Amazon reviews solely, we would be getting 970 Evos and Corsair fans. Don't gamble like this on power supplies.

It is of the uttermost importance that you gain knowledge from people who know what they are doing. Websites like Tom's Hardware (Aris from Cybernetics) and Jonnyguru have electrical engineers who spend each day with units, knowing how each one works inside and out. Those are the people you trust.

### [ What to Look for in a Power Supply Pt. 9 - Conclusion ]
Power supplies are very complex electrical systems with different avenues on to achieve the goal of changing the AC from the wall to the DC which feeds the system. This was a very shallow walk across what you should be looking for or rather avoiding in a power supply. You should have attained the basic enough knowledge to know when you definitely should not buy a power supply because of its bare or "interesting" internals, but there is so much more to learn. Look in  #specific-notes for more information.

### Extra Resources about the above topics
Anatomy of a Power Supply
- Very Simple Circuit Diagram: https://www.arrow.com/en/research-and-events/articles/switch-mode-power-supply
- PSU Anatomy 1: https://www.techspot.com/article/1967-anatomy-psu/
- A Power Supply Dissected: https://ithardware.pl/testyirecenzje/premierowy_test_silentiumpc_vero_l3_600_w_i_700_w_z_dc_dc_i_80_plus_bronze-11912.html
- PSU Anatomy 3: http://www.realhardtechx.com/index_archivos/SILVERSTONE_Essential_series_750W_4.html
- Function of PWM Controllers in a PSU: https://e2e.ti.com/blogs_/b/powerhouse/archive/2016/12/02/when-should-you-use-pwm-controllers

Architecture
- What topologies and regulation to look for: https://linustechtips.com/main/topic/1158795-topologies-and-regulations-what-should-i-look-for/
- Why Group Regulation is bad: https://linustechtips.com/main/topic/1122694-why-group-regulated-units-shouldnt-be-boughtsold-in-2019-and-on/
- Single vs Multirail: http://www.jongerow.com/multiple-12V-rails/index.html
- PSU Protections: https://linustechtips.com/topic/1154199-psu-protections-what-do-they-help-against-and-how-do-they-work

Outside notes
- Tierlist: https://linustechtips.com/main/topic/1116640-psucultists-psu-tier-list/
- Expanded LTT PSU spreadsheet (relating to tierlist): https://docs.google.com/spreadsheets/d/1eL0893Ramlwk6E3s3uSvH1_juom7SMG5SCNzP2Uov8w/edit#gid=1214219159

Guides
- Tom's Hardware Guide On Power Supplies: https://www.tomshardware.com/amp/reviews/power-supplies-101,4193-12.html
- TPU Guide on PSUs: https://www.techpowerup.com/articles//overclocking/psu/160/1
- Mini Glossary of PSU Terms (some unit specifications too, but are outdated): https://docs.google.com/spreadsheets/d/1_GMev0EwK37J3zZL98zIqF-OSBuHlFEHmrc_SPuYsjs/edit#gid=802417661

### Cool Stuff
Circruit Diagram of an actual ATX unit: https://youtu.be/nhhWyg8QKiI
Why PSUs die: https://www.google.com/amp/s/www.tomshardware.com/amp/news/why-power-supplies-fail-psus,36712.html
How Voltage Regulation Works: https://www.meanwelldirect.co.uk/glossary/what-is-load-regulation/
﻿
## Fans by realcelicahours

Basic terminology:

Bearing - the component of the fan responsible for reducing friction of the rotor shaft and with that, the blades. this is more often than not the part of a fan that dies first.

DBB - dual ball bearing. the most reliable fan bearing available, due to its usage of ball bearings to reduce friction of the rotor shaft, instead of relying on fluid as a means of stabilization which can leak or evaporate. has no issue with high start/stop cycles or temperature tolerance due to lubricant not being crucial to operation

HDB - also known as Hydro (dynamic) bearing or Hydraulic bearing. my usage of this is a catch-all term for bearings inbetween sleeve and true FDB (patented design by Matsushita). depending on the fan, HDBs can vary highly in groove count, lubricant viscosity and sealing. generally very mediocre in reliability, and doesn't do well for high start/stop cycles. temperature tolerance isnt great due to fluid evaporation. Often times is cheap and only recommended for very low budgets, which rarely applies because there are $7 fans with DBB.

FDB - the best fluid bearing that isn't magnetically stabilized. the original design is patented by Matsushita, and uses grooving on the inside of the bearing to allow fluid to circulate. quite reliable, but still not great for high start/stop cycles. temperature tolerance isnt great due to fluid evaporation.

Twister - Enermax's in house bearing that uses magnetic stabilization alongside a fluid based bearing to improve bearing reliability. quite effective and they reach high MTTF for what they are (100K and 160K at 25 degrees Celcius for G1 and G2 respectively)

MTTF - a rough estimate of fan lifespan by the manufacturer, measured at a given temperature (usually 25 degrees Celcius for fluid based bearings) and humidity. an L50 calculation, in that atleast 50% of bearings would survive the MTTF number in hours or more, and the remaining 50% would be dead.

RPM Response - Rpm response just refers to the range of control you can get from a fan by the pwm duty cycle from 0-100% and then what rpm corresponds to a duty cycle percentage based on testing multiple samples and averaging them.
Aka where one fan will start being controllable at 0% duty cycle like Uctba12p, other fans might start at 20-40% for their control depending on the fan.
They might also have a sudden spike in rotational speed when you increase duty cycle meaning the response curve will be less linear.
For rpm response you would basically ideally have a completely linear curve from 0% duty cycle to 100% with rpm increasing equally for each duty cycle interval.
Funnily enough Uctba12p gets very close to this.
From multiple samples of testing I observed that apart from a very slight spike at the lowest and highest end of the curve they are mostly entirely linear throughout the rest of the curve.
They also control from 0% duty cycle onwards so you can get extremely precise control for the rpm range it has.

A duty cycle or power cycle is the fraction of one period in which a signal or system is active. Duty cycle is commonly expressed as a percentage or a ratio. A period is the time it takes for a signal to complete an on-and-off cycle. 


Recommendations, best options in a category marked with a star, bearing type noted  next to name
=<15USD RGB

*Gelid Stella (DBB)
https://gelidstore.com/collections/thermal-solutions/products/stella
notes: the best budget rgb fan available with the best bearing, 24 LED dual ring daisychained ARGB and decent performance. keep shipping costs and time in mind as they ship from HK.

~50-60USD RGB 3 pack: 
*Enermax SquA RGB (G1 Twister)
notes: top tier noise normalized performance, full accessory set w/ button controller or mobo sync, standardized connectors 

=<15USD non RGB fan: 
*Arctic P12 PWM PST CO (DBB)
notes: great bearing reliability with a 10 year warranty, PWM daisychainable, decent noise normalized performance and static pressure

Arctic P14 PWM PST CO (DBB)
notes: see P12 PWM PST CO

*Aerocool Dead Silence 140mm Black (FDB)
notes: one of the best low noise fans you can buy, 3pin so voltage control only yet it has great RPM response 

Cougar Vortex (FDB)
notes: marketed as an HDB but is actually a true FDB with decent performance

*Enermax T.B. Silence Adv 120 (Twister G2) 
https://www.ebay.com/itm/Enermax-T-B-SILENCE-ADV-120mm-Ultra-Silent-Design-Case-Fan-3-Pack-Open-Box/392941865051? 
(open box unit, only real place to get it at a reasonable price in the US) 
notes: great fan that beats SilentWings3 in noise to airflow

Enermax T.B. Silence Adv 140 (Twister G2) https://www.ebay.com/itm/Enermax-T-B-SILENCE-ADV-140mm-Ultra-Silent-Design-Case-Fan-3-Pack/392805755229? notes: see T.B. Silence Adv 120
UK:

<15GBP RGB fan

*Gelid Stella (DBB)
https://gelidstore.com/collections/thermal-solutions/products/stella
notes: the best budget rgb fan available with the best bearing, 24 LED dual ring daisychained ARGB and decent performance. keep shipping costs and time in mind as they ship from HK.

50-60GBP RGB multipacks

*Enermax SquA RGB (Twister G1)
notes: top tier noise normalized performance, full accessory set w/ button controller or mobo sync, standardized connectors 

Alpenföhn Wing Boost 3 High Speed Black/White
(FDB)
notes: worse noise normalized performance than squa, modest fan profile recommended due to high speed range

Alpenföhn Wing Boost 3 Black/White (FDB)
notes: worse noise normalized performance than squa, 600RPM max cutdown from High Speed, no modest fan profile needed for most people

=<15GBP non RGB fan

*Arctic P12 PWM PST CO (DBB)
notes: great bearing reliability with a 10 year warranty, PWM daisychainable, decent noise normalized performance and static pressure

Arctic P14 PWM PST CO (DBB)
notes: see P12 PWM PST CO

Arctic F12 PWM PST CO (DBB) 
notes: worse noise normalized performance than P, still PWM daisychainable with great reliability

Arctic F14 PWM PST CO (DBB)
notes: see F12 PWM PST CO

NoiseBlocker BlackSilentPro P-2 (NB-NanoSLI 2)
notes: 3pin, so only voltage control, but good performance at low noise levels
AU: (WIP)

=<15AUD non RGB fan

*Scythe Slip Stream 1200RPM DB (DBB)
notes: beats anything else at the price point with amazing bearing reliability and top tier noise normalized performance, 3pin so only voltage control

=<20AUD RGB fan

*Gelid Stella (DBB)
https://gelidstore.com/collections/thermal-solutions/products/stella
notes: the best budget rgb fan available with the best bearing, 24 LED dual ring daisychainable ARGB and decent performance. keep shipping costs and time in mind as they ship from HK.

Options to avoid

"Cheap" RGB fans from brands such as Asiahorse and UpHere - RGB fans like this use low lifespan HDBs which leave alot to be desired in long term reliability and are far from the best in noise normalized performance or bearing reliability or LED count for the price when Gelid Stella exists

Every Corsair RGB fan - Corsair's RGB fans are horribly priced and have mediocre at best performance. every fan besides ML120RGB has a 40K MTTF bearing, and ML120RGB exacerbates the pricing issue. they also use proprietary connectors which lock you into their rgb ecosystem with iCUE which
is never a good thing

Lian Li Uni Fan SL120 - the frame thickness on these fans impedes heavily on blade diameter leading to mediocre noise normalized performance at a price higher than SquA, which makes them a bad option

Noctua fans, besides A12x25 in highly specific scenarios (infinite budget+radiator) - Noctua's fans are greatly overrated by the general public, horribly priced (yes, that includes Redux) and their in-house oil pressure bearing loses in reliability to DBB on fans like Arctic CO (which also has 4 years more warranty)

In Win RGB fans - In Win's RGB fans have mediocre noise normalized performance but more importantly a sleeve bearing which is the lowest lifespan bearing with the lowest temperature tolerance due to lubricant evaporation

Enermax T.B. RGB - T.B. RGB has worse noise to airflow than SquA by a noticeable margin, and uses proprietary connectors for the SKUs that aren't T.B. RGB A.D.

Silent Wings 3 - SW3 is over twice the price of T.B. Silence Adv which performs better and has much better RPM response

Cooler Master MF120R - of all the major brand name RGB fans, this one's probably the worst. it has awful noise normalized performance and at a given noise level it will displace roughly half the air of an LL120 which is already a bad fan.
