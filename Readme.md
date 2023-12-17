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

Steps for generating dataset
================
1. Collect pcap file in web server
```
sudo tcpdump -w capture.pcap
```
2. Perform attacks with command
```
wrk --header "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36" --header "Referer: https://saugau.com/" https://saugau.com/blog -t 48 -d 300 -c 100
```
3. Pass the pcap file through CICFLowMeter. Flow timeout: 120000000, Activity Timeout: 5000000 (Default). The result file named capture.pcap_Flow.csv
4. Label file capture.pcap_Flow.csv
Src IP attack: 42.114.37.222
Web server IP: 10.128.0.42
Suggest Labeling: Mark all IP flow with Src IP or Dst IP equal to attack IP. See Label-Suggesting.ipynb for details

Notes
=================
I use file Wednesday-workingHours.pcap_ISCX.csv from CIC-IDS-2017 dataset as comparison. This file have a duplicate feature " Fwd Header Length.1" which is a duplicate of "Fwd Header Length".
Aside from that, Wednesday-workingHours.pcap_ISCX.csv and capture.pcap_Flow.csv have the same set of features.
The raw pcap file can be found at https://drive.google.com/drive/folders/1xqokKb_GdDOZz4YbZk6Lro8IFhxB9gL0?usp=drive_link





