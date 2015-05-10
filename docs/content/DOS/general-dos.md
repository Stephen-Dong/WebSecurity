# 通用的DoS攻击技术 #
General DoS Attack

---

有些DoS攻击能影响许多不同类型的系统，这些DoS攻击成为通用的（generic）DoS攻击。带宽耗用和资源衰竭攻击是典型的通用DoS攻击。由于网络协议的实现一般遵循国际标准，如果网络协议存在缺陷，则遵循该标准的操作系统都会受影响。因此，通用DoS攻击大都利用网络协议进行攻击。    

下面按TCP/IP的协议层次，分别介绍协议攻击技术的实现原理及实例。    

---

## **应用层的DoS攻击** #
应用层DoS攻击的原理是在短期内简历大量合法的TCP连接，当连接数超出了目标服务器的上限，则新的连接将无法建立，从而拒绝为合法用户提供服务。攻击者必须拥有比目标更多的资源，否则相当于让自己停止服务。    

比如，对于只能同时支持10000个在线用户 的服务器，如果攻击者陆续发起20000个连接并保持住成功的连接，则足以让其他用户无法连接服务器。    

为了成功实现应用层的DoS攻击，必须对被攻击的应用作深入分析，**找出其脆弱点并加以利用**。    

---

## **传输层的DoS攻击** #

如果向服务器发送大量伪造IP地址的TCP连接请求，则由于IP地址是伪造的，无法完成最后一次握手，这样在服务器中有大量的半开连接，侵占了服务器的资源。如果在超时时限内的半开连接超过了上限，则服务器将无法响应新的正常连接。这种攻击方式成为SYN Flood攻击。SYN Flood攻击是当前最流行的DoS（拒绝服务攻击）与DDoS（分布是拒绝服务攻击）的方式之一。    

一般来说，如果一个系统（或主机）负荷突然升高甚至失去响应，使用netstat命令能看到大量的SYN RCVD的半连接（数量>500或占总连接数的10%以上），可以认定，这个系统（或主机）遭到了SYN Flood攻击。    

---

## **网络层的DoS攻击** #

在网络层实施DoS攻击主要利用的IP协议的脆弱性。如果**滥用IP广播和组播协议**，将人为导致网络拥塞，从而导致拒绝服务攻击。    

***Smur*f攻击**是最著名的网络层DoS攻击，它以最初发动这种攻击的程序名Smurf来命名。Smurf结合使用了IP欺骗和ICMP回应请求，使大量的ICMP回应报文充斥目标系统。由于目标系统优先处理ICMP消息，目标将因忙于处理ICMP回应报文而无法及时处理其他的网络服务，从而拒绝为合法用户提供正常的系统服务。    

Smurf攻击因为启发大相国而成为最具有破坏行的DoS攻击之一。这种放大效果是向一个网络上的多个系统刚发送定向的ICMP request请求，这些系统连接着对这种求情做出响应。    

Smurf攻击利用了定向广播技术，需要至少三个部分：**攻击者、放大网络（也称为反弹网络或站点）和受害者**。攻击者向放大网络的广播地址发送源地址未造成受害者IP地址的ICMP返回请求分组，这样看起来是受害者的主机发起了这些请求，导致放大网络上所有的系统都将对受害者的系统作出响应。如果一个攻击者给一个拥有100台主机的放大网络发送单个ICMP分组，那么DoS攻击的放大效果将会有100倍。    

Smurf攻击的前提：路由器接收到这个发送给IP广播地址的分组后，会认为这就是广播分组。这样路由器从因特网上接到该分组，会对本地网段中的所有主机进行广播。    

网段中的所有主机都会向**欺骗分组的IP地址**发送echo响应。如果这是一个很大的以太网段，可以会有几百个主机对收到的echo请求进行回复。    

由于多数系统都会尽快的处理ICMP传输信息，Attackers把分组的源地址设置为目标系统的IP地址，目标系统很快就会被大量的echo回应信息吞没，这样轻而易举地就能阻止该系统处理其他任何网络传输，从而拒绝为正常系统提供服务。    