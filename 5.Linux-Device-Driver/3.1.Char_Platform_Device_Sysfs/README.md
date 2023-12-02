# Char Platform Driver with sysfs
![Screenshot from 2023-12-02 18-16-32](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/34576104/ab625f95-8f45-4f33-a756-7362f319f6d6)



### Pin control core
- Pinctrl core implementation you can find in drivers/pinctrl/core.c
- This provides pin control helper functions to any consumer drivers
- Maintains pin exclusivity for a device
- Provides debug interface to user space through sysfs
