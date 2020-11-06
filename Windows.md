### Some tips in Windows system
### Some tips in Windows system

#### 1. How to hide a driver partition in win10? ( The best way(Highly recommand )

1. win+r open command

2. Type regeidt

3. file -> export [ backup current registry] 

4. open following path: 

   ```
   HEKY_CURRENT_USER\SOFTWARE
   \MICROSOFT\WINDOWS\CURRENT VERSION
   \POLICES\EXPLORER
   ```

5. Right-click inside the right side space and select [create a new DWORD (32-bit) value]

6. <b>name it "NoDrivers"</b>

7. open "NoDrivers", type the value data and select "base -> Decimal"

   - A : 1
   - B : 2
   - C ：4
   - D ：8
   - E ：16
   - F :  32
   - G : 64
   - H : 128
   - I : 256
   - J : 512
   - K : 1024
   - L : 2048
   - M : 4096
   - N : 8192
   - O : 16384
   - P : 32768
   - Q : 65536
   - R : 131072
   - S : 262144
   - T : 524288
   - U : 1048576
   - V : 2097152
   - W : 4194304
   - X : 8388608
   - Y : 16777216
   - Z : 33554432
   - ALL : 67108863

     save after typing

8. sign out and log in

9. MAGIC SHOWS
