# 前言

---

在 IT 技术迅速发展的今天，虚拟化技术程序软件系统中不可缺少的一层，我们可以从三个角度来看待虚拟化技术。从硬件平台来讲，虚拟化技术用于企业级服务器、个人电脑以及嵌入式系统之中；从用途来讲，虚拟化技术被用于系统资源管理、容错、软硬件维护、系统安全增强、性能提升以及资源共享等领域；从趋势上来讲，其他技术正密切地与虚拟化技术结合，或者依赖于虚拟化技术，并且由于硬件技术的提升，以及虚拟化技术得到广泛的认可，很多厂商提供对虚拟化技术的硬件支持，如 Intel 的 VT-d，AMD VSM 技术等。

虚拟化的含义十分广泛。将任何一种形式的资源抽象成另一种形式的技术都可以叫做虚拟化。在通常的操作系统中本身就存在某种意义上的“虚拟化”：如果把内存看做是一个设备，虚拟内存就是将物理内存虚拟成多个内存空间；进程的概念实际是对物理硬件执行环境的一种抽象，每一个进程都享有一个完成的硬件执行环境，并且与其他进程互相隔离。

相对于进程级的虚拟化，虚拟机是另一个层面的虚拟化，即系统级虚拟化。与虚拟单个进程的执行环境所不同，系统级虚拟化抽象的是整个计算执行需要的环境，抽象出的环境叫做虚拟机，主要包括 CPU、内存与 I/O。

本书分三个章节：虚拟化概述、虚拟化基础和虚拟化管理三大部分，对应于虚拟化的背景、基础理论、实践。第一部分，主要讲述各种虚拟化的相关历史，分类，以期读者对虚拟化技术的历史有一个正确的认知。第二部分，虚拟化基础，主要以 QEMU/KVM 这套完整的虚拟化解决方案为基础，列出了其中知识树，我们不会详细的介绍每一个知识点，而是以 OpenStack 中用到的相关的虚拟化技术来着重讲解某些虚拟化中的基础知识，其余部分我们会以参考链接的形式，对相关知识点给出十分有参考价值的资料，如果您对某个或者某部分我们没有详细讲解的知识点，可以直接查阅我们的给出的参考链接；第三部分，是您在掌握了第二部分的基础知识之后，应该来阅读的部分，里面主要是讲解通过 libvirt 及其命令行工具来管理我们的虚拟机，并且实践我们第二部分中讲到的重点知识。

本书的定位：既试用于虚拟化技术的初学者，也适用于对虚拟化核心技术有一定了解的读者。对于初学者，需要按照本书章节顺序详细阅读，以便对虚拟化技术的全貌以及其基本技术栈有一个清晰的认知；而对于有一点虚拟化基础的读者，可以跳过第一部分，在第二部分和第三部分中挑选自己感兴趣的部分进行深入研究。

