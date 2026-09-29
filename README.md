# Static Routing Multi-Site — Cisco Packet Tracer

Simulasi jaringan multi-site yang menghubungkan Head Office (HQ), Cabang A, dan Cabang B menggunakan Cisco Router dan static routing.

Project ini menunjukkan bagaimana beberapa jaringan LAN pada lokasi berbeda dapat saling berkomunikasi menggunakan routing statis melalui koneksi WAN point-to-point.

---

## 📌 Project Overview

Project terdiri dari tiga site:

```text
HQ
Cabang A
Cabang B
```

Masing-masing site memiliki jaringan LAN sendiri.

Routing antar jaringan dilakukan menggunakan **static routing**.

---

## 🗺️ Network Topology

```text
                    +----------------+
                    |       HQ       |
                    | 192.168.10.0/24|
                    +-------+--------+
                            |
                    10.10.1.0/30
                            |
                            |
                    +-------+--------+
                    |    Cabang A    |
                    | 192.168.20.0/24|
                    +----------------+


                    +----------------+
                    |       HQ       |
                    +-------+--------+
                            |
                    10.10.2.0/30
                            |
                            |
                    +-------+--------+
                    |    Cabang B    |
                    | 192.168.30.0/24|
                    +----------------+
```

---

## 🌐 IP Addressing

### HQ

```text
LAN:
192.168.10.1/24

WAN to Cabang A:
10.10.1.1/30

WAN to Cabang B:
10.10.2.1/30
```

### Cabang A

```text
LAN:
192.168.20.1/24

WAN to HQ:
10.10.1.2/30
```

### Cabang B

```text
LAN:
192.168.30.1/24

WAN to HQ:
10.10.2.2/30
```

---

## 📊 Network Summary

| Site | LAN Network | Gateway |
|---|---|---|
| HQ | 192.168.10.0/24 | 192.168.10.1 |
| Cabang A | 192.168.20.0/24 | 192.168.20.1 |
| Cabang B | 192.168.30.0/24 | 192.168.30.1 |

### WAN

| Link | Network | Side A | Side B |
|---|---|---|---|
| HQ ↔ Cabang A | 10.10.1.0/30 | 10.10.1.1 | 10.10.1.2 |
| HQ ↔ Cabang B | 10.10.2.0/30 | 10.10.2.1 | 10.10.2.2 |

---

# ⚙️ Static Routing Configuration

Static route digunakan agar setiap router mengetahui jaringan yang berada pada site lain.

## HQ

HQ memiliki route menuju:

```text
192.168.20.0/24
192.168.30.0/24
```

melalui:

```text
10.10.1.2
10.10.2.2
```

Konsep konfigurasi:

```text
ip route 192.168.20.0 255.255.255.0 10.10.1.2
ip route 192.168.30.0 255.255.255.0 10.10.2.2
```

---

## Cabang A

Cabang A memiliki route menuju:

```text
192.168.10.0/24
192.168.30.0/24
```

melalui:

```text
10.10.1.1
```

Konsep konfigurasi:

```text
ip route 192.168.10.0 255.255.255.0 10.10.1.1
ip route 192.168.30.0 255.255.255.0 10.10.1.1
```

---

## Cabang B

Cabang B memiliki route menuju:

```text
192.168.10.0/24
192.168.20.0/24
```

melalui:

```text
10.10.2.1
```

Konsep konfigurasi:

```text
ip route 192.168.10.0 255.255.255.0 10.10.2.1
ip route 192.168.20.0 255.255.255.0 10.10.2.1
```

---

## 🔍 Routing Table Verification

Verifikasi dilakukan menggunakan:

```text
show ip route
```

Static route ditandai dengan kode:

```text
S
```

Contoh pada HQ:

```text
S 192.168.20.0/24 [1/0] via 10.10.1.2
S 192.168.30.0/24 [1/0] via 10.10.2.2
```

Contoh pada Cabang B:

```text
S 192.168.10.0/24 [1/0] via 10.10.2.1
S 192.168.20.0/24 [1/0] via 10.10.2.1
```

---

## 🔌 Interface Verification

Verifikasi interface dilakukan menggunakan:

```text
show ip interface brief
```

Contoh pada Cabang B:

```text
GigabitEthernet0/0     192.168.30.1    up    up
Serial0/0/0            10.10.2.2       up    up
```

---

# 🧪 Connectivity Testing

Pengujian dilakukan dengan `ping` terhadap gateway dan client pada site lain.

### Test 1

```text
ping 192.168.10.1
```

Result:

```text
Sent     = 4
Received = 4
Lost     = 0 (0% loss)
Average  = 0 ms
```

### Test 2

```text
ping 192.168.20.10
```

Result:

```text
Sent     = 4
Received = 4
Lost     = 0 (0% loss)
Average  = 11 ms
```

### Test 3

```text
ping 192.168.30.10
```

Result:

```text
Sent     = 4
Received = 4
Lost     = 0 (0% loss)
Average  = 17 ms
```

---

## 📊 Testing Summary

| Destination | Result | Packet Loss | Average |
|---|---|---:|---:|
| 192.168.10.1 | Success | 0% | 0 ms |
| 192.168.20.10 | Success | 0% | 11 ms |
| 192.168.30.10 | Success | 0% | 17 ms |

---

## 🛠️ Troubleshooting

Jika komunikasi antar-site gagal, lakukan pemeriksaan:

### 1. Periksa interface

```text
show ip interface brief
```

### 2. Periksa routing table

```text
show ip route
```

### 3. Periksa static route

```text
show running-config
```

### 4. Test koneksi ke next-hop

```text
ping 10.10.1.1
ping 10.10.1.2
ping 10.10.2.1
ping 10.10.2.2
```

### 5. Test koneksi ke remote network

```text
ping 192.168.20.10
ping 192.168.30.10
```

---

## 📂 Repository Structure

```text
static-routing-multisite/
│
├── config/
│   ├── router-hq.txt
│   ├── router-cabangA.txt
│   ├── router-cabangB.txt
│   └── test ping.txt
│
├── topology.png
├── project-4-static-routing.pkt
└── README.md
```

---

## 🎯 Project Objectives

Project ini bertujuan untuk memahami:

- Jaringan multi-site
- LAN dan WAN
- Point-to-point networking
- Subnetting /30
- Static routing
- Next-hop routing
- Routing table
- Cisco Serial Interface
- Connectivity testing
- Network troubleshooting

---

## 🛠️ Tools & Technologies

- Cisco Packet Tracer
- Cisco Router
- Cisco Switch
- IPv4
- Static Routing
- Serial WAN
- Point-to-Point Network
- Subnetting
- ICMP/Ping
- Cisco IOS CLI

---

## 📚 Learning Outcome

Melalui project ini, saya mempraktikkan implementasi jaringan multi-site menggunakan koneksi WAN point-to-point serta konfigurasi static routing untuk menghubungkan jaringan LAN pada HQ, Cabang A, dan Cabang B.
