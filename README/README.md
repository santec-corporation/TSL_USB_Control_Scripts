<p align="right"> <a href="https://www.santec.com/jp/" target="_blank" rel="noreferrer"> <img src="https://www.santec.com/dcms_media/image/common_logo01.png" alt="santec" 
  width="250" height="45"/> </a> </p>


<h1>TSL USB Control Scripts</h1>

Python Scripts to command and manage Santec TSL Device(s) connected via USB utilizing the Santec_FTDI DLL as the backend.


<h2>Introduction</h2>

The Santec TSL Devices connected via USB can be controlled using python scripts with the help of a DLL (Santec_FTDI).

The Santec_FTDI DLL (Santec FTD2xx_helper) is designed to connect to TSL(s) and easily obtain the data points from the previous scan. It uses the FTDI driver for USB communication, and provides simple command line communication with the TSL.


<h2>Requirements</h2>

- Python version - any version 

- Make sure you have the USB Driver, Download the USB Driver ([CLICK HERE](https://downloads.santec.com/files/downloadfile/6dbd36cd-a29e-4ca0-a894-8ba4e4fdf0c5))

- Once the USB driver is downloaded, make sure that you are able to recognize the TSL-(model) in the System Device Manager.

- Make sure that you have all the necessary DLLs before running the Python Script(s).


<h2>Major Scripts</h2>

1) TSL_550_USB.py  -  This script communicates with the TSL with the use Santec Commands (Command Set-2).
**This script works for TSL-550, TSL-710 devices.

2) TSL_570_USB.py  -  This script communicates with the TSL with the use SCPI Commands.
**This script works for TSL-570 devices.

3) MULTI_TSL_550_USB.py  -  Example script to control two or more TSL devices. This script communicates with the TSL with the use Santec Commands (Command Set-2).
**This script works for TSL-550, TSL-710 devices.

4) SIMUL_MULTI_TSL_550_USB.py  -  Example script to run two or more TSL devices simultaneously at the same time. (uses Santec Commands)
**This script works for TSL-550, TSL-710 devices.


<h2>DLL List</h2>
- Santec_FTDI.dll
- FTD2XX_NET.dll
- FTDI2XX.dll


<h2>Writing your own script [Python Demo]</h2>

1) Make sure that the DLL directory containing all the three DLLs in the same directory as your script.

2) Accessing the DLL,
    ```
    assembly_path = r".\DLL"                                                
    sys.path.append(assembly_path)
    ref = clr.AddReference(r"Santec_FTDI")
    ```

3) Importing the namespace and creating an instance to the main class,
    ```
    import Santec_FTDI as ftdi              # Santec_FTDI is the main namespace
    
    TSL = ftdi.FTD2xx_helper()              # TSL is an instance to the class FTD2xx_helper
    ```

4) To print the list of connected TSLs with their information (mainly Serial Number),
    Using the properties of the DLL (check the README of the DLL directory for more info)
    ```
    for i in range(TSL.numDevices):
        print("Device Index: {}".format(i))
        print("Type: {}".format(TSL.ftdiDeviceList[i].Type))
        print("ID: {:x}".format(TSL.ftdiDeviceList[i].ID))
        print("Location ID: {:x}".format(TSL.ftdiDeviceList[i].LocId))
        print("Serial Number: {}".format(TSL.ftdiDeviceList[i].SerialNumber))
        print("Description: {}".format(TSL.ftdiDeviceList[i].Description))
        print("")
    ```

5) Creating a new instance to initialize and control the specific TSL device by passing the Serial Number as a parameter,
    ```
    TSL = ftdi.FTD2xx_helper('15070009')        # Replace the with the Instrument Serial Number
    ```
    If you have more than one TSL connected,
    ```
    TSL1 = ftdi.FTD2xx_helper('15070009')       # Replace the with the Instrument Serial Number
    TSL2 = ftdi.FTD2xx_helper('18060009')       # Replace the with the Instrument Serial Number
    .
    .
    TSLn = ftdi.FTD2xx_helper('00000000')
    ```

6) Use Query() method for querying or reading from the TSL by passing in the instrument command,
    ```
    TSL.Query('*IDN?')                 # Outputs: SANTEC TSL-(ModelNo.), Serial Number, Version Number
    ```
    
    Moreover, for specifically querying or reading the device identification information, you can use the below method,
    ```
    TSL.QueryIdn()                     # Outputs: SANTEC TSL-(ModelNo.), Serial Number, Version Number
    ```

7) Use Write() method for writing to the TSL,
    ```
    TSL.Write('OP10')                   # Sets the Output power of TSL to 10dBm(or mW)
    ```

8) Reading the Wavelength data from the TSL, <br>

 - For TSL-550, TSL-710, using the GetAllDataPointsFromLastScan_SantecCommand() method,
    ```
    Wavelength = [i/10000 for i in TSL.GetAllDataPointsFromLastScan_SantecCommand()]          
    print('\nWavelength data of TSL: \n', Wavelength)
    
    # Outputs: 
   Wavelength data of TSL:
   [1500.002, 1500.0958, 1500.2018,........, 1600]
    ```
- For TSL-570, using the GetAllDataPointsFromLastScan_SCPICommand() method,
    ```
    Wavelength = [i/10000 for i in TSL.GetAllDataPointsFromLastScan_SCPICommand()]          
    print('\nWavelength data of TSL: \n', Wavelength)
    
    # Outputs: 
  Wavelength data of TSL:
  [1500.002, 1500.0958, 1500.2018,........, 1600]
    ```
  
9) To close the USB connection through the FTDI driver, any future commands sent will throw an exception, as the connection is closed,
    ```
    TSL.CloseUSBConnection()
    ```
<br>
<details>
<summary>About Santec Swept Test System</summary>

### What is STS IL PDL ?
The Swept Test System is the photonic solution by santec Corp. to perform Wavelength 
Dependent Loss characterization of passive optical devices.
It consists of:
- A light source: santec’s Tunable Semiconductor Laser (TSL);
- A power meter: santec’s Multi-port Power Meter (MPM);
   

### For more information on Swept Test System [CLICK HERE](https://inst.santec.com/products/componenttesting/sts)
</details>