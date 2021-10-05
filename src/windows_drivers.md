# Windows Drivers
The Windows operating system does (like most modern OSes) work modularily.\
It does this to achieve an ease of extending the attached hardware. This is 
because new hardware requires DMA / serial access of some sort, so we need to 
have code running in ring 0 to interact with it.

## Driver types
There are a few you can go look up.
* WDM is the old one
* WDF is the new one, it has two subcategories
  * KMDF (kernel mode driver)
  * UMDR (user mode driver)
WDF is built ontop of WDM, and contains a lot of boilerplate wrapper
functionality. While WDM was introduced with Windows 98, and so is very old.
It is however still very much in use today.\

Windows drivers consist of some setup and teardown code, that are split into
different types.\
One type is when the driver is loaded, this will be on system startup or when 
it is installed. The other is when a device is added, i.e. you plug a mouse in.

## .sys files
`.sys` files are just DLLs with a fancy extension.

## Development and Debugging
Debugging kerneldrivers like on linux, is a bit of a hassle. Even more so because
NT is not open source.\
A few tools will help:
* Visual Studio (for driver development)
* OSR Driver loader (for loading your dirty unsigned drivers)
* Windbg (it has a steeper learning curve than radare, but it's the only one that works)
* Ghidra or IDA pro
* WDK (driver development kit, i believe it also comes with symbols)
* Sysinternals package
  * DbgView
  * WinObj
  * ...

For the OS i reccomend [AME](https://ameliorated.info/), it works fine, but 
make sure you test anything on an unmodified windows host.

Now you'll probably want two seperate VMs. One is going to be your development
and building machine (we'll call this DEV), the other is your debug and test 
machine (DBG).

DEV should probably contain Visual studio, IDA (or ghidra) and Windbg.
DBG should contain OSR driver loader, the debugging guest package, and 
sysinternals.

Follow [this](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/setting-up-a-network-debugging-connection-automatically) for enabling debugging
on DBG. It's a bit of a hassle, so i reccomend snapshotting the VM once 
everything is working. You will BSOD...

You can connect them using a Host-Only adapter in virtualbox.

Once you can do remote kernel debugging you're good to move on.

## Reverse engineering
Not a lot of info on this, i reccomend the Windows Internals book, the second 
volume has a chapter about kernel stuff and drivers.\
The book on programming WDM is also good.\
As well as the book on WDF.

IDA is pretty good at recognizing the data types and structs used for WDM. 

See [https://docs.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/writing-a-very-small-kmdf--driver](https://docs.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/writing-a-very-small-kmdf--driver) for driver development
with KMDF. You'll want this to experiment.