# MW3 MemberJoin RCE
Makes use of a vulnerable `MSG_ReadData` call on a stack allocated buffer in the function `PartyAtomicHost_HandleMemberJoin`
This repository only has some POC code for a protection and a premade DLL. I will not be releasing a POC for the exploit itself any time soon...

# Explanation
![Game ASM](https://github.com/Peribunt/MW3-RCE1/blob/main/POC.png?raw=true)

As you can see; The call to `MSG_ReadShort` determines the size of the next block of data being read into the allocated buffer of 512. Manipulating the short in the sent message enables arbitrary code execution. This exploit can be defeated quite easily with some prior knowledge about buffer lengths

# Defeating this exploit
![Protection Code](https://github.com/Peribunt/MW3-RCE1/blob/main/ProtPOC.png?raw=true)

By hooking `MSG_ReadData` and checking if the return address lands on the address after the correct call, you can defeat this exploit by simply checking the `dwReadLen` value.
If this value is greater than the buffer length, we simply reset it to be fixed to the buffer length
