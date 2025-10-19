# Helical Scanner

This program performs a 3D helical scan. The calculated x, y, and z coordinates are saved in the HelicalScan.txt file. It can be used for radar simulations or other applications requiring helical scanning coordinates.

```cpp

#include <stdio.h>
#include <math.h>
#include <unistd.h> // usleep function

#define FILENAME "HelicalScan.txt" // Output file

// Structure to store coordinates
typedef struct {
    double x;
    double y;
    double z;
} Coordinates;

// Helical scan and coordinate calculation
Coordinates performHelicalScan(double radius, double layer_height, double angle, double squint_angle) {
    Coordinates coords;

    // Calculate x and y coordinates
    coords.x = radius * cos(angle * M_PI / 180.0 + squint_angle * M_PI / 180.0);
    coords.y = radius * sin(angle * M_PI / 180.0 + squint_angle * M_PI / 180.0);

    // Calculate z coordinate
    coords.z = -angle * layer_height; // negative sign for counterclockwise rotation

    return coords;
}

int main() {
    double period, azimuth_center, elevation_center, squint_angle, squint_angle_velocity, pri_microseconds, helix_radius, layer_height, angle_increment;

    // Get parameters from user
    printf("Period (milliseconds): "); //100
    scanf("%lf", &period);
    printf("Azimuth Center (degrees): "); //85.1
    scanf("%lf", &azimuth_center);
    printf("Elevation Center (degrees): "); //-90
    scanf("%lf", &elevation_center);
    printf("Squint Angle (degrees): "); //50
    scanf("%lf", &squint_angle);
    printf("Squint Angle Velocity (degrees/second): "); //1
    scanf("%lf", &squint_angle_velocity);
    printf("PRI (microseconds): "); //100
    scanf("%lf", &pri_microseconds);
    printf("Helix Radius (meters): "); //0.8
    scanf("%lf", &helix_radius);
    printf("Layer Height (meters): "); //0.001
    scanf("%lf", &layer_height);
    printf("Angle Increment (degrees): "); //1
    scanf("%lf", &angle_increment);

    // Open the file in write mode
    FILE *file = fopen(FILENAME, "w");
    if (file == NULL) {
        printf("File could not be opened.\n");
        return 1;
    }

    double angle = 0.0; // Starting angle

    // Calculate the required number of iterations
    int iterations = (int)(period * 1000 / pri_microseconds);

    // Perform helical scan and write coordinates to file
    for (int i = 0; i < iterations; i++) {
        Coordinates coords = performHelicalScan(helix_radius, layer_height, angle, squint_angle);
        fprintf(file, "%.10f,%.10f,%.10f\n", coords.x, coords.y, coords.z);
        angle += angle_increment;
        squint_angle += squint_angle_velocity * pri_microseconds / 1000000;
        usleep(pri_microseconds); // usleep function
    }

    // Close the file
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
