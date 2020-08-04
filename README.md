# landscape-sysinfo-centos

`landscape-sysinfo-centos` provides an overview of various system metrics on REHL based systems. It was developed as a Message of The Day (MoTD) script but it can be run independently. If the system load is higher than the number of cores, it won't run.

It is a CentOS alternative on the cheap to Ubuntu's [landscape-sysinfo](http://manpages.ubuntu.com/manpages/cosmic/man1/landscape-sysinfo.1.html). It has been tested on CentOS 7 and 8.

It is powered by `bash` and a tiny bit of `awk` for floating-point arithmetic.

## Example output

```bash
System information as of wed jul  8 14:10:12 UTC 2020

Uptime: 12d 23h 56m

Usage of /:   16% of 8,0G (1,3G)  Processes:   383
Memory usage: 61.70% of 12G       System load: 0.62
Swap usage:   0% of 1,0G          SSH session: 1 (10.220.10.100:123456)

IP address for eth0:             10.220.10.101
IP address for docker_gwbridge:  172.18.0.1
IP address for docker0:          172.17.0.1
```

## Deployment

As root, run:

```bash
# enter the /usr/local/bin/ dir
cd /usr/local/bin/

# download landscape-sysinfo-centos
curl --remote-name "https://raw.githubusercontent.com/alct/landscape-sysinfo-centos/master/landscape-sysinfo-centos"

# download landscape-sysinfo-centos hash file
curl --remote-name "https://raw.githubusercontent.com/alct/landscape-sysinfo-centos/master/landscape-sysinfo-centos.sha256"

# verify the script integrity
sha256sum --check "landscape-sysinfo-centos.sha256"

# (if and only if the previous command doesn't throw an error)
# remove the hash file
rm -f "landscape-sysinfo-centos.sha256"

# make landscape-sysinfo-centos executable
chmod +x "landscape-sysinfo-centos"
```

## MoTD

As root, run:

```bash
# deploy the script using the above instructions
# add the script to the tail of /etc/profile.d/custom.sh
echo "/usr/local/bin/landscape-sysinfo-centos" >> "/etc/profile.d/custom.sh"
```

## F.A.Q

#### How can I get rid of this horrible left padding?

When running the script:

```bash
./landscape-sysinfo-centos | sed "s/^  //g"
```

Or, if you want to permanently get rid of it, edit `landscape-sysinfo-centos` as follows:

```bash
main | sed "s/^  //g"
```

#### Does it run on older versions of CentOS/REHL?

No. Older versions of system tools used by `landscape-sysinfo-centos` (e.g. `ss` and `free`) lack modern features that the script relies upon.

#### Why are some IP address' red in the session section?

Your current `ip-address:port` is white, everything else is red. You might want to investigate these red sessions.

## License

[MIT](LICENSE)
