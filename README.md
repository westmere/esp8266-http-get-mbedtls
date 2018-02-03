# esp8266-http-get-mbedtls

git clone --recursive https://github.com/westmere/esp8266-http-get-mbedtls.git

mkdir build

cd build

cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_BUILD_TYPE=Release -G "Eclipse CDT4 - Unix Makefiles" -DCMAKE_ECLIPSE_VERSION=4.6 ../

make all

./esptool.sh /dev/ttyUSB0
