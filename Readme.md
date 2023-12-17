Discription
================
This repo contains dataset and pcap file generated from tool wrk https://github.com/wg/wrk. wrk is a HTTP benchmark tool. The difference between HULK and wrk:
- HULK: Designed for heavy-duty stress testing and DoS attacks. It generates massive volumes of HTTP requests with various techniques to overwhelm web servers.
- wrk: Focuses on light weight, fast, and efficient load testing. It generates a steady stream of HTTP requests to measure a server's performance under realistic workloads.

My website use NGINX server bundled with Gunicorn. The backend is programmed with Djano framework. Host server is Google Cloud Server. Machine type e2-micro: 0.25-2 vCPU (1 shared core), 1 GB memory.


The dataset was generated using tool CICFLowMeter (https://github.com/ahlashkari/CICFlowMeter). This is the same tool used for generated dataset CIC-IDS-2017.

Steps for generating dataset
================
1. Collect pcap file in web server
```
sudo tcpdump -w capture.pcap
```
2. Perform attacks with command
```
wrk --header "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.0.0 Safari/537.36" --header "Referer: https://saugau.com/" https://saugau.com/blog -t 48 -d 1800 -c 100
```
3. Pass the pcap file through CICFLowMeter. Flow timeout: 120000000, Activity Timeout: 5000000 (Default). The result file named capture.pcap_Flow.csv for single machine attack and capture_multiple.pcap_Flow.csv for multiple machine attack.

4. Label file 

Web server IP: 10.128.0.42 

capture.pcap_Flow.csv: Single machine attack

Src IP attack: 42.114.37.222 (Localhost) (Macbook Pro M1)



capture_multiple.pcap_Flow.csv: Multiple machine attack

Attack machine: Google Cloud E2 micro instances. List of instances:

|name| zone| internal ip| external ip|
|---|---|---|---|
|test-instance-1| us-west1-b| 10.138.0.2| 34.105.126.61|
|test-instance-2| europe-north1-a| 10.166.0.2| 35.228.215.90|
|test-instance-3| europe-west2-c| 10.154.0.2| 35.197.230.23|
|test-instance-4| europe-west9-a| 10.200.0.2| 34.155.133.57|


Suggest Labeling: Mark all IP flow with Src IP or Dst IP equal to attack IP. See Label-Suggesting.ipynb and Label-Multiple.ipynb for details

Notes
=================
I use file Wednesday-workingHours.pcap_ISCX.csv from CIC-IDS-2017 dataset as comparison. This file have a duplicate feature " Fwd Header Length.1" which is a duplicate of "Fwd Header Length".
Aside from that, Wednesday-workingHours.pcap_ISCX.csv and capture.pcap_Flow.csv have the same set of features.

Due to large size, The csv for muliple machine and raw pcap file can be found at https://drive.google.com/drive/folders/1xqokKb_GdDOZz4YbZk6Lro8IFhxB9gL0?usp=drive_link

Further
=================
1. I dont think network intrusion can be detected solely on package statistics. As such, I think NSL-KDD, CIC-IDS-2017 and TON_IOT_Network can not be used for NIDS in general and DoS, DDoS specifically. I think NIDS should be a combination of time series problem, classfication abnormaly detection, and the data source not only include network related, but also processes and log inspection...
2. Data leak happens in the dataset I collected. I saw the same happened with KDD99's Dataset and imo it should happen with CIC-IDS-2017 and TON_IOT_Network as well. So high accuracy proves nothing and the dataset can not be used in real life.

Future work
=================
I try to use https://github.com/grafov/hulk for HULK attack but I can not make it to work with Macbook Arm and Python2 and Im too lazy to debug. I would look into https://github.com/Hyperclaw79/HULK-v3 and https://github.com/jseidl/GoldenEye if I have time.





