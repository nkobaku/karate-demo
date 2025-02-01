# JDK & JVM Implementations

## Overview

This document provides a **comprehensive list** of various **JDK (Java Development Kit) implementations** and **JVM (Java Virtual Machine) implementations**. It categorizes them based on their base JDK/JVM, owners, and key features.

---

## **JDK Implementations**

These are different **Java Development Kits (JDKs)** based on **Oracle OpenJDK** or other implementations.

### **JDK Implementations Overview**
Below is a **visual representation** of JDK implementations categorized by their base JDK:

```
                Oracle OpenJDK
                     │
  ┌──────────────────┴──────────────────┐
  │                 │                    │
Amazon Corretto  Azul Zulu          BellSoft Liberica
   │               │                      │
  (AWS)          (Enterprise)            (Embedded/IoT)

Other Notable JDKs:
- Eclipse Temurin
- IBM Semeru (Uses OpenJ9)
- Microsoft OpenJDK
- SAP Machine
- GraalVM (Community & Enterprise)
- Alibaba Dragonwell
- Red Hat OpenJDK
- Tencent Kona JDK
- Huawei BiSheng JDK
- Pivotal Spring JDK
```

### **JDK Implementations Table**

| JDK Implementation     | Base JDK                | Owner              | Key Features                                 |
| ---------------------- | ----------------------- | ------------------ | -------------------------------------------- |
| **Oracle JDK**         | Oracle OpenJDK          | Oracle             | Commercial Support, Performance Enhancements |
| **Amazon Corretto**    | Oracle OpenJDK          | Amazon             | Free LTS, AWS Optimized                      |
| **Azul Zulu**          | Oracle OpenJDK          | Azul Systems       | TCK-certified, Security Updates              |
| **BellSoft Liberica**  | Oracle OpenJDK          | BellSoft           | OpenJFX, LTS, IoT Support                    |
| **Eclipse Temurin**    | Oracle OpenJDK          | Eclipse Foundation | Open-source, Security Focused                |
| **IBM Semeru**         | Oracle OpenJDK + OpenJ9 | IBM                | Uses OpenJ9, Low Memory Footprint            |
| **Microsoft OpenJDK**  | Oracle OpenJDK          | Microsoft          | Azure Optimized, Enterprise Security         |
| **SAP Machine**        | Oracle OpenJDK          | SAP                | Optimized for SAP Cloud & ERP                |
| **GraalVM Community**  | GraalVM                 | Oracle             | JIT + AOT, Polyglot Support                  |
| **GraalVM Enterprise** | GraalVM                 | Oracle             | Enterprise Performance, Advanced Security    |
| **Azul Zing**          | Oracle OpenJDK          | Azul Systems       | Low Latency, Real-Time Performance           |
| **Alibaba Dragonwell** | Oracle OpenJDK          | Alibaba            | Optimized for Alibaba Cloud & Big Data       |
| **Red Hat OpenJDK**    | Oracle OpenJDK          | Red Hat            | Enterprise Support by Red Hat                |
| **Tencent Kona JDK**   | Oracle OpenJDK          | Tencent            | Optimized for Tencent Cloud Services         |
| **Huawei BiSheng JDK** | Oracle OpenJDK          | Huawei             | Optimized for Huawei Kunpeng Server          |
| **Pivotal Spring JDK** | Oracle OpenJDK          | Pivotal (VMware)   | Optimized for Spring Boot Applications       |

---

## **JVM Implementations**

The **Java Virtual Machine (JVM)** executes Java bytecode. Different vendors provide their own JVM implementations, optimized for various needs.

### **JVM Implementations Overview**
Below is a **visual representation** of different JVM implementations:

```
                     JVM Implementations
                           │
  ┌───────────────┬───────────────┬───────────────┐
  │               │               │               │
HotSpot JVM    OpenJ9         GraalVM JVM      Others
  │               │               │               │
 (Oracle)       (IBM/Eclipse)  (Oracle)   (Zing, Dalvik, ART)
```

### **JVM Implementations Table**

| JVM Implementation        | Developer     | Key Features                          |
| ------------------------- | ------------- | ------------------------------------- |
| **HotSpot JVM**           | Oracle        | Adaptive JIT, G1/ZGC GC               |
| **OpenJ9**                | Eclipse (IBM) | Low memory, fast startup              |
| **GraalVM JVM**           | Oracle        | JIT + AOT, Polyglot                   |
| **Dalvik JVM**            | Google        | Legacy Android runtime                |
| **ART (Android Runtime)** | Google        | AOT Compilation                       |
| **Zing JVM**              | Azul Systems  | C4 GC, real-time performance          |
| **Microsoft JVM**         | Microsoft     | Optimized for Azure                   |
| **PERC JVM**              | Aonix         | Real-time Java for aerospace/military |
| **Avian JVM**             | Independent   | Lightweight JVM for embedded systems  |
| **Maxine JVM**            | Oracle Labs   | Research-focused JVM                  |

---

## **Conclusion**

- **JDKs** are complete development environments, while **JVMs** execute Java programs.
- **Most JDKs are based on Oracle OpenJDK**, but vendors customize them for performance, security, and cloud integration.
- **JVMs differ in optimizations**, with **HotSpot, OpenJ9, and GraalVM** being the most widely used.

This document serves as a reference for choosing the right **JDK** and **JVM** based on specific needs.
