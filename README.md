# Boolean-Adder-Subtracter
A simple 4-bit adder/subtracter using only logic gates

- BOM: [BOM.md](BOM.md)
- BOM: [BOM.csv](BOM.csv)
- Journal: [JOURNAL.md](JOURNAL.md)
- Logic gate design: [logic.png](logic.png)
- Logic gate design source file: [mokxi.json](mokxi.json)
- Breadboard design: [breadboard.png](breadboard.png)
- No source file for breadboard since TinkerCad won't let you export them

## What is this?
A boolean adder uses logic gates to add binary values. In this project, I have implemented the theory of boolean adders that I learned in my Computer Science A-Level Studies in a real-world scenario - on a breadboard. Breadboards are super useful for prototyping and building electronics projects without soldering, making debugging much easier. 
This project can add and also subtract 2 4-bit numbers (so numbers up to 15). It also has a memory module to store and display the last calculation. 
In the original logic gates design, it was also going to read the values back from memory but I couldn't get this fully working, or fit it within the project scope, so decided against it. I was also going to implement the memory only using NAND gates and inverters, but again, due to complexity and budget considerations, I decided to use dedicated TI flip flop ICs.


## How it works
Okay, prepare yourself - this is going to be complicated. 

<img width="10216" height="2696" alt="4-bit-adder-subtracter" src="https://github.com/user-attachments/assets/45dd2b85-1067-4ccd-be06-73b28a4b303b" />

This is the 4-bit adder/subtracter. Each of the four modules is capable of adding two 1-bit numbers, and a carry bit. The first adder sends its carry bit into the carry in of the second adder, and so on. The final adder sends its carry bit to an output LED for overflow. 
The input B is XORd with a control signal, K, to allow subtraction. The easiest way to do binary subtraction on this level is using two's complement. This is where you flip each bit of the number to be subtracted, and add one, making it into its two's complement (a signed way to show negative numbers in binary) equivalent. The really clever part is how the add one is implemented. The carry in bit for the first adder takes input from the control line, meaning that a 1 is automatically added to the result (instead of added to the second number - this works because of the associativity of addition). Pretty neat, huh?

---

<img width="6862" height="1000" alt="register" src="https://github.com/user-attachments/assets/1cc62fef-ed0f-4d4e-990d-d1881e98b9e5" />

The second part of this system is the memory. I have implemented a basic 5-bit accumulator (ACC) to store the result of the previous calculation. This is again done only with logic gates. The ACC is made up of 5 Master-Slave D Flip-Flops, each taking input from the output of the four adders and the fourth carry out bit. Where this gets clever is how values are added to the accumulator. 
Usually, such a flip-flop uses a clock signal to change its state. When the clock signal is low, the state is locked, no matter what signal goes into the input. However, when the clock signal is high, the value can be changed to match the input signal. I only want my accumulator to store the value once the calculation is done. The output LEDs light up and change as you enter the numbers to calculate with, hence the need for a master-slave design. Instead of a clock giving the write-allow signal, though, a button provides it. It functions in exactly the same way, but implements the functionality of a STORE button. 

---

<img width="5198" height="647" alt="just multiplexers" src="https://github.com/user-attachments/assets/d69ea3c1-3b2b-4cb1-85bb-e51402e08b83" />

The final part of this system that I implemented today is the ability to calculate with the value stored in memory. To do this, we need to be able to take input from either the Input A switches, or from memory, leaving B unchanged (since we can turn B into a negative number to be subtracted. This is how a real calculator behaves). To do this, the circuit uses four 2-to-1 multiplexers. These can be seen above the A and B input switches on the above image. They take input from A, the ACC and the memory-read mode switch. They output the values from the Input A switches **UNLESS** the mode switch is high, in which case, they override the A values with the values from the ACC. This passes these values back into the adder as input A.

## Images
The full logic design:

<img width="6862" height="2332" alt="image" src="https://github.com/user-attachments/assets/9d0589b7-1492-4831-9700-5bb912d13b78" />

The full breadboard design:

<img width="1984" height="1148" alt="image" src="https://github.com/user-attachments/assets/e7d79814-c27e-4f9e-870f-1a8f92483e00" />

The breadboard calculating a sum while a previous result is stored in memory, inside the Tinkercad simulator:

<img width="1642" height="1020" alt="image" src="https://github.com/user-attachments/assets/42946823-0252-47d8-8ba7-4112ec21fbab" />
