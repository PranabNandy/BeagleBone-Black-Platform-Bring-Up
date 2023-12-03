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


## pinctrl-single,pins property
- The pin configuration nodes for pinctrl-single are specified as pinctrl register offset and value pairs using pinctrl-single,pins.
- For example, setting a pin for a device could be done with:
- pinctrl-single,pins = <0xdc 0x118>;
- Where 0xdc is the offset from the pinctrl register base address for the device pinctrl register, and
-  0x118 contains the desired value of the pinctrl register. See the device example and static board pins example below for more information.

![Screenshot from 2023-12-03 09-34-09](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/6b090ba7-ac6f-4563-87a9-f70a30e5dc78)

## Pinctrl related properties to be used with client device nodes

- pinctrl-names : The list of names to assign states. List entry 0 defines the name for integer state ID 0, list entry 1 for state ID 1, and so on.
- pinctrl-<id> : List of phandles, each pointing at a pin configuration node within a pin controller

![Screenshot from 2023-12-03 09-31-38](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/66f64a43-45de-4c47-8298-a9dfc7831d92)

![Screenshot from 2023-12-03 09-08-44](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/485574a3-6fa3-4467-952c-f719f73efdc6)

![Screenshot from 2023-12-03 09-09-29](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/0710bfd0-95c7-4b8d-9126-1c34b41028c4)

![Screenshot from 2023-12-03 09-10-14](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/5b500f9a-0fea-447f-ad97-83dc09832863)
