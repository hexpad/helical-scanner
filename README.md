## Helical Scanner

This program performs a 3D helical scan. The calculated x, y, and z coordinates are saved in the HelicalScan.txt file. It can be used for radar simulations or other applications requiring helical scanning coordinates.


<img width="604" height="535" alt="helical" src="https://github.com/user-attachments/assets/576a277a-c467-4b01-9483-62a7f8fee6ee" />


## How to use

1. Compile the program
   
2. Run the program

Enter the requested parameters when prompted:

```
Period (milliseconds): 100
Azimuth Center (degrees): 85.1
Elevation Center (degrees): -90
Squint Angle (degrees): 50
Squint Angle Velocity (degrees/second): 1
PRI (microseconds): 100
Helix Radius (meters): 0.8
Layer Height (meters): 0.001
Angle Increment (degrees): 1
```

After execution, check the file HelicalScan.txt for the generated coordinates.

## Example Output

0.5142300877,0.6128355545,-0.0000000000

0.5034552277,0.6217176479,-0.0010000000

...

0.6791758744,-0.4227530386,-0.9980000000

0.6864512075,-0.4108341997,-0.9990000000

## Source Code
https://github.com/hexpad/helical-scanner/blob/main/main.c
