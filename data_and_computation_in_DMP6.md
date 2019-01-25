# Type and function used in DMP6

##	The following is a list of variable involved in the arduino code


```cpp
Quaternion q;           // [w, x, y, z]         quaternion container
VectorInt16 aa;         // [x, y, z]            accel sensor measurements
VectorInt16 aaReal;     // [x, y, z]            gravity-free accel sensor measurements
VectorInt16 aaWorld;    // [x, y, z]            world-frame accel sensor measurements
VectorFloat gravity;    // [x, y, z]            gravity vector
float euler[3];         // [psi, theta, phi]    Euler angle container
float ypr[3];           // [yaw, pitch, roll]   yaw/pitch/roll container and gravity vector
```

The functions related to them are 

| Related variable | Function Name | Task |
|--------|--------|--------|
| q | dmpGetQuaternion      |  fetch data from fifobuffer[0-13]      |
|  aa |  dmpGetAccel     | fetch data from fifobuffer[28-37] |
| euler | dmpGetEuler | value computation |
| gravity | dmpGetGravity | value computation |
| ypr | dmpGetYawPitchRoll | value computation |
| aaReal | dmpGetLinearAccel | value computation |
| aaWorld | dmpGetLinearAccelInWorld | value computation |

##	Problem

At this moment, the code reads from fifobuffer for q and aa values in two seperate function calls. It will case mismatch in values. Also, after a number of loop, code will halt in Arduino. Hence, we can try to write all above code into one single function to improve the performance.

