## 7-5 Principles: addressing and routing to mobile users

What is mobility?

> + no mobility: mobile wireless user, using same access point
> 
> + low mobility: mobile user, connecting/disconnecting from network using DHCP.
> 
> + high mobility: mobile user, passing through multiple access point while maintaining ongoing connections (like cellphone)

### Approaches

1. let routing handle it. **too complex**

2. let end-system handle it.
   
   1. indirect routing
   
   2. direct routing

### Registration

> visited user 向 visited network 註冊
> 
> ![./src](./src/7-1.png)

### Indirect routing

// TODO

### Direct routing

// TODO

## 7-6 Mobile IP

> RFC 3344
>
> Mobile IP has many features we've seen
>
> 1. home agents
>
> 2. foreign agents
>
> 3. foreign-agent registration
>
> 4. care-of-addresses
>
> 5. encapsulation
>
> Three components to standard
>
> 1. agent discovery
>
>    > Mobile IP defines the protocols used by a home or foreign agent to advertise its services to mobile nodes, and protocols for mobile nodes to solicit the services of a foreign or home agent.
>
> 2. registration with home agent
>
>    > Mobile IP defines the protocols used by the mobile node and/or foreign agent to register and deregister COAs with a mobile node's home agent
>
> 3. **indirect routing** of datagrams
>
>    > The standard also defines the manner in which datagrams are forwarded to mobile nodes by a home agent, including rules for forwarding datagrams, and several forms of encapsulation (RFC 2003, RFC 2004)

### 7-7 Handing mobility in cellular networks

Like mobile IP, **GSM adopts an indirect routing** approach, first routing the correspondent's call to the mobile user's home network and from there to the visited network. In GSM terminology, the mobile users' home network is referred to as the mobile user's **home public land mobile network** (home PLMN)

### 7-8 Mobility and higher-layer protocols
