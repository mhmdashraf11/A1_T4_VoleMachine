#include <iostream>
using namespace std;

class Register {
private:
    int memory[16];
public:
    int getCell(int index) {
        return memory[index];
    }
    void setCell(int index, int value) {
        memory[index] = value;
    }
};
static int registersize = 16;

class Memory {
private:
    string memory[256];
public:
    int size = 256;

    string getCell(int index) {
        return memory[index];
    }
    void setCell(int index, string value) {
        memory[index] = value;
    }
};

class PC {
    int address;
public:
    PC() {
        address = 0;
    }
    void increment() {
        address++;
    }
    void setValue(int newAddress) { // to jump
        address = newAddress;
    }
};

class ALU {
public:
    int hexToDecimal(string str) {  // Return type changed to int
        int dec = 0;
        for (int i = 0; i < str.length(); i++) {
            dec = dec * 16 + (str[i] >= 'A' ? str[i] - 'A' + 10 : str[i] - '0');
        }
        return dec;
    }

    string decimalToHex(int dec) {
        if (dec == 0) return "0";  // Handle zero case

        string hex = "";
        while (dec > 0) {
            int digit = dec % 16;
            char hexChar = (digit < 10 ? '0' + digit : 'A' + (digit - 10));
            hex = hexChar + hex;
            dec /= 16;
        }
        return hex;
    }
    bool isValid(string str) {
        for (char c : str) {
            if (!(c >= '0' && c <= '9') &&!(c >= 'A' && c <= 'F')) {
                return false;
            }
        }
        return true;
    }
    void add(int idx1,int idx2,int idx3,Register& reg){
        // Add two integers and store the result in register
        int sum = reg.getCell(idx1) + reg.getCell(idx2);
        reg.setCell(idx3, sum);
    }
};

class CU {
public:
    void load(int intReg, int intMem, Register& Reg, Memory& Mem) {
        ALU alu;
        // Convert hex string from memory to decimal integer for register storage
        int decimalValue = alu.hexToDecimal(Mem.getCell(intMem));
        Reg.setCell(intReg, decimalValue);
    }
//    void loadFromRegisterToMemory(int intReg, int intMem, Register& Reg, Memory& Mem) {
//        // Load value from register to memory
//        Reg.getCell(intReg); // Simulate reading from register
//        ALU alu;
//        Mem.setCell(intMem, alu.decimalToHex(Reg.getCell(intReg))); // Convert decimal to hex string and store in memory
//    }
    void Store(int intReg, int intMem, Register& Reg, Memory& Mem){
        // Store value from register to memory
        Reg.getCell(intReg); // Simulate reading from register
        ALU alu;
        Mem.setCell(intMem, alu.decimalToHex(Reg.getCell(intReg))); // Convert decimal to hex string and store in memory
    }
    void jump(int newAddress, PC& PC) {
        PC.setValue(newAddress);
    }
    void move(int idxReg1, int idxReg2, Register& reg) {
        // Copy value from idxReg2 to idxReg1
        reg.setCell(idxReg1, reg.getCell(idxReg2));

        // Clear the source register (idxReg2)
        reg.setCell(idxReg2, 0);
    }
    void halt(){
        cout << "Program halted." << endl;
        exit(0);
    }
};
