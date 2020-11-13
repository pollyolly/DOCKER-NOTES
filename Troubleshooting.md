Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```



Install Updated WSL 2 for x64.
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
```
Force Removing Docker Installation
```
sudo mv /var/lib/dpkg/info/docker.* /tmp/
sudo dpkg --remove --force-remove-reinstreq docker.io
sudo apt-get remove docker.io
sudo apt-get autoremove && sudo apt-get autoclean
```
