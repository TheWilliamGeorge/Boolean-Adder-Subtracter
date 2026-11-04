# Boolean-Adder-Subtracter
A simple 4-bit adder/subtracter using only logic gates


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
