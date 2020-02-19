# EOSIO.cdt (Contract Development Toolkit)
This is a step-by-step guideline to install EOSIO.cdt, which can be done via 2 methods:
* [Install by Prebuilt Binaries](#install-by-prebuilt-binaries)
* [Install by building from Source Code](#Install-by-building-from-source-code)

## Install by Prebuilt Binaries
### Mac OS X Brew Install
```sh
brew tap eosio/eosio.cdt
brew install eosio.cdt
```

### Mac OS X Brew Uninstall
```sh
brew remove eosio.cdt
```

### Debian Package Install
```sh
wget https://github.com/tuan-tl/eosio.cdt/releases/download/v1.7.0/eosio.cdt_1.7.0-1-ubuntu-18.04_amd64.deb
sudo apt install ./eosio.cdt_1.7.0-1-ubuntu-18.04_amd64.deb
```

### Debian Package Uninstall
```sh
sudo apt remove eosio.cdt
```

### RPM Package Install
```sh
wget https://github.com/tuan-tl/eosio.cdt/releases/download/v1.7.0/eosio.cdt-1.7.0-1.el7.x86_64.rpm
sudo yum install ./eosio.cdt-1.7.0-1.el7.x86_64.rpm
```

### RPM Package Uninstall
```sh
sudo yum remove eosio.cdt
```
## Install by building from Source Code
```sh
git clone --recursive https://github.com/tuan-tl/eosio.cdt
cd eosio.cdt
mkdir build
cd build
cmake ..
make -j8
sudo make install
```
## References
* _Install the Contract Dev. Toolkit_, EOS Developers, viewed 19 Feb 2020, <<https://developers.eos.io/welcome/latest/getting-started/development-environment/install-the-CDT>>.