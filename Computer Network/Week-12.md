# Control Plane

## Traditional Route Algorithms

路由算法分类：
四象限
* static->dynamic
* decentralized->global

1. link state（广播link cost）
   * Dijkstra
2. distance vector（广播path cost--->安全性隐患，black hole）
   * Bellman-Ford

实际运用：
需要考虑到
* scaling
* [autonomy](http://www.youdao.com/w/autonomy/#keyfrom=E2Ctranslation)
引入自治系统(AS, Autonomous System)
* intra-domain
* among-domain

## intra-domain routing algorithm
1. RIP (DV-based)
2. OSPF (LS-based)
	* IP
	* 自定义cost

## among-domain routing algorithm
 
 BGP
* eBGP (External)
* iBGP (Internal)

# SDN
[[Week-11#SDN]]
#SDN 
SDN-APP------Control Plane，功能的抽象
SDN-Controller（Network Operating System）------Control Plane
SDN-controlled switches------Data Plane, OpenFlow

Questions
1. 

