# Helical Scanner

This program performs a 3D helical scan. The calculated x, y, and z coordinates are saved in the HelicalScan.txt file. It can be used for radar simulations or other applications requiring helical scanning coordinates.





```c


#include <stdio.h>
#include <math.h>
#include <unistd.h> // usleep function

#define FILENAME "HelicalScan.txt" 

typedef struct {
    double x;
    double y;
    double z;
} Coord;

Coord performHelicalScan(double helixR, double layerH, double angle, double squintAng) {
    Coord c;
    c.x = helixR * cos(angle * M_PI / 180.0 + squintAng * M_PI / 180.0);
    c.y = helixR * sin(angle * M_PI / 180.0 + squintAng * M_PI / 180.0);
    c.z = -angle * layerH; 
    return c;
}

int main() {
    double prd, aziCenter, eleCenter, squintAng, squintVel, priUs, helixR, layerH, angleInc;

    printf("Period (milliseconds): ");
    scanf("%lf", &prd);
    printf("Azimuth Center (degrees): ");
    scanf("%lf", &aziCenter);
    printf("Elevation Center (degrees): ");
    scanf("%lf", &eleCenter);
    printf("Squint Angle (degrees): ");
    scanf("%lf", &squintAng);
    printf("Squint Angle Velocity (degrees/second): ");
    scanf("%lf", &squintVel);
    printf("PRI (microseconds): ");
    scanf("%lf", &priUs);
    printf("Helix Radius (meters): ");
    scanf("%lf", &helixR);
    printf("Layer Height (meters): ");
    scanf("%lf", &layerH);
    printf("Angle Increment (degrees): ");
    scanf("%lf", &angleInc);

    FILE *file = fopen(FILENAME, "w");
    if (file == NULL) {
        printf("File could not be opened.\n");
        return 1;
    }

    double angle = 0.0; 
    int iterations = (int)(prd * 1000 / priUs);

    for (int i = 0; i < iterations; i++) {
// calculate and write helical scan coordinates for each step
        Coord c = performHelicalScan(helixR, layerH, angle, squintAng);
        fprintf(file, "%.10f,%.10f,%.10f\n", c.x, c.y, c.z);
        angle += angleInc;
        squintAng += squintVel * priUs / 1000000;
        usleep(priUs);
    }

    fclose(file);

    return 0;
}


```

# How to use

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

# Example Output

0.5142300877,0.6128355545,-0.0000000000

0.5034552277,0.6217176479,-0.0010000000

...

0.6791758744,-0.4227530386,-0.9980000000

0.6864512075,-0.4108341997,-0.9990000000
