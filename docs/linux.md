#WSL - Windows Linux Subsystem

Some tips etc on how to use Windows WSL

## Stop resolv.conf updating with local DNS
If there is an issue connecting to some stuff defined in your WSL it is most likely DNS. To stop auto generation of **resolv.conf** you need to create a file **/etc/wsl.conf** and add the following, 

``` 
[network] 
generateResolvConf = false
``` 

Then in a windows cmd line session,

``` wsl --shutdown ```

This shuts down wsl properly and saves any changes. 
