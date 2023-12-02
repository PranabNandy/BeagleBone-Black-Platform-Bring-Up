# Char Platform Driver with sysfs
![Screenshot from 2023-12-02 18-16-32](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/ab625f95-8f45-4f33-a756-7362f319f6d6)



### Pin control core
- Pinctrl core implementation you can find in drivers/pinctrl/core.c
- This provides pin control helper functions to any consumer drivers
- Maintains pin exclusivity for a device
- Provides debug interface to user space through sysfs

Pin control subsystem of Linux is used to configure pins of a
microprocessor/microcontroller

 What is **pin configuration ?**

- Pin mode configuration for alternate functions
- Slew rate adjustment
- Driver strength
- Pull up and pull down resistor enable/disable
- Output type control (push pull, open drain)

![Screenshot from 2023-12-02 18-45-55](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/5559ba6f-665e-4d54-8b9c-59da8b80cbcc)


## What is a pin-controller ?
- Hardware modules that control pin multiplexing and configuration
parameters such as pull-up/down, tri-state, drive-strength are
designated as pin controllers.
- Each pin controller must be represented as a node in the device tree,
just like any other hardware module node.
- For am335x, the **pad config registers** are called as **pin controllers**
