# Uninstall Python3.6 on Ubuntu

1. Remove the repo:
```
sudo add-apt-repository --remove ppa:fkrull/deadsnakes
```
2. Refresh apt cache:
```
sudo apt-get update
```
3. Remove the package:
```
sudo apt-get remove --purge python3.6
```
