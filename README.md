# ECE585_TomasuloAlgorithm_Modifed

##C++ Tomasulo Algorithm Datapath/Simulator w/ Load Fubnctionality

**Author: Aron Resty B. Ramillano**

Original simulator was created by:
**Arthur J. Miller**

---

###This program follows the algorithm as stated by the following sources

1. Computer Architecture : A Quantitative Approach (5th Edition) by Hennessy, John L., Patterson, David A.
2. https://en.wikipedia.org/wiki/Tomasulo_algorithm#Implementation_concepts
3. Dr. Chandra professor at Cal Poly Pomona

Full project can be found on www.github.com/milleraj66/ECE585_TomasuloAlgorithm

This modified project is found on https://github.com/PERSEUS-1337/ECE585_TomasuloAlgorithm_Modified

Date of last development: 12-08-2022

---

#INSTRUCTIONS FOR USE OF THIS PROGRAM:
**1. SETUP RESERVATION STATION ARCHITECTURE:**

a. Define the constants for the # of Reservation Stations

     // NUMBER OF RESERVATION STATIONS
     const int Num_LOAD_RS = 2
     const int Num_ADD_RS = 4;
     const int Num_MULT_RS = 2;
     const int Num_DIV_RS = 3;

b. Define latency's

    // RESERVATION STATION LATENCY
    const int LOAD_Lat = 2
    const int ADD_Lat = 4;
    const int MULT_Lat = 12;
    const int DIV_Lat = 38;
    // Datapath Latency
    const int ISSUE_Lat = 1;
    const int WRITEBACK_Lat = 1;

c. Initialize Reservation Station Objects

     //// Input reservation station architecture
     ReservationStation
             LOAD1(LoadOp, OperandInit),
             LOAD2(LoadOp, OperandInit);
     ReservationStation
             ADD1(AddOp, OperandInit),
             ADD2(AddOp, OperandInit),
             ADD3(AddOp, OperandInit),
             ADD4(AddOp, OperandInit);
     ReservationStation
             MULT1(MultOp, OperandInit),
             MULT2(MultOp, OperandInit);
     ReservationStation
             DIV1(DivOp, OperandInit),
             DIV2(DivOp, OperandInit),
             DIV3(DivOp, OperandInit);

** Note: Currently only 12 registers are being used. To add more registers to the architecture
you must edit both the `RegisterStatus` objects and `Register` objects **

    // Initialize register status objects
    RegisterStatus
            F0(RegStatusEmpty),F1(RegStatusEmpty),
            F2(RegStatusEmpty),F3(RegStatusEmpty),
            F4(RegStatusEmpty), F5(RegStatusEmpty),
            F6(RegStatusEmpty),F7(RegStatusEmpty),
            F8(RegStatusEmpty),F9(RegStatusEmpty),
            F10(RegStatusEmpty),F11(RegStatusEmpty),
            F12(RegStatusEmpty);
    // Pack register status objects into vector
    vector<RegisterStatus> RegisterStatus =
            {F0, F1, F2, F3, F4, F5, F6, F7, F8, F9, F10, F11, F12};

    // Initialize register file vector
    vector<int> Register = {ZERO_REG,1,2,3,4,5,6,7,8,9,10,11,12};

**2. INITIALIZE PROGRAM:**

a.Input the MIPS like instructions to your given program `Ex. ADD F1,F2,F3 // rd <- rs + rt`

    // Input program instructions
    // Has load operations to simulate the examples
    Instruction
        I0(6, 12, 2, LoadOp),
        I1(2, 12, 3, LoadOp),
        I2(0, 2, 4, MultOp),
        I3(8, 6, 2, SubOp),
        I4(10, 0, 6, DivOp),
        I5(6, 8, 2, AddOp);

**3. COMPILE AND RUN PROGRAM**

a. compile using provided makefile with "make all" command

b. run program

**4. OUTPUT:**

a. Displays the register content of each clock cycle

     Register Content:
     5000 5 2 3 10 5 -1 6 25 100 10 -12 12

b. Displays the timing diagram of the ISSUE EXECUTE WRITEBACK for each instruction at each clock cycle

        Inst      Issue     Execute   WB        SystemClock
                                                        57

        0         1         2-3         4
        1         2         3-4         5
        2         3         6-15        16
        3         4         6-7         8
        4         5         17-56        57
        5         6         9-10        11

---

# Here is the example program that has been tested

Instructions for provided example test program:

    I0: LOAD    F6,F12,F2
    I1: LOAD    F2,F12,F3
    I2: MULT    F0,F2,F4
    I3: SUB     F8,F6,F2
    I4: DIV     F10,F0,F6       // Data Dependency on I2
    I5: ADD     F6,F8,F2

Code for defining given example test program

    Instruction
                // Has load operations to simulate the examples
                I0(6, 12, 2, LoadOp),
                I1(2, 12, 3, LoadOp),
                I2(0, 2, 4, MultOp),
                I3(8, 6, 2, SubOp),
                I4(10, 0, 6, DivOp),
                I5(6, 8, 2, AddOp);

Initial Register File Values

    F0 = 5000 (This is zero for our purpose)
    F1 = 1
    F2 = 2
    F3 = 3
    F4 = 4
    F5 = 5
    F6 = 6
    F7 = 7
    F8 = 8
    F9 = 9
    F10 = 10
    F11 = 11
    F12 = 12

Code for defining given initial register file values

    // Initialize register file vector
    vector<int> Register = {ZERO_REG,1,2,3,4,5,6,7,8,9,10,11,12};

Expected Output w/ given reservation station architecture (This was checked by hand)

    Register Content:
    36 1 9 3 4 5 10 7 1 9 3 11 12
    Inst      Issue     Execute   WB        SystemClock
                                                    57

    0         1         2-3         4
    1         2         3-4         5
    2         3         6-15        16
    3         4         6-7         8
    4         5         17-56        57
    5         6         9-10        11

**NOTE: By changing the number of reservation stations you can see the timing change.
This program has been tested with several different reservation station architectures**
