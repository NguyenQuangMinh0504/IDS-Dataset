Discription
================
This repo contains dataset and pcap file generated from tool wrk https://github.com/wg/wrk. wrk is a HTTP benchmark tool. The difference between HULK and wrk:
- HULK: Designed for heavy-duty stress testing and DoS attacks. It generates massive volumes of HTTP requests with various techniques to overwhelm web servers.
- wrk: Focuses on light weight, fast, and efficient load testing. It generates a steady stream of HTTP requests to measure a server's performance under realistic workloads.

My website use NGINX server bundled with Gunicorn. The backend is programmed with Djano framework. Host server is Google Cloud Server. Machine type e2-micro: 0.25-2 vCPU (1 shared core), 1 GB memory.

wrk command I used for testing.
```
wrk -t 12 -c 12 -d 300 https://saugau.com/blog
```

The pcap file were generated on my website server using the command
```
sudo tcpdump -w capture.pcap
```
The dataset was generated using tool CICFLowMeter (https://github.com/ahlashkari/CICFlowMeter). This is the same tool used for generated dataset CIC-IDS-2017.



