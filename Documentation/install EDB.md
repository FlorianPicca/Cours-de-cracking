# Get root

sudo su

# Install dependencies

apt-get install git build-essential pkg-config
apt-get install cmake
apt-get install qt5-qmake libqt5svg5-dev libqt5xmlpatterns5-dev
apt-get install libboost-dev

# Install Capstone

git clone https://github.com/aquynh/capstone.git
cd capstone
./make.sh
./make.sh install
cd ..

# Install EDB

git clone --recursive https://github.com/eteran/edb-debugger.git
cd edb-debugger
mkdir build
cd build
cmake ..
make
make install
