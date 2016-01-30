# Docker par l'exemple

On récupère l'image Docker `debian`:
```
# docker pull debian
latest: Pulling from debian
77e39ee82117: Pull complete
5eb1402f0414: Pull complete
debian:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
Digest: sha256:d2ea9df44c61c1e3042c20dd42bf57a86bd48bb428e154bdd1d1003fad6810a4
Status: Downloaded newer image for debian:latest
```


```
# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
debian              latest              5eb1402f0414        4 days ago          125.1 MB
```

```
# docker history debian
IMAGE               CREATED             CREATED BY                                      SIZE
5eb1402f0414        4 days ago          /bin/sh -c #(nop) CMD ["/bin/bash"]             0 B
77e39ee82117        4 days ago          /bin/sh -c #(nop) ADD file:e5a3d20748c5d3dd5f   125.1 MB
```

```
# docker run debian
# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
f908266cc831        debian:latest       "/bin/bash"         6 seconds ago       Exited (0) 5 seconds ago                       naughty_wozniak
```

Si on demarre un container à partir de cette image `debian` et que l'on crée un fichier:
```
# docker run -i -t debian /bin/bash
root@4276f7423f82:/# echo "file created in container" > in_container.txt
```

On le retrouvera dans `/var/lib/docker/aufs/diff/<id_container>/`:
```
# cat /var/lib/docker/aufs/diff/4276f7423f82af390391e94b73b94fae97fc3bc47985668677834822fe2be62c/in_container.txt
file created in container
```

```
# docker logs 4276f7423f82
root@4276f7423f82:/# echo "file created in container" > in_container.txt
```


```
# more /var/lib/docker/containers/f908266cc8314f3791673b5b4811c63b8a56deb0d2aa86619751b00c99dc11af/config.json | json_pp
{
   "Volumes" : {},
   "MountLabel" : "",
   "State" : {
      "Error" : "",
      "Running" : false,
      "Paused" : false,
      "Pid" : 0,
      "Dead" : false,
      "ExitCode" : 0,
      "Restarting" : false,
      "OOMKilled" : false,
      "FinishedAt" : "2016-01-30T21:51:49.888590927Z",
      "StartedAt" : "2016-01-30T21:51:49.840315034Z"
   },
   "VolumesRW" : {},
   "AppArmorProfile" : "",
   "RestartCount" : 0,
   "Created" : "2016-01-30T21:51:49.488250882Z",
   "AppliedVolumesFrom" : null,
   "Args" : [],
   "LogPath" : "/var/lib/docker/containers/f908266cc8314f3791673b5b4811c63b8a56deb0d2aa86619751b00c99dc11af/f908266cc8314f3791673b5b4811c63b8a56deb0d2aa86619751b00c99dc11af-json.log",
   "ID" : "f908266cc8314f3791673b5b4811c63b8a56deb0d2aa86619751b00c99dc11af",
   "NetworkSettings" : {
      "IPv6Gateway" : "",
      "Ports" : null,
      "IPPrefixLen" : 0,
      "PortMapping" : null,
      "LinkLocalIPv6Address" : "",
      "LinkLocalIPv6PrefixLen" : 0,
      "Gateway" : "",
      "IPAddress" : "",
      "Bridge" : "",
      "GlobalIPv6Address" : "",
      "GlobalIPv6PrefixLen" : 0,
      "MacAddress" : ""
   },
   "Image" : "5eb1402f041415f4d72ec331c9388e4981420dfe88ef4e9bdf904d4687e4de09",
   "Config" : {
      "ExposedPorts" : null,
      "Image" : "debian",
      "OpenStdin" : false,
      "Cpuset" : "",
      "MemorySwap" : 0,
      "OnBuild" : null,
      "WorkingDir" : "",
      "StdinOnce" : false,
      "NetworkDisabled" : false,
      "Cmd" : [
         "/bin/bash"
      ],
      "AttachStderr" : true,
      "AttachStdout" : true,
      "CpuShares" : 0,
      "Volumes" : null,
      "Domainname" : "",
      "MacAddress" : "",
      "Entrypoint" : null,
      "Env" : null,
      "AttachStdin" : false,
      "PortSpecs" : null,
      "User" : "",
      "Memory" : 0,
      "Tty" : false,
      "Labels" : {},
      "Hostname" : "f908266cc831"
   },
   "Path" : "/bin/bash",
   "HostsPath" : "/var/lib/docker/containers/f908266cc8314f3791673b5b4811c63b8a56deb0d2aa86619751b00c99dc11af/hosts",
   "Driver" : "aufs",
   "ExecDriver" : "native-0.2",
   "HostnamePath" : "/var/lib/docker/containers/f908266cc8314f3791673b5b4811c63b8a56deb0d2aa86619751b00c99dc11af/hostname",
   "UpdateDns" : false,
   "ResolvConfPath" : "/var/lib/docker/containers/f908266cc8314f3791673b5b4811c63b8a56deb0d2aa86619751b00c99dc11af/resolv.conf",
   "Name" : "/naughty_wozniak",
   "ProcessLabel" : ""
}
```
