# Useful stuff for Raspberry Pi

### Show GPU temperature 

```
vcgencmd measure_temp
```
OR
```
/opt/vc/bin/vcgencmd measure_temp
```

### Show CPU temperature
```
cat /sys/class/thermal/thermal_zone0/temp
```
Divide it by 1000 to see the temperature in a more readable
```
cpu=$(</sys/class/thermal/thermal_zone0/temp)
echo "$((cpu/1000)) c"
```

A script to put it togther
```
#!/bin/bash
cpu=(</sys/class/thermal/thermal_zone0/temp) echo "(</sys/class/thermal/thermalzone0/temp)echo"(date) @ $(hostname)"
echo "-------------------------------------------"
echo "GPU => $(/opt/vc/bin/vcgencmd measure_temp)"
echo "CPU => $((cpu/1000))'C"
```

---

# Configure a Static IP address
