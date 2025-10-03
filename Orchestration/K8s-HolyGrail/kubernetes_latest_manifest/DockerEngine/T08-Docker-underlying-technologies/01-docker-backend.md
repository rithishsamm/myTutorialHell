**Docker is written in Go and takes advantage of several features of the Linux kernel to deliver its functionality**

## Namespaces

- Docker uses a technology called namespaces to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container.

- These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

- Docker Engine uses namespaces such as the following on Linux:
  - **The pid namespace:** Process isolation (PID: Process ID).
  - **The net namespace:** Managing network interfaces (NET: Networking).
  - **The ipc namespace:** Managing access to IPC resources (IPC: InterProcess Communication).
  - **The mnt namespace:** Managing filesystem mount points (MNT: Mount).
  - **The uts namespace:** Isolating kernel and version identifiers. (UTS: Unix Timesharing System).


## Control groups
Docker Engine on Linux also relies on another technology called control groups (cgroups). A cgroup limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resources to containers and optionally enforce limits and constraints. For example, you can limit the memory available to a specific container.


## Union Filesystem
Union file systems, or UnionFS, are file systems that operate by creating layers, making them very lightweight and fast. Docker Engine uses UnionFS to provide the building blocks for containers. Docker Engine can use multiple UnionFS variants, including AUFS, btrfs, vfs, and DeviceMapper.


## Container format
Docker Engine combines the namespaces, control groups, and UnionFS into a wrapper called a container format. The default container format is libcontainer. In the future, Docker may support other container formats by integrating with technologies such as BSD Jails or Solaris Zones.