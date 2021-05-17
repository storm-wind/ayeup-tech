### How to Update to a newer version


```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install
which aws
ls -l /usr/local/bin/aws
./aws/install --bin-dir /usr/bin --install-dir /usr/bin/aws-cli --update
```

If old version hangs around, look for the aws binary and then do the following and confirm the binary version

```
find / -name aws
/usr/bin/aws --version
rm -f /usr/bin/aws
./aws/install --bin-dir /usr/bin --install-dir /usr/bin/aws-cli --update
```

Job done so when you run `aws --version` it should return the correct version.



### Switch between AWS CLI Profiles

To switch between profiles you can add

```
--profile <profile name>
```

Or just 

```
export AWS_PROFILE=<profile name>

``` 
for multiple commands.

