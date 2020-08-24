# Summary
- Used by container for network isolation
- Containers are isolated from host using namespaces
![](assets/isolation.png)
# Process namespace
- Container runs its process in it's own namespace
![](assets/process-isolation.png)
# Network namespaces 
![](assets/network-namespaces.png)
# Create network namespace on Linux
![](assets/create-network-namespace.png)
- On running `ip link on host machine, it will show `eth0`
![](assets/host-vs-container-ns.png)
- Check arp table
![](assets/check-arp-table.png)
- As of now, these network namespace doesn't have its own internet i.e. no connectivity
- Let's first establish connectivity between namespaces
    - Two physical machines are connected using physical cable/wifi
    - Two network namespaces are connected using virtual cable
    - This is called `veth` i.e. virtual ethernet 
    ![](assets/veth-red-blue-created.png)
    - Apply veth-red to red namespace
    ![](assets/veth-red-to-red-ns.png)
    - Apply reth-blue to blue namespace
    ![](assets/veth-blue-to-blue-ns.png)
    - Assign ip address to veth-red
    ![](assets/veth-red-ip-address.png)
    - Assign ip address to veth-blue
    ![](assets/veth-blue-ip.png)
    - Start network
    ![](assets/up-veth-connectivities.png)
    - Test connectivity 
    ![](assets/test-veth-connectivity.png)
- Host has no idea on network connectivity between network namespaces
- How to enable communication between many network namespaces
    - just like physical world, we create virtual network inside physical box
    - For physical world you need a switch
    - For network namespace you need virtual switch
    - Create virtual switch withing the hosts and add namespaces to it
    - There are multiple soln to create virtual switch
        - Linux bridge 
        - Open vSwitch
        ![](assets/bridge-network.png)
    - Linux bridge
        - Create linux bridge
        ![](assets/linux-bridge-create.png)
        - Delete existing veth-red-blue link
        ![](assets/delete-veth-red-blue.png)
        - Create veth-red-br bridge link
        ![](assets/veth-red-br-link.png)
        - Create veth-blue-br bridge link
        ![](assets/veth-blue-br-link.png)
        - set  veth-red-br and veth-blue-br to network namespace
        ![](assets/set-bridge-link-to-ns.png) 
        - assign ip address and start link
        ![](assets/bridge-links-assign-ip.png)
        - Multiple network namespace connectivity 
        ![](assets/multiple-namespace-connectivity.png)
- Connectivity from host to network namespaces
    - Assign ip address to virtual bridge network
    ![](assets/assign-ip-to-bridge.png)
    - This network is still private and restricted to host
    ![](assets/private-network-bridge.png)
    - localhost is gateway which connect host network and virtual bridge network
    ![](assets/multi-hosts-bridge-add-ip-route.png)
    - Add `nat` functionality to host using iptable
    ![](Assets/add-nat-to-bridge-network.png)
    - Connectivity 
    ![](assets/connectivity-to-two-bridge-network.png)
- Connectivity from outside world to network namespaces
    ![](assets/from-out-side.png)
    - Add iptable routing for connectivity 
    ![](assets/add-routing.png)
# Docker bridge network
![](assets/docker-bridge-network.png)