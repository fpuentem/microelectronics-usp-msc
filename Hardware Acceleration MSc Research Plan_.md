

# **Exploring Pre-Compilers for Hardware Acceleration in Heterogeneous Computing Systems: A Foundational Guide for MSc Research**

### **1\. Introduction to Hardware Acceleration and Heterogeneous Computing**

Modern computing is undergoing a significant transformation, driven by the escalating demands for performance and energy efficiency that traditional general-purpose processors can no longer meet alone. This shift is particularly pronounced as the benefits derived from Moore's Law, which historically enabled exponential improvements in transistor density and processing power, begin to diminish.1 Consequently, there is a growing imperative to explore alternative architectural paradigms, with hardware acceleration and heterogeneous computing emerging as pivotal solutions. This report provides a foundational understanding of these concepts, their synergistic relationship, and the critical role of specialized compilers in unlocking their full potential, offering a structured learning path for an MSc research endeavor.

#### **1.1. Defining Hardware Acceleration: Principles and Benefits**

Hardware acceleration involves the strategic utilization of specialized computer hardware to execute particular functions with greater efficiency than software running on a general-purpose Central Processing Unit (CPU).2 The fundamental principle underpinning this approach is the computational equivalence of hardware and software; any data transformation computable by software on a generic CPU can also be realized through custom-designed hardware or a combination of both.2 The decision to employ hardware acceleration is typically driven by a careful evaluation of trade-offs, including desired latency, throughput, and energy consumption.2

A primary advantage of hardware acceleration is the substantial increase in processing speed, often accompanied by reduced power consumption and lower latency.2 This is achieved by offloading computation-intensive algorithms to dedicated hardware units that are optimized for specific tasks. For instance, a cryptographic accelerator card can perform cryptographic operations at a significantly faster rate than a CPU.2 Furthermore, hardware accelerators facilitate greater parallelism and more efficient utilization of the functional components available on an integrated circuit.2 These benefits are particularly pronounced for functions that are fixed in nature and do not require frequent updates, as the design is etched onto silicon, making post-fabrication modifications costly.2

In contrast, software-based computing, predominantly reliant on the von Neumann architecture, faces inherent limitations. Processors adhering to this architecture must sequentially fetch and decode instructions, and load data operands from memory, leading to what is known as the "von Neumann bottleneck".2 This fundamental limitation constrains the throughput of software, even in systems with separate instruction and data caches (modified Harvard architecture), due to the overhead of instruction decoding and multiplexing execution units.2 Hardware accelerators, by contrast, improve execution by allowing greater concurrency, employing specific datapaths for temporary variables, and significantly reducing the overhead associated with the instruction fetch-decode-execute cycle.2 This architectural evolution, driven by the increasing demand for processing vast amounts of data in fields such as AI, machine learning, and high-performance computing, is not merely an optimization but a necessary step to overcome inherent CPU limitations and achieve superior performance and energy efficiency.

#### **1.2. Understanding Heterogeneous Computing: Architectures and Significance**

Heterogeneous computing describes systems that integrate multiple types of computing cores or processors, such as CPUs, Graphics Processing Units (GPUs), Application-Specific Integrated Circuits (ASICs), Field-Programmable Gate Arrays (FPGAs), and Neural Processing Units (NPUs).3 This architectural diversity allows for the assignment of different workloads to processors specifically designed for those tasks, thereby optimizing both overall performance and energy efficiency.3 A common and early example of heterogeneous computing is the combination of CPU cores and a GPU, widely used in gaming and other graphics-intensive applications.3

The significance of heterogeneous computing is underscored by its crucial role in handling modern, data-intensive workloads. Fields such as Artificial Intelligence (AI) and Machine Learning (ML), High-Performance Computing (HPC), and large-scale data centers rely heavily on these architectures to process immense volumes of data efficiently.3 By enabling a single system to comprise multiple computing subsystems that work in parallel, heterogeneous computing accelerates computation speed, minimizes task completion times, and enhances energy efficiency, offering a flexible and scalable approach to complex computational problems.3

However, the advantages of heterogeneous computing introduce considerable engineering challenges. Integrating different processor types into a cohesive system demands advanced design methodologies to ensure compatibility and efficient communication.4 Furthermore, software development for these complex systems is inherently more intricate, often necessitating specialized tools and expertise to effectively manage diverse processing elements, allocate resources, and schedule tasks.4 This direct relationship between architectural advancements in heterogeneous hardware and the increased complexity of software tooling highlights a critical area of focus: unlocking the full potential of these systems requires parallel advancements in compiler technology and programming models.

To further illustrate the distinct roles of various components in a heterogeneous system, Table 1 provides a comparative overview.

**Table 1: Comparison of Heterogeneous Computing Components**

| Component Type | Primary Role/Strength | Key Characteristics | Typical Use Cases |
| :---- | :---- | :---- | :---- |
| **CPU** | General-purpose processing, sequential tasks | Versatile, complex control logic, moderate parallelism | Operating systems, general applications, control |
| **GPU** | Massively parallel processing, floating-point ops | High throughput, many simple cores, SIMD | Graphics rendering, AI/ML training, scientific simulation |
| **FPGA** | Reconfigurable logic, custom data paths | Flexible, programmable post-manufacturing, medium-high performance | Prototyping, custom accelerators, signal processing |
| **ASIC** | Fixed-function, highest performance/efficiency | Optimized for specific task, high NRE costs, fixed | High-volume production, specialized accelerators (e.g., AI inference chips) |
| **NPU** | Neural network specific computations | Optimized for AI workloads (e.g., matrix multiplication, convolutions) | AI inference on edge devices, dedicated ML acceleration |

This table clarifies the unique contributions of each processing unit, demonstrating why their combination in heterogeneous systems is essential for efficiently handling diverse workloads. Understanding these distinctions is crucial for comprehending how compilers map different parts of an application to the most suitable hardware component.

#### **1.3. The Convergence: Why Hardware Acceleration in Heterogeneous Systems?**

The convergence of hardware acceleration within heterogeneous systems is a direct response to the "end of Moore's Law" and the escalating demand for energy-efficient, high-performance computing.1 As the traditional scaling of general-purpose CPUs slows, the industry can no longer rely solely on improvements in clock speed or transistor density for performance gains. This situation has necessitated a strategic shift towards specialization and integration.

By combining general-purpose CPUs with specialized accelerators like GPUs, FPGAs, and ASICs, heterogeneous systems can optimally execute a wide array of computational tasks, ranging from simple data processing to highly complex AI algorithms.4 This approach seeks to bridge the gap between the versatility offered by software and the superior efficiency inherent in custom hardware. The result is a flexible yet highly optimized solution that can adapt to evolving computational needs while maintaining stringent performance and power budgets. This strategic imperative for specialization means that tools like "pre-compilers" are not merely technical conveniences; they are strategic enablers for this new era of computing, facilitating the efficient deployment of specialized hardware without incurring prohibitive development costs.

### **2\. Custom Silicon and Specialized Hardware Architectures**

The advisor's mention of a "bespoke silicone group" points directly to the growing trend of designing custom hardware solutions. This section explores the concept of custom silicon and the distinct roles of FPGAs and ASICs in its development.

#### **2.1. Bespoke Silicon: Definition, Advantages, and Applications**

"Bespoke silicon," also known as custom silicon, refers to Integrated Circuits (ICs) that are meticulously designed for a specific application or a particular customer.6 This contrasts sharply with off-the-shelf, general-purpose silicon, which is mass-produced for a broad range of applications with limited configuration options.6 Custom silicon is engineered to precisely meet unique performance, power consumption, and feature requirements, allowing for deep customization of aspects such as I/O capabilities, memory interfaces, and the integration of workload-specific accelerators.6

The advantages of employing custom silicon are substantial. Companies gain a significant competitive edge by achieving higher performance, lower power consumption, superior feature integration, and enhanced security compared to designs reliant on general-purpose components.6 Notable examples include Amazon's AWS Graviton processors, which are custom silicon optimized for cloud computing with enhanced memory encryption and power efficiency, and AWS Nitro DPUs, which incorporate custom silicon for more efficient handling of storage, networking, and security tasks.6

However, the design of custom silicon, particularly at advanced process nodes (e.g., 7nm or below), presents considerable challenges. These include managing power leakage, addressing complex thermal management issues, and overcoming manufacturing yield challenges.6 Successfully navigating these difficulties requires sophisticated design and optimization techniques. Custom silicon plays a critical role in industries that demand specialized features and high reliability, such as automotive systems (enabling real-time processing and redundancy for safety-critical functions), healthcare, and telecommunications (accelerating Radio Access Network functions for faster, more efficient data processing).6 The existence of "bespoke silicon groups" underscores an industry-wide pursuit of extreme optimization that off-the-shelf components simply cannot provide. This necessity for deep customization inherently implies a demand for highly specialized design flows and tools, where "pre-compilers" for hardware acceleration become indispensable by automating complex, high-cost design processes.

#### **2.2. The Role of FPGAs and ASICs in Custom Hardware Development**

Field-Programmable Gate Arrays (FPGAs) and Application-Specific Integrated Circuits (ASICs) represent two distinct but complementary approaches to creating custom hardware, each with its own advantages and trade-offs.

FPGAs are reconfigurable hardware devices that can be programmed or re-programmed after manufacturing.2 This inherent flexibility makes them ideal for rapid prototyping, design space exploration, and lower-volume applications where time-to-market is critical. They allow designers to quickly test and iterate on hardware designs without the lengthy and costly fabrication cycles associated with ASICs.

ASICs, on the other hand, are integrated circuits custom-designed from the ground up for a specific function.6 They offer the highest possible performance, lowest power consumption, and smallest physical area for high-volume production.6 However, these benefits come at the cost of high Non-Recurring Engineering (NRE) expenses and significantly longer development cycles due to the complexities of chip fabrication.2

Traditionally, the design flow for both FPGAs and ASICs involves describing the hardware at the Register-Transfer Level (RTL) using Hardware Description Languages (HDLs) such as VHDL and Verilog.2 The choice between FPGAs and ASICs depends on factors such as production volume, performance requirements, power budget, and development time. This spectrum of hardware customization, from flexible reconfigurability to fixed, highly optimized silicon, directly influences the type and sophistication of the "pre-compiler" tools required. For FPGAs, High-Level Synthesis (HLS) tools are paramount for rapid prototyping and efficient design space exploration.8 For ASICs, while HLS can still be utilized, the ultimate goal of achieving peak performance and minimal power consumption often necessitates more manual optimization or the use of highly specialized Domain-Specific Languages (DSLs).9 Understanding this continuum is vital for defining a research topic that aligns with specific hardware targets and their associated design methodologies.

### **3\. Compilers for Hardware Acceleration: The "Pre-Compiler" Paradigm**

This section delves into the pivotal role of compilers in hardware design, specifically focusing on High-Level Synthesis (HLS) and Domain-Specific Languages (DSLs), and contextualizing the concept of a "pre-compiler" within this framework.

#### **3.1. Compiler Fundamentals and Their Role in Hardware Design**

At its core, a compiler acts as a translator, converting high-level, human-readable programming code into low-level machine code that a computer's processor can execute.11 The efficiency of this translation is paramount for ensuring optimal performance when a given piece of code runs on a machine.11

In the realm of hardware design, the role of compilers extends beyond mere software translation. Historically, hardware development involved the laborious and error-prone manual translation of algorithms into Hardware Description Languages (HDLs) at the Register-Transfer Level (RTL).8 This process created a significant productivity bottleneck, with software developers typically producing 10-100 lines of code per day, while hardware designers achieved only hundreds of LUTs (or thousands of equivalent gates) per day, corresponding to dozens of lines of HDL code.10 This substantial productivity gap is a primary driver for innovation in compiler technology for hardware design.

Compilers now play a critical role in bridging the abstraction gap between high-level software algorithms and their hardware implementations. They aim to automate significant portions of this translation, making hardware design more accessible to a broader range of developers, including those with a software background.8 The concept of a "pre-compiler" emerges as a direct response to this economic and engineering challenge, leveraging software development paradigms—such as higher abstraction and automation—to accelerate the creation of hardware.

#### **3.2. High-Level Synthesis (HLS): Automating Hardware Generation from High-Level Code**

High-Level Synthesis (HLS), also known as C synthesis, electronic system-level (ESL) synthesis, algorithmic synthesis, or behavioral synthesis, is an automated design process that transforms abstract behavioral specifications into Register-Transfer Level (RTL) hardware designs.8 This process typically begins with high-level code, often written in synthesizable subsets of ANSI C, C++, SystemC, or MATLAB, where the algorithmic behavior is largely decoupled from low-level circuit mechanics like clock-level timing.8

The HLS process involves several key steps:

1. **Input Analysis and Constraints:** The high-level source code is analyzed, and architectural constraints are applied to guide the synthesis.8  
2. **Scheduling:** The algorithm is partitioned into "control steps," each representing a small section of the algorithm that can be executed within a single clock cycle in the resulting hardware. These control steps define the states in the finite-state machine of the hardware design.8  
3. **Allocation and Binding:** Instructions and variables from the algorithm are mapped to specific hardware components. This includes allocating functional units (e.g., ALUs), registers, multiplexers, and wires, and binding them to the data path.8  
4. **Output Generation:** The HLS tool generates an RTL design, typically in a Hardware Description Language (HDL) such as Verilog or VHDL. This RTL design is then passed to a logic synthesis tool to be converted into a gate-level netlist for implementation on an FPGA or ASIC.8

HLS tools empower designers to describe their designs at a higher level of abstraction, while the tool automatically handles the intricate details of the micro-architecture.8 This allows for efficient exploration of design trade-offs, such as achieving higher execution speed by utilizing more hardware resources (e.g., ALUs, registers, memories) or reducing hardware complexity by performing operations over more clock cycles.8 Directives, known as pragmas, can be inserted into the source code to guide the synthesis process, allowing designers to fine-tune the resulting hardware design.13

The Nios II C-to-Hardware Acceleration Compiler serves as a concrete example of a "pre-compiler" that embodies the principles of HLS.12 This tool automates the conversion of performance-critical ANSI C functions into hardware accelerators on FPGAs, dramatically reducing development time from weeks to minutes.12 It simplifies the process to profiling software code to identify bottlenecks, selecting a function, and then initiating acceleration with a simple command.12 This paradigm of automating hardware generation from higher-level software descriptions is precisely what the term "pre-compiler" refers to in this context.

#### **3.3. Domain-Specific Languages (DSLs): Tailoring Software for Hardware Optimization**

Domain-Specific Languages (DSLs) are programming languages meticulously crafted for a particular application domain.9 Unlike general-purpose languages, DSLs provide higher levels of abstraction that are natural to the domain expert, meticulously limiting the set of programming constructs to minimize errors and enabling a rich array of domain-specific optimizations and program transformations.10

The core objective of DSLs in hardware acceleration is to allow programmers to specify *what* they intend to compute, rather than dictating the precise *how* of its hardware implementation.10 This approach offers several significant benefits for hardware acceleration:

* **Abstraction of Hardware Details:** DSLs abstract away low-level hardware intricacies such as timing, clocking discipline, and complex memory management, thereby simplifying the programming process and reducing the potential for error.10  
* **Optimized Concurrency Exploitation:** Compilers for DSLs can automatically identify and exploit the inherent parallelism within a specific domain, translating it into spatial acceleration on FPGAs or other accelerators.10  
* **Specialized Operations:** DSLs facilitate the creation of specialized hardware operations tailored for domain-specific data types. This can lead to substantial performance and efficiency gains, enabling complex inner-loop functions to be executed in a single clock cycle.9  
* **Reduced Overhead:** By specializing the hardware through DSLs, the overhead associated with program interpretation is significantly reduced or eliminated.9

Halide, a C++-based DSL for high-performance image and array processing, exemplifies this approach by decoupling the definition of an algorithm from its scheduling logic.11 This separation of concerns allows programmers to experiment with different execution strategies to discover optimal implementations. The challenge of software and programming model complexity in heterogeneous computing is directly addressed by DSLs, which provide an intuitive, domain-native means to express computation while automating the complex mapping to diverse hardware architectures. This capability expands the accessibility of hardware acceleration to domain experts who may not possess deep hardware design knowledge.

#### **3.4. The "Pre-Compiler" in Practice: Examples and Functionality**

The term "pre-compiler" in the context of hardware acceleration refers to a specialized compiler or a component within a broader toolchain that prepares high-level software for efficient execution on dedicated hardware accelerators. This preparation often encompasses several critical functionalities:

* **High-Level Synthesis (HLS):** As previously discussed, a core function is the translation of high-level languages like C, C++, or SystemC into Register-Transfer Level (RTL) hardware descriptions.8 This automates the generation of the hardware itself from a software-like description.  
* **Domain-Specific Language (DSL) Compilers:** These compilers translate code written in DSLs into optimized hardware designs or specialized instructions for accelerators.10 This allows for highly efficient code generation tailored to specific application domains.  
* **Hardware-Software Partitioning:** A crucial aspect involves intelligently deciding which segments of an application are best suited for execution on the general-purpose CPU and which should be offloaded to specialized accelerators.5 This partitioning aims to maximize overall system performance and energy efficiency.  
* **Memory Management and Data Movement Optimization:** Efficient data transfer between the host CPU's memory and the accelerator's local memory is paramount, as data movement can often be a performance bottleneck.5 "Pre-compilers" implement techniques such as Direct Memory Access (DMA) or manage Shared Virtual Memory (SVM) to streamline these transfers.15  
* **Parallelization and Scheduling:** Identifying inherent parallelism within the source code and scheduling operations optimally across the available hardware resources is another key function.5 This involves mapping parallel constructs to the many cores of an accelerator or exploiting instruction-level parallelism.

A prime practical example is the Nios II C-to-Hardware Acceleration Compiler, which automates the conversion of performance-critical ANSI C functions into hardware accelerators on FPGAs.12 This tool significantly reduces development time, allowing developers to profile their code, identify bottleneck functions, and initiate hardware acceleration with a simple command.12 Historically, offloading software to hardware was a manual, expert-intensive task.12 The "pre-compiler" paradigm, exemplified by such tools, represents a fundamental shift towards automation. This automation is critical for scaling hardware acceleration beyond niche applications and making it accessible to a broader range of software developers, thereby accelerating innovation in heterogeneous systems.

To provide a clearer picture of various compiler technologies relevant to hardware acceleration, Table 2 outlines their primary functions and characteristics.

**Table 2: Compiler Technologies for Hardware Acceleration**

| Compiler Type/Tool | Primary Function/Role in Acceleration | Input Language(s) | Target Hardware/Output | Key Advantages/Features |
| :---- | :---- | :---- | :---- | :---- |
| **HLS Tools** | Automate RTL generation from high-level behavioral code | C/C++/SystemC/MATLAB | HDL (Verilog, VHDL) for FPGA/ASIC | Rapid prototyping, design space exploration, abstracts micro-architecture details 8 |
| **DSLs & Compilers** | Tailor software for specific hardware optimization | Domain-specific languages | Optimized hardware/accelerator instructions | Higher abstraction, domain-specific optimizations, simplified concurrency 10 |
| **LLVM** | Modular compiler infrastructure for optimization and code generation | Various (C, C++, MLIR) | Efficient machine code for diverse architectures | Common infrastructure for compilation stack, open-source, widely adopted 11 |
| **GCC** | General-purpose compiler collection | C, C++, Fortran, etc. | Executable code | Open-source, broad language support, foundational for many toolchains 11 |
| **NVCC** | NVIDIA CUDA Compiler | CUDA C/C++ | GPU machine code | Enables GPU acceleration, parallel thread execution backend 11 |
| **Nios II C2H Compiler** | Converts C functions to FPGA hardware accelerators | ANSI C | RTL for FPGA | Automates hardware creation, reduces development time, boosts performance 12 |
| **Halide** | DSL for image/array processing | C++-based DSL | Optimized code for CPUs/GPUs/DSPs | Decouples algorithm from scheduling, enables performance exploration 11 |
| **MLIR-based (e.g., hyper)** | Unified compilation framework for heterogeneous devices | Intermediate Representations | Device-specific executables | Abstracts data communication, task splitting/scheduling for diverse hardware 5 |

This table provides a structured overview of the diverse compiler landscape relevant to hardware acceleration, highlighting how different tools contribute to the overall pipeline and offering context for potential research avenues.

#### **3.5. Key Challenges in Compiler Design for Heterogeneous Hardware**

Designing compilers for heterogeneous hardware systems presents a unique set of complex challenges that demand innovative solutions. These challenges arise from the inherent diversity and intricate interactions within such systems.

One significant hurdle is the **complexity of diverse architectures**.14 Compilers must effectively manage various Instruction Set Architectures (ISAs), differing architectural widths (e.g., 64-bit host CPUs versus 32-bit accelerators), distinct memory organizations (e.g., hardware-managed coherent caches versus software-managed scratchpad memories), and varied addressing schemes (virtual memory versus physical addresses).14 This architectural heterogeneity complicates the compiler's task of generating optimal code for each component.

Another critical challenge lies in **task splitting and scheduling**.5 The compiler needs to intelligently partition an application into tasks and then efficiently schedule these tasks across different heterogeneous devices—CPUs, GPUs, FPGAs, and Coarse-Grained Reconfigurable Arrays (CGRAs)—to maximize performance and energy efficiency.5 This often involves solving complex optimization problems, some of which are NP-complete, as the optimal solution can vary significantly with each algorithm-hardware combination.14

**Data management and communication optimization** represent a third major challenge.5 Ensuring efficient data movement and maintaining data coherence between host processors and heterogeneous devices is crucial. This includes managing the intricacies of shared memory and minimizing data transfer overheads, which can often become the primary bottleneck in heterogeneous systems.5

**Performance portability** is also a key concern.14 The goal is to develop compiler strategies that allow code optimized for one heterogeneous platform to perform well on another without requiring extensive re-tuning or manual modifications. This is difficult given the unique characteristics of each hardware target.

Finally, **verification complexity** is a pervasive issue.14 The deep interdependencies between hardware components and software layers, especially when automated synthesis tools are involved, make debugging and validating heterogeneous systems notoriously difficult. A change in one accelerator architecture, for example, can necessitate re-validation of multiple compiler backends, toolchains, and runtime software stacks across all supported applications.14 The ability to parameterize many hardware components and propagate these parameters throughout the software stack adds another layer of complexity to verification.14

These profound challenges in designing and programming Heterogeneous Systems on Chip (HeSoCs) highlight that the compiler is not merely a translator but an orchestrator. It must intelligently manage resource allocation, scheduling, data movement, and even adapt to differing memory models and ISAs. This elevates the role of the "pre-compiler" from a simple code generator to a system-level optimization engine, making research in this area highly impactful for the future of computing.

### **4\. Deep Dive into Open-Source Heterogeneous Computing Platforms**

The advisor's notes specifically mentioned several open-source platforms: PULP, Celerity, Occamy, PULP-Hero, and PULPissimo. These platforms are at the forefront of heterogeneous computing research and development, providing invaluable environments for exploring hardware acceleration and compiler design. The integration of "Core Linux" within some of these platforms further underscores the full-stack nature of this domain.

#### **4.1. The PULP (Parallel Ultra Low Power) Platform Ecosystem**

The PULP (Parallel Ultra Low Power) platform is a prominent open-source initiative dedicated to designing energy-efficient, parallel computing systems.18 Built upon the open-source RISC-V Instruction Set Architecture (ISA), PULP encompasses a rich ecosystem of hardware and software components tailored for various parallel processing applications, including machine learning, signal processing, and Internet of Things (IoT).18 Its core principles revolve around maximizing energy efficiency, exploiting parallelism, and fostering open-source development under the Solderpad Hardware License.17

The PULP ecosystem is characterized by its diverse portfolio of RISC-V cores, each optimized for different performance and power profiles:

* **CV32E40P (formerly RI5CY):** A 4-stage, 32-bit core that includes Digital Signal Processing (DSP) extensions such as hardware loops, SIMD instructions, bit manipulation, and Multiply-Accumulate (MAC) operations. It is designed for energy-efficient signal processing.16  
* **CVA6 (formerly Ariane):** A 6-stage, 64-bit CPU that fully supports the user-level and privileged RISC-V ISA specifications, enabling full Linux support. It is designed for high-frequency operation and serves as a general-purpose host processor in more complex PULP systems.17  
* **Ibex (formerly Zero-riscy):** A compact, 2-stage, 32-bit core optimized for ultra-low-power and ultra-low-area constraints, making it suitable for embedded microcontrollers.16  
* **Snitch:** A small, super-efficient, in-order, 32-bit RISC-V integer core, notably utilized in the Occamy project, often paired with powerful Floating-Point Units (FPUs).17

The PULP platform, with its wide array of RISC-V cores—ranging from the ultra-low-power Ibex to the Linux-capable CVA6—and its strong emphasis on energy efficiency and parallelism, serves as a comprehensive example of heterogeneous system design. Its modularity and open-source nature make it an ideal testbed for research into how "pre-compilers" can effectively target and optimize for such a varied set of hardware components.

#### **4.2. PULP-Hero: An Open Heterogeneous Research Platform for Transparent Acceleration**

PULP-Hero (HERO) stands as an open heterogeneous research platform designed to simplify the programming of complex heterogeneous systems.15 Its architecture ingeniously combines a PULP-based open-source parallel manycore accelerator, implemented on an FPGA and built around RISC-V cores, with a hard ARM Cortex-A multicore host processor running a full-stack Linux operating system.15 This unique combination makes HERO the first heterogeneous system architecture to integrate a powerful ARM multicore host with a highly parallel and scalable manycore RISC-V accelerator.15

A core design principle of HERO is to enable transparent accelerator programming through the OpenMP v4.5 Accelerator Model and Shared Virtual Memory (SVM).15 This means that programmers can write a single application source file for the host and use OpenMP directives to offload parallel sections to the accelerator, abstracting away lower-level complexities such as differing Instruction Set Architectures (ISAs) and memory organization.15 The platform's comprehensive software stack includes a heterogeneous toolchain (based on GCC 7), a kernel-level Linux driver, runtime libraries, and open-source hardware IPs to manage these intricate details.15

The HERO platform is organized into two main repositories: the bigPULP hardware repository, which contains the accelerator-side system-level architecture and the IOMMU for SVM, and the HERO SDK repository, which provides the toolchain, Linux driver, runtime libraries, and example applications.15 A typical base configuration for the Xilinx Zynq ZC706 Evaluation Kit features a PULP cluster with 8 RI5CY cores, dedicated L1/L2 scratchpad memory, and an IOMMU to facilitate SVM.15 HERO's explicit goal of "transparent accelerator programming" using OpenMP and SVM directly addresses the challenge of software and programming model complexity in heterogeneous systems.4 This makes HERO an excellent case study for "pre-compiler" research, as it demonstrates how a compiler and runtime system can abstract hardware intricacies, allowing programmers to focus on parallelization rather than low-level hardware details.

#### **4.3. PULPissimo: A Microcontroller for System-on-Chip Control**

PULPissimo represents a single-core microcontroller architecture within the broader PULP platform, serving as the primary System-on-Chip (SoC) controller for more recent multi-core PULP chips.16 While a single-core system, it signifies a substantial advancement in terms of completeness and complexity compared to its predecessor, PULPino.16 PULPissimo is designed to manage autonomous I/O operations, advanced data pre-processing, and external interrupts within the larger SoC.16

The architecture of PULPissimo is notable for several key features:

* **Main Core Options:** It can be configured with either the RI5CY core (a 4-stage, 32-bit core with DSP extensions) or the Ibex core (a 2-stage, 32-bit core optimized for ultra-low-power and area constraints).16  
* **Autonomous I/O Subsystem (uDMA):** PULPissimo incorporates an efficient micro-DMA (uDMA) that communicates autonomously with peripherals, offloading data transfer tasks from the main core.16  
* **Hardware Processing Engines (HWPEs):** A significant feature is its support for integrating custom hardware accelerators, known as Hardware Processing Engines (HWPEs).16 These HWPEs, which can be programmed on the memory map, are designed for tasks like neural network acceleration on edge computing devices and share memory with the main core.16  
* **Peripherals:** It includes a new memory subsystem, a simplified interrupt controller, and support for various I/O interfaces such as SPI, I2S, Camera Interface (CPI), I2C, UART, Hyperbus, and JTAG.16

PULPissimo demonstrates that hardware acceleration is not exclusive to large-scale High-Performance Computing systems; it is equally critical for ultra-low-power microcontrollers.16 The inclusion of features like uDMA and HWPEs highlights the importance of specialized data movement and processing at a fine-grained level, even in resource-constrained environments. This implies that "pre-compilers" targeting such systems must excel at identifying small, critical code sections for hardware offloading and managing efficient, low-power data flow, rather than solely focusing on large kernel offloads.

#### **4.4. Occamy: A Many-Core Processor for High-Performance Computing**

Occamy is a cutting-edge research prototype designed as a 2.5D integrated dual-chiplet system, pushing the boundaries of high-performance heterogeneous computing.17 It features an impressive 432+2 RISC-V cores, with a primary focus on ultra-efficient (mini-)floating-point computation, achieving peak system performances of up to 6.144 TFlop/s in 8-bit floating-point formats.17

The core design of Occamy combines the small, super-efficient, in-order, 32-bit RISC-V Snitch integer core with a powerful multi-precision Floating-Point Unit (FPU).17 This FPU is enhanced with Single Instruction Multiple Data (SIMD) capabilities, supporting a wide range of floating-point formats from FP64 down to FP8.17 To achieve exceptional efficiency on data-parallel floating-point workloads, Occamy leverages custom architectural extensions, including Stream Semantic Registers (SSRs) and FP Repetition (FREP) instructions, which enable the Snitch core to achieve FPU utilization exceeding 90% for compute-bound kernels.17

Each chiplet within the Occamy system contains over 216 Snitch cores, organized into groups of four compute clusters.17 Each cluster shares a tightly-coupled memory among eight compute cores and a dedicated DMA-enhanced core that orchestrates data flow.17 An AXI-based wide, multi-stage interconnect and dedicated DMA engines manage the substantial on-chip bandwidth.17 A Linux-capable 64-bit RISC-V CVA6 core manages all compute clusters and system peripherals.17 Each chiplet also includes private 16GB High-Bandwidth Memory (HBM2e) and can communicate with neighboring chiplets via a high-bandwidth die-to-die DDR link.17 The main chiplet architecture is open-source under the Solderpad Hardware License, with software released under the Apache-2.0 license.17

Occamy represents the bleeding edge of heterogeneous system design, leveraging advanced packaging, specialized FPUs, and custom ISA extensions. This level of specialization presents significant challenges and opportunities for "pre-compilers." Research could focus on automatically generating code that fully exploits these custom ISA extensions and complex memory hierarchies, or on managing task distribution across multiple chiplets for optimal performance, thereby directly impacting the realization of next-generation hardware.

#### **4.5. Celerity: An Accelerator-Centric RISC-V Tiered Fabric SoC**

Celerity is an open-source, accelerator-centric System-on-Chip (SoC) that employs a tiered accelerator fabric to enhance energy efficiency, particularly for high-performance embedded systems.23 Designed in TSMC 16nm technology, this 5x5 mm2 chip holds a world record for RISC-V performance, achieving 500 billion RISC-V instructions per second.23

The SoC structure of Celerity is strategically divided into three distinct tiers:

1. **General Purpose Tier:** This tier comprises several fully featured RISC-V processors, which are modified versions of the Berkeley Rocket core. These processors are capable of running general-purpose software, including a full operating system.23  
2. **Massive Parallel Tier:** This tier features a manycore architecture with hundreds of lightweight RISC-V processors, supported by a distributed shared memory system and a mesh-based interconnect.23  
3. **Specialization Tier:** Dedicated to application-specific accelerators, this tier's components can potentially be generated using High-Level Synthesis (HLS).23

Celerity's explicitly tiered architecture represents a sophisticated approach to heterogeneous computing. This design fundamentally demands an intelligent "pre-compiler" capable of performing fine-grained hardware-software partitioning and task mapping across these distinct tiers. The direct mention of HLS for the specialization tier aligns perfectly with the interest in "pre-compilers." Research could explore adaptive runtime techniques or compiler optimizations that dynamically allocate workloads to the most suitable tier for maximum energy efficiency and performance, thereby leveraging Celerity as a platform for advanced compiler development.

#### **4.6. Core Linux for Embedded and Custom Hardware Systems**

"Core Linux" refers to a Linux kernel specifically tailored for embedded systems and custom hardware platforms.25 While built upon the standard Linux kernel, it includes additional packages and configurations necessary to meet the stringent constraints of embedded environments, such as higher reliability, enhanced security requirements, tighter resource availability (memory, storage, CPU power), and specific peripheral availability.25

The importance of Core Linux for custom hardware systems is paramount. Platforms like PULP-Hero, which runs a full-stack Linux 15, or Celerity's general-purpose tier, which is designed to run an operating system 23, rely on a customized Linux kernel to function effectively.15 Building a custom Linux kernel for an embedded system offers several advantages:

* **Performance Optimization:** Unused drivers and modules can be removed, significantly improving system performance, reducing boot times, and optimizing resource usage.26  
* **Reduced Resource Consumption:** A stripped-down kernel minimizes memory usage and conserves valuable system resources by eliminating unnecessary features like unused file systems or debugging symbols, which is crucial for devices with limited computational power or battery life.26  
* **Enhanced Security:** Disabling vulnerable or unnecessary kernel features reduces the attack surface, enhancing security in critical applications like automotive or medical devices.26  
* **Hardware Compatibility:** A custom kernel ensures full compatibility with specialized hardware components, including the System-on-Chip (SoC), peripherals, and sensors.26

The development of Core Linux for custom hardware often involves cross-compilation (compiling code for an architecture different from the development host) and Board Support Package (BSP) development, which entails creating hardware-specific drivers and routines that enable Linux to operate in a particular hardware environment.25

Core Linux is not merely an operating system; it is a critical enabler for the entire software stack on complex heterogeneous platforms. The necessity for deep customization and cross-compilation of Linux highlights the intricate interplay between software and hardware design. A "pre-compiler" must not only target the accelerators but also integrate seamlessly with the host operating system and its specific configurations, ensuring a complete and functional system. This emphasizes the full-stack nature of potential research in this domain.

To provide a comprehensive overview of the key open-source heterogeneous platforms discussed, Table 3 offers a comparative summary.

**Table 3: Overview of Key Open-Source Heterogeneous Platforms**

| Platform Name | Primary Focus/Goal | Key Processor(s)/Cores | Architectural Highlights | Target Application/Domain | Open-Source Status/License |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **PULP-Hero** | Transparent heterogeneous acceleration | ARM Cortex-A (host), RISC-V (RI5CY) manycore accelerator | FPGA-based, OpenMP 4.5, Shared Virtual Memory (SVM) 15 | General-purpose heterogeneous computing research | Open-source (GCC, Apache-2.0) 15 |
| **PULPissimo** | Single-core microcontroller, SoC control | RISC-V (RI5CY, Ibex) | Autonomous I/O (uDMA), HWPE support, new memory subsystem 16 | Ultra-low-power embedded, IoT, edge computing 16 | Open-source (Solderpad) 16 |
| **Occamy** | Many-core processor for high-performance FP compute | RISC-V (Snitch, CVA6) | 2.5D integrated chiplet, custom FPUs, SIMD, HBM2e 17 | High-performance computing, AI/ML, scientific simulation 17 | Open-source (Solderpad, Apache-2.0) 17 |
| **Celerity** | Accelerator-centric, tiered fabric SoC | RISC-V (modified Rocket core, lightweight manycore) | Tiered architecture (General, Parallel, Specialization), mesh interconnect 23 | High-performance embedded systems, energy efficiency 23 | Open-source 23 |

This table facilitates a quick comparison of the distinct characteristics and focuses of each platform, aiding in the identification of platforms most relevant for specific "pre-compiler" research topics.

### **5\. Synthesizing Concepts for MSc Research: Opportunities and Challenges**

The exploration of hardware acceleration, heterogeneous computing, custom silicon, and specialized compilers reveals a tightly integrated ecosystem, rather than isolated concepts. The diminishing returns of Moore's Law drive the fundamental need for hardware acceleration and heterogeneous systems. This, in turn, necessitates the development of custom silicon and sophisticated "pre-compilers" to manage the inherent complexity and extract optimal performance. Open-source platforms like PULP and Celerity provide the practical testbeds for this cutting-edge research. Achieving optimal performance and energy efficiency requires co-optimization across the entire computing stack: from high-level algorithms, through programming models and compilers, down to the underlying hardware architecture.1 The "pre-compiler" is a central piece in this full-stack puzzle, serving as the nexus of modern computing challenges.

#### **5.1. Identifying Potential MSc Research Directions**

The landscape of heterogeneous computing and hardware acceleration offers numerous fertile grounds for MSc research. Based on the foundational understanding established, several potential directions for a research topic emerge:

* **Optimizing HLS for Specific Architectures:** Research could focus on developing novel pragmas, compiler passes, or modifications to existing HLS flows to better exploit the unique features of specific open-source platforms. For instance, this could involve optimizing for PULP-Hero's Shared Virtual Memory (SVM) 15, Occamy's custom floating-point units and specialized ISA extensions 17, or Celerity's tiered accelerator fabric.23 Such work directly applies HLS principles to real-world, open-source heterogeneous hardware.  
* **Developing Domain-Specific Compilers/DSLs:** A compelling research direction involves creating or extending a Domain-Specific Language (DSL) and its associated compiler. This could target a specific application domain, such as signal processing on PULPissimo's Hardware Processing Engines (HWPEs) 16 or AI workloads on Celerity's specialization tier.23 This approach directly addresses the "software complexity" challenge by raising the level of abstraction and enabling deeper, domain-specific optimizations.  
* **Automatic Hardware-Software Partitioning and Scheduling:** Research could investigate advanced compiler techniques, potentially leveraging modern intermediate representations like MLIR (e.g., the hyper dialect).5 The goal would be to automatically determine which parts of a program should execute on the general-purpose CPU and which should be offloaded to specialized accelerators, and how tasks are scheduled for optimal performance and energy efficiency. This directly tackles a core challenge of heterogeneous computing.4  
* **Memory Management and Data Coherence for Heterogeneous Systems:** Given that data movement often constitutes a significant bottleneck in heterogeneous systems, research could explore compiler-assisted techniques for efficient data transfer, cache coherence, and shared virtual memory management across diverse memory hierarchies. An example would be optimizing data flow through PULP-Hero's IOMMU for SVM.15  
* **Performance Portability Across Open-Source Platforms:** A valuable contribution would be to develop compiler strategies or intermediate representations that enable applications to be efficiently mapped to different PULP variants (Hero, Pulpissimo, Occamy) or Celerity with minimal re-tuning. This addresses a key challenge for broader adoption and reusability of heterogeneous systems.14  
* **Customizing Embedded Linux for Accelerator Integration:** Research could investigate how to optimize and customize Core Linux to seamlessly integrate with and manage specific hardware accelerators. This might involve developing custom device drivers for new HWPEs on PULPissimo or refining the Linux kernel's interaction with the accelerator in PULP-Hero.25 This area focuses on the crucial software-hardware interface and system-level integration.

The open-source nature of platforms like PULP and Celerity provides unparalleled access to full-stack heterogeneous systems for academic research, overcoming limitations often associated with proprietary commercial simulators.14 This presents a unique opportunity for an MSc student to contribute directly to the practical advancement of the field by developing and testing "pre-compiler" solutions on real hardware or detailed RTL models, thereby bridging the gap between theoretical compiler advancements and their real-world impact.

#### **5.2. Overcoming Key Challenges in Heterogeneous System Design and Programming**

While the opportunities are vast, several enduring challenges in heterogeneous system design and programming must be acknowledged and addressed in any research endeavor.

The fundamental challenge remains **bridging abstraction levels**.8 Effectively translating high-level algorithmic intent into precise, low-level hardware specifics continues to be a central problem. "Pre-compilers" must adeptly manage this gap, balancing the need for high-level programmability with the demands of hardware-specific optimizations.

**Managing system complexity** is another inherent difficulty. The sheer diversity of components, including various ISAs, memory models, and communication protocols within heterogeneous systems, makes compiler design intrinsically complex.14 Developing compilers that can intelligently navigate and optimize across this vast design space is a formidable task.

**Verification and debugging** pose significant hurdles.14 Debugging issues that span the hardware and software boundaries, particularly when automated synthesis is involved, is notoriously difficult. The deep interdependencies between components mean that a change in one part of the system can have cascading effects, requiring extensive re-validation.14

There is also a constant need to **balance performance and productivity**.10 Achieving optimal hardware performance often requires meticulous, low-level tuning, which can conflict with the goal of providing high programmer productivity through high-level tools. "Pre-compilers" strive to find the optimal balance, automating as much as possible while retaining avenues for expert-level optimization.

Finally, **keeping pace with hardware evolution** is a continuous challenge. Hardware architectures are rapidly advancing, with new RISC-V extensions, chiplet technologies, and novel memory systems constantly emerging.17 Compilers must be flexible and adaptable to support these advancements without requiring complete redesigns.

These recurring challenges in heterogeneous systems underscore that hardware-software co-design is not a transient trend but a fundamental necessity. The "pre-compiler" is a manifestation of this co-design philosophy, attempting to automate and optimize the interaction between software and hardware from the earliest stages of design. An MSc research project in this area would contribute significantly to the long-term goal of making complex heterogeneous systems more programmable and efficient.

### **6\. Conclusion and Recommended Next Steps**

The domain of hardware acceleration within heterogeneous computing systems represents a critical frontier in modern computer architecture. Driven by the limitations of general-purpose computing and the escalating demands of contemporary workloads such as AI/ML and High-Performance Computing, specialized hardware and diverse processing units have become indispensable.

"Pre-compilers," encompassing technologies like High-Level Synthesis (HLS) and Domain-Specific Languages (DSLs), serve as the essential bridge in this complex landscape. They automate the intricate translation of high-level software into efficient custom hardware, thereby managing the inherent complexity of heterogeneous systems and making hardware design more accessible. The emergence of robust open-source platforms such as PULP (including Hero, Pulpissimo, and Occamy) and Celerity provides rich, transparent environments that are ideal for exploring cutting-edge heterogeneous architectures and developing advanced compiler techniques. Ultimately, achieving optimal solutions in this field necessitates a full-stack perspective, where hardware, software, and their intricate interactions are considered in unison.

For an MSc student embarking on research in this fascinating and cutting-edge area, the following recommended next steps are crucial for defining a compelling and impactful research topic:

1. **Deep Dive into a Specific Platform:** Select one of the open-source platforms discussed, such as PULP-Hero, Celerity, PULPissimo, or Occamy, that most resonates with your interests. Thoroughly explore its documentation, code repositories, and existing examples. Understanding the architectural nuances and design philosophies of a chosen platform will provide a concrete foundation for your research.15  
2. **Explore Existing Compiler Toolchains:** Investigate the heterogeneous toolchains utilized by these platforms. For instance, examine HERO's GCC-based toolchain to understand how it implements concepts like OpenMP offloading or Shared Virtual Memory (SVM).15 This will provide insights into current practices and potential areas for improvement.  
3. **Hands-on with HLS Tools:** Gain practical experience with a commercial or open-source HLS tool (e.g., Xilinx Vitis HLS, LegUp HLS). This hands-on experience will demystify the process of converting C/C++ code into hardware and highlight the practical challenges and opportunities in HLS.  
4. **Identify a Specific Problem Area:** Based on your in-depth learning, pinpoint a specific, unsolved challenge within compiler design for heterogeneous systems that aligns with your passions. This could involve improving the quality of HLS output for a particular computational kernel, developing a DSL for a novel application domain, optimizing data movement strategies, or enhancing performance portability across different hardware targets.  
5. **Formulate Specific Research Questions:** Translate the identified problem area into clear, concise, and testable research questions that will guide your MSc thesis. These questions should define the scope and objectives of your project.  
6. **Consult with Your Advisor:** Schedule a follow-up meeting with your advisor. Come prepared with this foundational knowledge and your initial research ideas. This collaborative discussion will be vital for refining your research topic, ensuring its feasibility, and aligning it with the broader research goals of your advisor's team.

By following these steps, an MSc student can effectively navigate the complexities of hardware acceleration and heterogeneous computing, positioning themselves to make a meaningful contribution to this rapidly evolving field.

#### **Works cited**

1. Architecture, Compilers, and Parallel Computing, accessed June 15, 2025, [https://siebelschool.illinois.edu/research/areas/architecture-compilers-and-parallel-computing](https://siebelschool.illinois.edu/research/areas/architecture-compilers-and-parallel-computing)  
2. Hardware acceleration \- Wikipedia, accessed June 15, 2025, [https://en.wikipedia.org/wiki/Hardware\_acceleration](https://en.wikipedia.org/wiki/Hardware_acceleration)  
3. What is heterogenous compute? \- Arm, accessed June 15, 2025, [https://www.arm.com/glossary/heterogenous-compute](https://www.arm.com/glossary/heterogenous-compute)  
4. What is Heterogeneous Computing? \- Supermicro, accessed June 15, 2025, [https://www.supermicro.com/en/glossary/heterogeneous-computing](https://www.supermicro.com/en/glossary/heterogeneous-computing)  
5. A Method for Efficient Heterogeneous Parallel Compilation: A Cryptography Case Study, accessed June 15, 2025, [https://arxiv.org/html/2407.09333v2](https://arxiv.org/html/2407.09333v2)  
6. What is Custom Silicon – Arm®, accessed June 15, 2025, [https://www.arm.com/glossary/custom-silicon](https://www.arm.com/glossary/custom-silicon)  
7. Custom Silicon Overview \- Enosemi, accessed June 15, 2025, [https://www.enosemi.com/products/custom\_silicon\_overview/](https://www.enosemi.com/products/custom_silicon_overview/)  
8. High-level synthesis \- Wikipedia, accessed June 15, 2025, [https://en.wikipedia.org/wiki/High-level\_synthesis](https://en.wikipedia.org/wiki/High-level_synthesis)  
9. Domain-specific hardware accelerators \- DSpace@MIT, accessed June 15, 2025, [https://dspace.mit.edu/bitstream/handle/1721.1/146193/3361682.pdf](https://dspace.mit.edu/bitstream/handle/1721.1/146193/3361682.pdf)  
10. Survey of Domain-Specific Languages for FPGA ... \- Nachiket Kapre, accessed June 15, 2025, [https://nachiket.github.io/publications/rc-dsl-survey\_fpl2016.pdf](https://nachiket.github.io/publications/rc-dsl-survey_fpl2016.pdf)  
11. Compilers: Talking to The Hardware \- Unify AI, accessed June 15, 2025, [https://unify.ai/blog/deep-learning-compilers](https://unify.ai/blog/deep-learning-compilers)  
12. Introducing the Nios II C-to-Hardware Acceleration ... \- BBRC.RU, accessed June 15, 2025, [https://www.bbrc.ru/upload/iblock/e73/bykjtfx8crrv9yfjr6ecuoai7mkd898m/pgurl\_nios2\_c2hl.pdf](https://www.bbrc.ru/upload/iblock/e73/bykjtfx8crrv9yfjr6ecuoai7mkd898m/pgurl_nios2_c2hl.pdf)  
13. \[2409.13138\] Learning to Compare Hardware Designs for High-Level Synthesis \- arXiv, accessed June 15, 2025, [https://arxiv.org/abs/2409.13138](https://arxiv.org/abs/2409.13138)  
14. An Open-Source Research Platform for Heterogeneous Systems on ..., accessed June 15, 2025, [https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/568448/1/kurth\_thesis.pdf](https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/568448/1/kurth_thesis.pdf)  
15. HERO: The Open Heterogeneous Research Platform \- PULP Platform, accessed June 15, 2025, [https://pulp-platform.org/hero/](https://pulp-platform.org/hero/)  
16. pulp-platform/pulpissimo: This is the top-level project for the ... \- GitHub, accessed June 15, 2025, [https://github.com/pulp-platform/pulpissimo](https://github.com/pulp-platform/pulpissimo)  
17. Occamy \- PULP Platform, accessed June 15, 2025, [https://pulp-platform.org/occamy/](https://pulp-platform.org/occamy/)  
18. ahmad-mirsalari/PULP: A public repository discussing the PULP (Parallel Ultra Low Power) platform for open-source RISC-V processors and associated software. \- GitHub, accessed June 15, 2025, [https://github.com/ahmad-mirsalari/PULP](https://github.com/ahmad-mirsalari/PULP)  
19. PULP Platform, accessed June 15, 2025, [https://pulp-platform.org/](https://pulp-platform.org/)  
20. Occamy Overview, accessed June 15, 2025, [https://pulp-platform.github.io/occamy/rm/1\_overview.html](https://pulp-platform.github.io/occamy/rm/1_overview.html)  
21. pulp-platform/hero: Heterogeneous Research Platform (HERO) for exploration of heterogeneous computers consisting of programmable many-core accelerators and an application-class host CPU, including full-stack software and hardware. \- GitHub, accessed June 15, 2025, [https://github.com/pulp-platform/hero](https://github.com/pulp-platform/hero)  
22. Architecture of the PULPissimo microcontroller. | Download Scientific Diagram, accessed June 15, 2025, [https://www.researchgate.net/figure/Architecture-of-the-PULPissimo-microcontroller\_fig1\_335174529](https://www.researchgate.net/figure/Architecture-of-the-PULPissimo-microcontroller_fig1_335174529)  
23. Celerity | \- GitHub Pages, accessed June 15, 2025, [https://shawnless.github.io/Personal//projects/2\_celerity/](https://shawnless.github.io/Personal//projects/2_celerity/)  
24. OpenCelerity | Open-Source RISC-V Tiered Accelerator Fabric SoC, accessed June 15, 2025, [http://opencelerity.org/](http://opencelerity.org/)  
25. What Is Embedded Linux? \- Wind River Systems, accessed June 15, 2025, [https://www.windriver.com/solutions/learning/embedded-linux](https://www.windriver.com/solutions/learning/embedded-linux)  
26. Crafting a Custom Linux Kernel for Your Embedded Projects, accessed June 15, 2025, [https://www.linuxjournal.com/content/crafting-custom-linux-kernel-your-embedded-projects](https://www.linuxjournal.com/content/crafting-custom-linux-kernel-your-embedded-projects)