# Haproxy
%title: HAPROXY - Algo
%author: xavki




-> HAPROXY :  LB & algo <-
=========



<br>
* 6 algos



* gérer la répartition en fonction du service

-------------------------------------------------------------------


-> Round Robin <-



* une fois sur le nombre de serveurs dans le pool



```

backend load1
    balance roundrobin
    server srv1 172.17.0.2:80
    server srv2 172.17.0.3:80
    server srv3 172.17.0.4:80

```

------------------------------------------------------------------


-> Round Robin Weighted <-


* idem mais avec pondération

* weight : pondération (0 aucun traffic = disabled)

```

backend load1
    balance roundrobin
    server srv1 172.17.0.2:80  weight 1
    server srv2 172.17.0.3:80  weight 2
    server srv3 172.17.0.4:80  weight 3

```

* une fois sur 6 le serveur 1

* 2 fois sur 6 le serveur 2

* 3 fois sur 6 (1/2) le serveur 3


------------------------------------------------------------------


-> Leastconn <-



* prend en compte le serveur qui a le plus de connexions en cours

* utile pour les bases de données par exemple


```

backend load1
    balance leastconn
    server srv1 172.17.0.2:80
    server srv2 172.17.0.3:80
    server srv3 172.17.0.4:80

```

------------------------------------------------------------------


-> Leastconn weighted <-



* idem avec pondération



```

backend load1
    balance leastconn
    server srv1 172.17.0.2:80 weight 1
    server srv2 172.17.0.3:80 weight 2
    server srv3 172.17.0.4:80 weight 3

```

--------------------------------------------------------------------



-> hash uri <-



* utilisé pour les caches

* si des images sont déjà chargés sur un serveur on lui envoi le trafic



```

backend load1
    balance uri
    server srv1 172.17.0.2:80 
    server srv2 172.17.0.3:80
    server srv3 172.17.0.4:80

```
Rq : weight possible


--------------------------------------------------------------------



-> first connexion <-



* dans l'ordre de la liste

* pour économiser des ressources et s'adapter réellement à la charge (machine up pour déborement)



```

backend load1
    balance first
    server srv1 172.17.0.2:80 maxconn 5
    server srv2 172.17.0.3:80 maxconn 5
    server srv3 172.17.0.4:80 maxconn 5

