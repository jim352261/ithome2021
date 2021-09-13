# Day6-控制器是在控什麼 controller說明

## pod的管理與控制
對k8s來說,pod是k8s最小的元件,但是當我們在使用時,通常不會直接對pod進行部屬或操作,而是會透過控制器(controller)來對pod進行操作.
雖說叫做控制器,不過並非有個元件叫做控制器,而是多個對象,下面是主要的控制器:
1. deployment:
  告訴k8s如何去建立pod的實體,透過ReplicaSets來控制pod更新,rolling-update等行為
2. ReplicaSets:
  算是替代了ReplicationController的工作(官網建議現在以 ReplicaSet 的 Deployment來建立副本管理機制 ),能夠確保pod的數量以及
5. DaemonSet
6. Jobs
7. StatefulSets
