**1. Clone wallet sources**

```
git clone https://github.com/GrumpyBum/harrier.wallet.git
```

**2. Set symbolic link to coin sources at the same level as `src`. For example:**

From Git Clone Directory
```
git submodule add https://github.com/GrumpyBum/harrier.coin.git shc
```

**3. Build**

For Debian Systems
```
mkdir build && cd build && cmake .. && make
```

For Windows Systems (Visual Studio Command Line Suite)
```
md build && cd build && cmake -G "Visual Studio 12" ..
Build Solution in Visual Studio 2013 Community or 2013 Express
```
