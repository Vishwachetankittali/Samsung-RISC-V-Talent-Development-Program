# Samsung RISC-V Talent Development Program

## Basic Details 

**Name:**  Vishwachetan Kittali  
**College:** New Horizon college of Engineering  
**Email:** kittalivishwachetan@gmail.com  
**GitHub Profile:** [Vishwachetankittali](https://github.com/Vishwachetankittali)  
**LinkedIn Profile:** [vishwachetan-kittali](https://linkedin.com/in/vishwachetan-kittali)  

----

## Tasks

### [Task 1: Environment setup and comparing RISC-V compiler with gcc compiler](https://github.com/Vishwachetankittali/Samsung-RISC-V-Talent-Development-Program/tree/main/task%201)
**Description:** Refer to C-based and RISC-V-based lab videos to:

- Compile C code using `gcc`.
- Compile and count the number of instructions used in `o1` and `ofast` using the RISC-V compiler.

### [Task 2: Analyzing RISC-V Performance with Compiler Optimization Flags (-O1 and -Ofast)](https://github.com/Vishwachetankittali/Samsung-RISC-V-Talent-Development-Program/tree/main/TASK%202)
**Description:** In this task, you will explore the compilation process for both standard C and RISC-V architecture:

- Compile a simple C program using the `gcc` compiler.
- Use the RISC-V compiler to compile the same code with optimization flags `-O1` and `-Ofast`.
- Count and compare the number of instructions generated for each optimization level, understanding the impact of compiler optimizations on RISC-V assembly code.

### [Task 3: Decoding and Analyzing RISC-V Instruction Types and 32-Bit Patterns](https://github.com/Vishwachetankittali/Samsung-RISC-V-Talent-Development-Program/tree/main/TASK%203)  
**Description:** In this task, you will analyze and decode RISC-V assembly instructions using instruction type formats.

A. **Review RISC-V Instruction Formats:**  
   - Studying the official RISC-V software documentation to understand the following instruction types:  
     - **R-type**  
     - **I-type**  
     - **S-type**  
     - **B-type**  
     - **U-type**  
     - **J-type**  

B. **Analyze Application Code:**  
   - Use the `riscv-objdump` tool to analyze your application code.  
   - Identify 15 unique RISC-V instructions from the disassembled output.

### [Task 4: Functional Simulation of RISC-V Core](https://github.com/Vishwachetankittali/Samsung-RISC-V-Talent-Development-Program/tree/main/TASK%204) 

**Description**:  
This task involves performing a functional simulation of a given RISC-V Core Verilog netlist and testbench to verify the core's functional correctness and document the results.

 **Objectives**  
- Simulate the RISC-V Core using the provided Verilog netlist and testbench.  
- Verify the functional correctness by analyzing the output signals.  
- Document the results with waveform snapshots and upload them to GitHub.  

#### Steps  

 **1. Download Files**  
 **2. Set Up Simulation Environment**  
 **3. Run Functional Simulation**  
 **4. Capture Waveforms**  
**5. Documentation to repository**  

### [Task 5: Ultrasonic Obstacle Detection Circuit using VSD_Squadron-MINI](https://github.com/Vishwachetankittali/Samsung-RISC-V-Talent-Development-Program/tree/main/TASK%205)

**Description:**
This task involves implementing an **Ultrasonic Obstacle Detection Circuit** using the **VSD_Squadron-MINI** (RISC-V-based SoC). The system utilizes an **HC-SR04 ultrasonic sensor** to measure distance and an **active buzzer** to provide alerts based on obstacle proximity. If an object is detected **within 50 cm**, the buzzer is activated, enhancing real-time obstacle awareness.

**Objectives:**
- Implement an **Ultrasonic Obstacle Detection Circuit** on the **VSD_Squadron-MINI**.
- Use the **HC-SR04 ultrasonic sensor** to measure distances.
- Process sensor data and trigger an **active buzzer** based on obstacle proximity.
- Ensure real-time distance detection and alert functionality.

#### Steps:
1. **Circuit Setup** – Connect **HC-SR04** for distance measurement and **buzzer** for alerts.
2. **GPIO Configuration** – Initialize and configure GPIO pins for **sensor input and buzzer output**.
3. **Distance Measurement Logic** – Implement obstacle detection using **time-of-flight calculations**.
4. **Buzzer Activation** – Trigger the buzzer **when an obstacle is within 50 cm**.
5. **Testing & Verification** – Validate system functionality with real-world obstacles.
6. **Documentation to Repository** – Upload circuit schematics, firmware code, and results to **GitHub**.

### [Task 6: Final Code Submission & Application Demo](https://github.com/Vishwachetankittali/Samsung-RISC-V-Talent-Development-Program/tree/main/TASK%206)  

**Description**  
This task involves submitting the complete working code and a demonstration video showcasing the application on the **VSD_Squadron-MINI (RISC-V-based SoC)**.  

**Objectives:**
- Upload the final code and documentation to **GitHub**.  
- Record and submit a **demo video** of the working application.  
- Validate the **functionality** in real-world conditions.  

#### Steps  
1. **Code Finalization** – Review, optimize, and document the firmware.  
2. **GitHub Submission** – Upload code, circuit schematics, and explanations.  
3. **Demo Video** – Record and submit a clear demonstration of the system.  
4. **Final Submission** – Share the **GitHub repository link** and **video** as per guidelines.  

### Goal  
To successfully complete the project by providing a **working implementation of RISC-V technology,** showcasing practical applications, and enabling knowledge sharing through proper documentation and demonstration.
