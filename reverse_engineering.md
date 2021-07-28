# Reverse engineering

Reverse engineering was performed using two methods: decompilation and packet capture. 

Decompilation was done using dex2jar and fernflower to produce broken but somewhat readable Java code and apktool for the parts fernflower didn't manage to decompile. The code is obfuscated but enums keep their original names so no guessing as to what values mean is neccessary. The interesting parts are in the package `com.sony.songpal.tandemfamily.message.mdr`.

Packet capture was performed by enabling btsnoop on an android device and viewing the packets in wireshark. This helps in figureing out what order and what values the packets should have where the code is unreadable.
