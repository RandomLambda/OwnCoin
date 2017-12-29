
### Preparation

3. Buy one or two Ubuntu-based dedicated servers (at least 2Gb of RAM) for seed nodes.

### First step. Give a name to your coin

**Good name must be unique.** Check uniqueness with [google](http://google.com) and [Map of Coins](mapofcoins.com) or any other similar service.

Name must be specified twice:

**1. in file src/CryptoNoteConfig.h** - `CRYPTONOTE_NAME` constant

Example: 
```
const char CRYPTONOTE_NAME[] = "furiouscoin";
```

**2. in src/CMakeList.txt file** - set_property(TARGET daemon PROPERTY OUTPUT_NAME "YOURCOINNAME**d**")

Example: 
```
set_property(TARGET daemon PROPERTY OUTPUT_NAME "furiouscoind")
```

### Second step. Emission logic 

**4. Block reward formula -> noch prüfen o diese angepasst werden muss!**

In case you are not satisfied with CryptoNote default implementation of block reward logic you can also change it. The implementation is in `src/CryptoNoteCore/Currency.cpp`:
```
bool Currency::getBlockReward(size_t medianSize, size_t currentBlockSize, uint64_t alreadyGeneratedCoins, uint64_t fee, uint64_t& reward, int64_t& emissionChange) const
```

### Third step. Networking

**1. Default ports for P2P and RPC networking** (src/CryptoNoteConfig.h)

P2P port is used by daemons to talk to each other through P2P protocol.
RPC port is used by wallet and other programs to talk to daemon.

It's better to choose ports that aren't used by other software or coins. See known TCP ports lists:

* http://www.speedguide.net/ports.php
* http://www.networksorcery.com/enp/protocol/ip/ports00000.htm
* http://keir.net/portlist.html

Example:
```
const int P2P_DEFAULT_PORT = 17236;
const int RPC_DEFAULT_PORT = 18236;
```

**3. Seed nodes** (src/CryptoNoteConfig.h)

Add IP addresses of your seed nodes.

Example:
```
const std::initializer_list<const char*> SEED_NODES = {
  "111.11.11.11:17236",
  "222.22.22.22:17236",
};
```

### Sixth step. Genesis block

**1. Build the binaries with blank genesis tx hex** (src/CryptoNoteConfig.h)

You should leave `const char GENESIS_COINBASE_TX_HEX[]` blank and compile the binaries without it.

Example:
```
const char GENESIS_COINBASE_TX_HEX[] = "";
```


**2. Start the daemon to print out the genesis block**

Run your daemon with `--print-genesis-tx` argument. It will print out the genesis block coinbase transaction hash.

Example:
```
furiouscoind --print-genesis-tx
```


**3. Copy the printed transaction hash** (src/CryptoNoteConfig.h)

Copy the tx hash that has been printed by the daemon to `GENESIS_COINBASE_TX_HEX` in `src/CryptoNoteConfig.h`

Example:
```
const char GENESIS_COINBASE_TX_HEX[] = "013c01ff0001ffff...785a33d9ebdba68b0";
```


**4. Recompile the binaries**

Recompile everything again. Your coin code is ready now. Make an announcement for the potential users and enjoy!


## Building CryptoNote 

### On *nix

Dependencies: GCC 4.7.3 or later, CMake 2.8.6 or later, and Boost 1.55.

You may download them from:

* http://gcc.gnu.org/
* http://www.cmake.org/
* http://www.boost.org/
* Alternatively, it may be possible to install them using a package manager.

To build, change to a directory where this file is located, and run `make`. The resulting executables can be found in `build/release/src`.

**Advanced options:**

* Parallel build: run `make -j<number of threads>` instead of `make`.
* Debug build: run `make build-debug`.
* Test suite: run `make test-release` to run tests in addition to building. Running `make test-debug` will do the same to the debug version.
* Building with Clang: it may be possible to use Clang instead of GCC, but this may not work everywhere. To build, run `export CC=clang CXX=clang++` before running `make`.

### On Windows
Dependencies: MSVC 2013 or later, CMake 2.8.6 or later, and Boost 1.55. You may download them from:

* http://www.microsoft.com/
* http://www.cmake.org/
* http://www.boost.org/

To build, change to a directory where this file is located, and run theas commands: 
```
mkdir build
cd build
cmake -G "Visual Studio 12 Win64" ..
```

And then do Build.
Good luck!
