---
tags: 大三
---
# 數據通訊期中小考精選 - 國立中興大學大資訊工程系 Autumn 2020
 
[![](https://img.shields.io/badge/最大連線數-13人%20@Nov%201%2022:51%20-green)](https://i.imgur.com/2H7oqpT.png) [![](https://img.shields.io/badge/%E5%91%BD%E4%B8%AD%E7%8E%87-95%25-yellow)](https://hackmd.io/ifeBQE0vQ4mmgAFZ_WI-oA)

<p id="blessing">祝大家考試順利</p>

<style>
#blessing{
    font-weight: bolder;
    font-size: 2.5em;
    text-align: center;
    margin: 20px 0px;
    animation-name: bling;
    animation-duration: 6s;
    animation-iteration-count: infinite;
}

@keyframes bling{
0%  {color: black;}
10% {color: brown;}
20% {color: red;}
30% {color: orange;}
40% {color: yellow;}
50% {color: green; }
60% {color: blue;}
70% {color: purple;}
80% {color: gray;}
90% {color: gold;}
}

.loader {
  border: 5px solid transparent; /* Light grey */
  border-top-color: #3498db; /* Blue */
  border-radius: 50%;
  width: 60px;
  height: 60px;
  animation: spin 500ms linear infinite;
  margin: 15px auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>

## 第一次小考 9/21
1. Please use an example to explain why link-state routing algorithm would cause router oscillation
![](https://i.imgur.com/eMbFfJm.png)
> 有點像是交通部長說A地會大塞車，結果大家都跑去B地。然後交通部長說B地大塞車，結果大家又跟LS一樣低能跑去A地。
> 
> 一開始時，D 到 A 為 1, B 到 A 也為 1, 因此 C 到 A 隨便走一條，比如經過 B 好了，變成 C -> B -> A。再下一個 run 開始前 B 會通知大家他(B)到 A 很滿(1+e)，所以大家往另一個方向送(圖2)，但是這又導致 D 到 A 很滿，所以 D 又通知大家往別的方向(圖3)，依此類推，不斷振盪。

2. Please show how poisoned reverse solved the problem of count-down-infinity
> 如果 z 會經由 y 繞送到目的端 x，則 z 會通知 y 他到 x 的距離是無限大。
> x
> | \\
> z→y

3. Please compare link-state and distance-vector algorithm in terms of message complexity, convergence speed and robustness.

|          | Message complexity |Convergence |  Robustness   |
| -------- | -------- | -------- | --- |
|    DV    | Exchange between neighbors only.      |    收斂的時間不同(count-to-infinity problem)      |   advertise(發佈) <mark>incorrect link cost</mark>  |
|    LS    | With **n** nodes **E** links, **O(nE)** messages sent     |O(n<sup>2</sup>) algorithm requires O(nE) messages.    |  advertise(發佈) <mark>incorrect path cost</mark>   |

4. Please describe the functions of eBGP and iBGP sessions.

> eBGP session: Exterior BGP 負責 AS 與 AS 間的訊息傳遞
> iBGP session: internal BGP 負責同一個 AS 間的訊息傳遞
> 節錄自維基百科 [邊界閘道器協定](https://zh.wikipedia.org/wiki/%E8%BE%B9%E7%95%8C%E7%BD%91%E5%85%B3%E5%8D%8F%E8%AE%AE)
>
> iBGP和eBGP的區別主要在於轉發路由資訊的行為。例如，從eBGP peer獲得的路由資訊會分發給所有iBGP peer和eBGP peer，但從iBGP peer獲得的路由資訊僅會分發給所有eBGP peer。所有的iBGP peer之間需要全互聯。

5. Please describe how hot potato routing works. 

> router 若要傳送封包給另外一個 AS 的 router，會盡可能減少在內部 AS 所消耗的路徑，亦即<mark>尋找 NEXT-HOP 最小的 router 傳送</mark>
>
> Hot potato routing: choose local gateway that has least intra-domain cost

## 第二次小考 9/28

1. Please explain how Internet uses intra-AS and inter-AS routing to achieve scalable routing.
譯：請解釋 Internet 如何利用 intra-AS 和 inter-AS 的路由實現可擴展的路由。
>
> 如果沒有 AS 只有路由器那麼當路由器數目增多時，交流、計算與儲存送資訊所帶來的負擔將會變得過於高昂。目前世界上有數億個路由器，如果每個路由器都要儲存連結到其他路由器的路由表那會相當巨大，再加上路由器間廣播也會造成很大的負載。
>
> 此時 AS 出現啦
> 
> 將世界上的路由器分成數個AS，AS內的路由器(內部路由器 internal router)只要負責AS內的轉送，AS間的轉送則由AS的邊境路由器(閘道路由器 gateway router)負責，如此一來便可提高路由的規模
> 
> 補充：hierarchical routing saves table size, reduce update traffic

2. Please describe the concept and benefits of hierarchical OSPF.
> OSPF 用來針對 intra-AS 的協定，OSPF 採用 link-state 讓每個 router 都擁有整個網路的拓僕 (所以router要一直瘋狂廣播)
> 
> **benefits:**
> + Reduces the cost(cost for equipment).
> + Makes design easier.
> + Provide more security.
> + Traffic multiplexing.
> 
> **concepts:**
> + Two level hierarchy: Backbone and local area.
> + Area border routers: 每個區域中，都會有一或多台area border router負責將封包繞送到區域之外。
> + Backbone routers: AS內會有唯一一個OSPF區域被設定為Backbone，在AS的其他區域之間繞送資料流。 
> + Boundary routers: 與其他AS連接
> 
> ![](https://i.imgur.com/o6aR5jv.png)
>
> ---
> 瑋哥子安版：
> 
> link-state advertisements only in area; each nodes has detailed area topology; only know direction to nets in other areas
> 
> 總括:將AS分成一個 backbone 和數個 local area,其中在 backbone 中的為backbone router, backbone 與 area 間的為 area border routers, 跟外界AS連接的為 boundary router
>  benefits: only one path get into the AS , security: all OSPF messages authenticated

3. Please describe the generation of forwarding-table entries in a router with BGP and OSPF.

> Step1: 從BGP得知，subnet x 可以經由多條路徑抵達。
>
> Step2: 使用來自OSPF的繞送資訊，判斷前往各閘道的最小成本路徑。
>
> Step3: Hot potato(選擇擁有最小成本的閘道)
>
> Step4: 從 forwarding table 中判斷前往最小成本閘道的介面I。在 forwarding table 中加入(x,I)
>
> 補：大致上是上面那樣。如果針對單一router看的話會是：
> BGP tells router “path to X network goes through 1c router”, OSPF calculate to get to 1c forwarding over outgoing local interface 1
> 譯：BGP 告訴router要前往的router的area(1c), OSPF 計算怎麼過去

4. Please give an example to show how BGP policy affects routing.

> ![](https://i.imgur.com/3r19ujV.png)


5. Please explain the operation of generalized forwarding.

> **Generalized forwarding**: Each packet switch contains a match-plus-action table that is computed and distributed by a remote controller.(Textbook p.383) 譯：每個 packet switch 都包含一個 match-plus-action table，該 table 由 remote controller 計算和分發。
> 
> 
> 匹配＋動作 （Match + action）
> action: 
> + 轉送
> + 丟棄
> + 修改欄位
> 
> Generalized forwarding，是指「通用轉送」，也就是「軟體定義網路的轉送方法」，比如：簡單的轉送、負載平䚘、防火牆、NAT
> 
> 補：可以分成四個部分：
> + Pattern
> + Action: drop, forward, modify, matched packet or send matched packet to controller
> + Priority
> + Counters

![](https://i.imgur.com/YtZTJMg.png)

## 第三次小考 10/5

1. A typical SDN controller consists of three layers: interface, network-wide state management, and communication. Please describe their main functions.

> + interface: 最上面那個, The controller interacts with network-control applications through its "northbound" interface. 
> Note: abstractions API
> 
> + network-wide state management: 記錄轉送的數據吧
> Note: state of networks links, switches services: a distributed database
>
> + communication: data plane 和 control plane 中間那個, communicating between the SDN controller and controlled network devices.

2. Please describe the functions of northbound and southbound API.

> ![](https://i.imgur.com/h6xf8N1.png)
> 1. northbound
> This API allows network-control application to read/write network state and flow tables within the state-management layer.
> 2. southbound
> interacts with network switches "below" via southbound API (怎麼好像是幹話)
> >northbound: 與高階component之間溝通
> >southbound: 與低階component之間溝通
>

3. How does SDN overcome the limitation of traditional routing?

> 透過 SDN controller 統一管理所有的router
>
> SDN 可以做到
> 1. 將同一個到達位址的封包分成不同路徑傳送
> 2. 過濾特定封包(防火牆)
> 3. 實現 NAT (Network Address Translation, 網路位址轉寫)
> 
> Note: by splitting the control plane from the data plane. The logic of the network is moved to a component called the controller that manages devices in the data plane.

4. Please explain the operations of traceroute based on ICMP error messages.

> Traceroute 一次發送三個 TTL 為 1 的 UDP 封包，因為 Timeout 所以路由器會返回 ICMP 逾時的錯誤訊息(type 11 code 0)。接著遞增 TTL 直到收到回傳的封包為 「埠不可達」(type 3 code 3)的 ICMP 報文 (Traceroute 故意使用了一個大於 30000 的埠號，因 UDP 協定規定埠號必須小於 30000)

5. What is the transport protocol of SNMP?

> SNMP 沒有規定，但一般是用 UDP 實作 Port 預設為 161
> SNMP: Simple network management protocol 簡單網路管理協定
>  
## 第四次小考 10/12

1. Where is the link layer implemented?

> On network adapters
> Note: in "adaptor"(aka network interface card) or on a chip

2. Given the data: 101110 and CRC genetrator: 1001, please generate the CRC.

> 101110**000** % 1001 = 011 
CRC = 101110**011**

3. Please list the pros and cons of embedding error correction bits in link-layer frames.

> pros: 可以偵測 bit 錯誤的封包、**節省傳到 transport layer 才重傳所花的時間**
> cons: 浪費封包的空間

4. How does CSMA/CD minimize the collision probability in a broadcast channel?

> 類似團康遊戲「竹筍竹筍蹦蹦出」在傳送封包前先確定是否有其他裝置在使用該線路。
> 
![](https://i.imgur.com/lBWY2Tp.png)

> Note:
> + 先看線路上有沒有正在傳，等到沒有正在傳的再傳
> 萬一遇到碰撞：
> + binary backoff
> + after mth collision,NIC chooses K at random from {0,1,2,... $2^m-1$ }


5. How does CSMA/CD minimize the collision period in a broadcast channel?

> 如果碰撞發生時，第一個偵測到的裝置會通知其他相鄰的裝置「碰撞發生了」
> (如果網路卡在傳輸過程中偵測到來自於其他網路卡的訊號能量，他會停止傳送frame，就不用把無用、毀損的frame完整傳送)

## 第五次小考 10/19
1. Please describe the operation of exponential backoff in Ethernet.

> 送出封包前等待 K\*512 bit times
>
> 碰撞發生 n 次時 K 的範圍從 $0$ \~ $2^n-1$ (n 最大為 10)
>
> 也就說發生第二次碰撞時 K 的值可以是 0\~3
> 發生第三次碰撞時 K 的值是 0 \~ 7
> After collision, wait a random time before trying again(應該寫這個就可以了吧)

2. There are three types of MAC protocols: channel partitioning, random access and taking turns. Please describe their pros and cons
> + Channel partitioning
>   + pros: 可以避免碰撞，並且公平地將頻寬分給N個節點
>   + cons: slot 中會有 idle 發生，浪費頻寬
> + Random access
>   + pros :在沒有發生碰撞的傳輸下，傳輸速率永遠是以通道的全速
>   + cons: 碰撞發生時必需重傳，浪費時間
> + taking turns
>   + pros: 有 M 個點在運作時，每個運作中的節點產出率會接近於 R/M bps
>   + cons: token 傳遞時會浪費時間(造成延遲)

3. Please describe the purpose and operation of ARP
> ARP: Address Resolution Protocol
>
> 當某個裝置要透過 IP 傳送訊息時，因為不知道其他 Network adapter 的 MAC address，所以沒辦法將 layer-2 的 Ethernet frame 的 destination mac address 填入。此時可以藉由 ARP 來完成
>
> ARP 的運作時對其他相鄰的裝置做廣播，尋問他們有沒有人是這一台 IP，如果該裝置 IP 相符會回傳 ARP response 封包讓發送 ARP request 的裝置知道 MAC address
> 
> 解析網路層，透過IP定位來得到MAC
> (林子安你是不是不會打全形逗號)（我會！）
> 
4. Ethernet preamble consists of seven "10101010" bytes followed by one "10101011" byte, What is the purpose of Ethernet preamble?

> 對 clock 做同步

5. How are the entries of an Ethernet switch table generated?
>by self-learning
> 由收到的封包自我學習，switch 會透過接收到的封包的來源MAC address, interface, 和當前時間製作 switch table

## 第六次小考 10/26
1. Please list three sources of frame broadcasting in a Ethernet LAN with switches.

> 協定的種類
> 1. DHCP
> 2. ARP
> 3. switch flood message

2. Please describe the purposes and operations of VLAN.
> Purpose: 普通 LAN 缺乏流量隔離措施(廣播造成的效能下降)，以及在不使用多個 switch 的狀況下分割成多群組
> Operation: 在單一實體 LAN 下，定義多個 VLAN

3. What is the function of VLAN trunk port?
> 兩台switch上各有一個特殊的 port 被設定為 trunk port 以連接兩台 VLAN switch.

4. How can the frame transmission between two VLANs be achieved?
> 設定一台外部路由器讓兩個 VLAN 透過trunk port共用

5. Please list all possible protocols involved in a Web request from a newly connected laptop.
> 1. DHCP 請求分配 IP
> 2. ARP 取得 MAC address
> 3. OSPF,BGP 傳送和計算出到DNS的路徑
> 4. UDP, IP, DNS 將域名解析為 IP 地址
> 5. TCP, HTTP 對網頁伺服器建立 TCP 連線並取得 HTTP 封包
> 
> DHCP, UDP, IP, DNS, ARP, TCP, HTTP, OSPF, BGP

---

期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg

---

<style>
#author{
    border: 1px solid gray;
    border-radius: 10px;
    padding: 10px;
    width: 80%;
    margin: 10px auto;
}
</style>
<div id="author">
<p>111級中興資工</p>
<p>作者：<a href="https://github.com/liao2000">廖柏丞</a>、<a href="https://github.com/minniemu">穆冠蓁</a>、<a title="瑋哥" href="https://github.com/wei-coding">游庭瑋</a>、<a title="aka 中興噁男" href="https://github.com/leonardo-lin">林子安</a></p>
</div>

---

## 2015 年考古題精選 (105級)

1. What is the difference between intra- and inter- AS routing.

> intra-AS 對內，告知 router 到哪一個 gateway 的 path 最短
> inter-AS 對外，告知 AS 需走到哪一個 AS 才能到目的地

2. Please describe an approach of error correction based on parity checking.

> 利用checkbit將該行和該列的1補成偶數個

3. Please describe the drawbacks of channel partitioning

> 部分channel可能沒有全時用到，浪費頻寬
4. Why does slotted ALOHA have better performance than pure ALOHA?
> slotted ALOHA 透過切分時間片來讓各個node分別送一單位資料，不會出現pure ALOHA全部丟上去才發現碰撞又全部重傳的問題(應該是因為pure ALOHA沒有分割時槽，造成她需要注意傳送前後有沒有碰撞，所以效率比較低)
> slotted ALOHA max efficiency: 37%
> pure ALOHA max efficiency: 18%

5. Please list several drawbacks of slotted ALOHA.
> 要同步時間、偵測到碰撞下一個slot才能傳、一直碰撞浪費頻寬

6. In CSMA/CD, how can the collision period be minimized in a broadcast channel?
> 當有某張網卡發現碰撞時會通知其他網卡發生碰撞了

7. Please list the difference between Ethernet hub and switch.
> hub: broadcast, each port intercommunicate.
switch: self-learning, each port independent.

8. How are the entries of a Ethernet switch table generated?
> 1. The switch table is initially empty
> 2. For each incoming frame received on the interface
>    1. the MAC address in the frame's source address field
>    2. the interface from which the frame arrived
>    3. the current time
>
>| Address              | Interface | Time |
>| :------------------- | :-------: | :--: |
>| 01-12-23-34-56       |     2     | 9:39 |
>| AA-BB-CC-DD-EE-FF-00 |     1     | 9:32 |
>| ...                  |    ...    | ...  |
>

9. Please describe the operation of ARP.
> 廣播IP,有相同ip者回報他的MAC

10. What is the function of jam signal in Ethernet?
> 告訴network當前channel已經發生collision，其他設備不要再傳data了

## 2014 年考古題精選 (104級)
1. Please list several drawbacks of slotted ALOHA.
2. Please list the drawbacks of taking-turn MAC protocols.
3. Ethernet switches learn the incoming port of each source MAC address for selective packet forwarding. What is happened if there is a loop in an Ethernet network?
4. How can MPLS virtualize a network as a link?
5. Please describe the benefit of network virtualization of MPLS.
 

---
<!--
## 網路安全考前猜題

![](https://i.imgur.com/wxwQMFP.png)

密碼四大功能:
1. 機密性
2. 資料完整性
3. 可認證性
4. 不可否認性


加密種類: 取代or換位  &&  對稱or非對稱

安全分類
1. 無條件安全: 用完就丟
2. 計算上安全: 無破解價值
3. 可證明安全: RSA

加密法:凱薩，單字母替代，維吉尼亞，恩尼格（輪盤），鑰匙排列，軌道加密，韓國瑜樹(你吸很多)、三角函數、恩尼媽的數、完全二元數

-->

---

期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg
期末筆記請至：https://hackmd.io/s1xj_pBeThWwU-KdfoorBg

---