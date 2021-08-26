***Handling Error in UERANSIM Installation ***

1. Error: `make` failed, it is due to lower version of default `cmake` version
2. Remove `cmake` using `sudo apt remove cmake`
3. Install `cmake` using `sudo snap install cmake --classic`
4. Now build using `make`
