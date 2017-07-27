# Agilent 34410A Digital Multimeter

The Agilent multimeter has a ethernet port and can be accessed from Python using the [InstrumentKit](https://github.com/Galvant/InstrumentKit)

Determining ip address and port

1. Enter Utility Menu by pressing Shift | Utility
2. Select Remote I/O and press Enter
3. Select Lan and press Enter
4. Select Yes for Enable Lan? and press Enter
5. Select View and press Enter
6. Press right arrow until you see the IP address
7. Assuming the IP Address is 192.168.1.100 point a web browser from a computer on the same subnet to http://192.168.1.100 click the **Advance Information** link and note the **SCPI TCPIP Socket Port** (for these instruction let's assume the port is 5025)

Launch a Python environment (We like docker)

```bash
$ docker run -it microsoft/cntk:2.0-cpu-python3.5 bash

# install InstrumentKit
$ pip install instrumentkit

# Launch Python inertactively
$ python
```

From the interactive Python prompt

```python
>>> import instruments as ik
>>> inst = ik.generic_scpi.SCPIMultimeter.open_gpibusb("192.168.1.100", 5025)
>>> reading = inst.measure(inst.Mode.voltage_dc)
>>> print("Value: {}, units: {}".format(reading.magnitude, reading.units))
```
