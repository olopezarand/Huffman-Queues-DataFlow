# Lab2 – Huffman, Queues, and Data Flow

**Author:** Abraham Lopez  
**Course / Institution:** UTSA — Information Theory / Networking Lab  
**Date:** Fall 2025

## Brief overview
This repository contains the final report for Lab 2: *Huffman, Queues, and Data Flow*.  
The work combines theoretical exercises (Huffman coding and channel capacity) with hands-on experiments on Kali Linux and Metasploitable 2, covering compression, AES/RSA encryption, throughput measurement with `nc`/`dd`, and network delay simulation with `tc` and `iperf3`.

## Files
- `Lopez_Abraham_Lab2.pdf` — Final lab report (contains written solutions, command outputs, screenshots, analysis, conclusion, and appendix). This is the single file used for grading/submission.

## How to view
Download or open `Lopez_Abraham_Lab2.pdf` in any PDF viewer. The report includes:
- Part 1 — Written problems (Huffman coding A–C, channel capacity D)
- Part 2 — Hands-on (Compression, AES, RSA, Channel Capacity in practice, Data Flow)
- Analysis, Conclusion, and Appendix with screenshots and captions
- Throughput calculations and commands used

## Tools & commands (summary)
Key tools used during the lab:
- `gzip`, `dd`, `nc`, `openssl`, `iperf3`, `ping`, `tc qdisc`, `stat`, `ls`, `cat`

Example commands used (see PDF for full context):
```bash
# compression demo
echo "AAAAABBBBBCCCCCDDDDD" > test.txt
gzip -k test.txt

# AES (symmetric)
openssl aes-256-cbc -salt -pbkdf2 -in sym.txt -out sym.enc
openssl aes-256-cbc -d -salt -pbkdf2 -in sym.enc -out sym.dec

# RSA (asymmetric) - OpenSSL 3.x
openssl genrsa -out private.pem 2048
openssl rsa -in private.pem -pubout -out public.pem
openssl pkeyutl -encrypt -pubin -inkey public.pem -in asym.txt -out asym.enc
openssl pkeyutl -decrypt -inkey private.pem -in asym.enc -out asym.dec

# throughput test (Kali -> Metasploitable)
time dd if=/dev/zero bs=1M count=50 | nc <METASPLOITABLE_IP> 12345

# simulate delay
sudo tc qdisc add dev eth0 root netem delay 200ms
iperf3 -c <METASPLOITABLE_IP>
sudo tc qdisc del dev eth0 root
