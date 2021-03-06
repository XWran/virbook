运行在服务器上的实现全虚拟化的VMM\(Virtual Machine Manager，虚拟机管理软件\)层可以控制所有的硬件，其主要功能是为客户机抽象模拟出它所需要的cpu，内存，网卡，磁盘等必备的虚拟设备，为客户机提供一个高度真实的虚拟环境。且并不需要对客户机操作系统做任何改变，客户机上应用程序就可以正常运作，因此客户机会认为自己运行在真实的物理机上。这种类型即全虚拟化。

**全虚拟化的实现**

**全虚拟化又分为：软件辅助的全虚拟化 & 硬件辅助的全虚拟化hvm**

软件辅助全虚拟化的实现图：

![](/images/introduction/virtualization_type/soft_virtualization.png)

如上图，VMM和Guest OS是运行在Ring3用户态的应用程序。理论上来说，用户态的应用程序是没有权限操作硬件的，但是在全虚拟化中，当客户机的内核执行特权指令（实际上是用户态指令）时，由于它并不知道自己所处的是虚拟环境，所以会像发送普通的IO一样发送数据，被Hypervisor拦截，因此会触发异常，然而VMM会捕获这些异常指令并将其虚拟化成只针对虚拟CPU起作用的虚拟特权指令。但是由于当初设计标准x86架构CPU时，并没有考虑到要支持虚拟化技术，所以会存在一部分特权指令运行在Ring 1用户态上，而这些运行在Ring 1上的特权指令并不会触发异常然后再被VMM捕获。从而导致在GuestOS中执行的特权指令直接对HostOS造成了影响\(GuestOS和HostOS没能做到完全隔离\)。

针对这个问题，VMM依赖了二进制翻译技术，即，VMM会对GuestOS中的二进制代码\(运行在CPU中的代码\)进行扫描，一旦发现GuestOS执行的二进制代码中包含有运行在用户态上的特权指令二进制代码时，就会将这些二进制代码翻译成虚拟特权指令二进制代码或者是翻译成运行在核心态中的特权指令二进制代码从而强制的触发异常。这样就能够很好的解决了运行在Ring 1用户态上的特权指令没有被VMM捕获的问题，更好的实现了GuestOS和HostOS的隔离。

总的来说，软件虚拟化是VMM通过二进制翻译来运行虚拟机的技术，因为虚拟机的CPU是通过VMM翻译指令之后在物理机CPU上执行的，而翻译过程也需要占用CPU时间，所以软件虚拟化的执行效率比较低，但软件虚拟化的一个好处是可以跨CPU架构运行虚拟机，比如X86的CPU可以通过软件虚拟化技术运行ARM架构的虚拟机，也可以运行power架构的虚拟机，安卓SDK组件包含的调试应用的虚拟机即用了软件虚拟化技术，QEMU是软件虚拟化技术的一个典型应用。

硬件辅助全虚拟化的实现图：

![](/images/introduction/virtualization_type/hvm.png)

硬件虚拟化是通过硬件的协助，比如使用支持虚拟化功能的CPU进行支撑，如上图，硬件辅助的全虚拟化方式中GuestOS直接运行在Ring0，由VMM充当宿主机，直接运行在硬件平台上。VMM会将虚拟化模块加载到Host的内核中。从而提高虚拟机运行效率的技术，Intel CPU的VT和AMD CPU的AMD-V都是在CPU上实现的硬件虚拟化技术，在虚拟机使用CPU的时候，虚拟机管理软件VMM将CPU切换到一个硬件虚拟化专用的运行模式，这样可以让虚拟机相对于直接运行在物理CPU上，可以执行绝大部分指令，只有少部分影响物理机的指令会被拦截，比如关机这样的指令，不能因为一台虚拟机执行了关机指令而把宿主机关闭了，所以被拦截的指令需要交给VMM进行虚拟化处理，令其只对虚拟机生效，KVM是使用硬件虚拟化技术的一个典型应用。M通过在HostOS内核中加载KVM Kernel Module来将HostOS转换成为一个VMM。

查看系统是否可以支持硬件辅助虚拟机化，可以使用如下命令：

对于Intel CPU:

```
# grep "vmx" /proc/cpuinfo
```

对于AMD CPU:

```
# grep "svm" /proc/cpuinfo
```

