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

The solution at this moment is to divide the output into several different scripts and change line every output:
```cpp
Serial.print(q.w);
    Serial.print(F(" "));
    Serial.print(q.x);
    Serial.print(F(" "));
    Serial.print(q.y);
    Serial.print(F(" "));
    Serial.println(q.z);
    //Serial.println(F(" "));
    Serial.print(euler[0] * 180 / M_PI);
    Serial.print(F(" "));
    Serial.print(euler[1] * 180 / M_PI);
    Serial.print(F(" "));
    Serial.println(euler[2] * 180 / M_PI);
//    Serial.print(F(" "));
    Serial.print(ypr[0] * 180 / M_PI);
    Serial.print(F(" "));
    Serial.print(ypr[1] * 180 / M_PI);
    Serial.print(F(" "));
    Serial.println(ypr[2] * 180 / M_PI);
//    Serial.print(F(" "));
    Serial.print(aaReal.x);
    Serial.print(F(" "));
    Serial.print(aaReal.y);
    Serial.print(F(" "));
    Serial.println(aaReal.z);
//    Serial.print(F(" "));
    Serial.print(aaWorld.x);
    Serial.print(F(" "));
    Serial.print(aaWorld.y);
    Serial.print(F(" "));
    Serial.println(aaWorld.z);
```

As a result, now Arduino can send these values to the serial port and later recorded. Now, matlab code can read in data from the saved text. The function name used here is `read_formatted_data_from_IMU_text.m`


