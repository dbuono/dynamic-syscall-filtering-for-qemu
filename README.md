# dynamic-syscall-filtering-for-qemu
Quick Emulator (QEMU) is a generic and open source machine emulator and virtualizer. It has become a de facto tool in industry for virtualization in cloud. Therefore, security for QEMU becomes one of the topmost priorities for organizations running cloud environments around the world. Secure Computing mode (Seccomp) is one such library which protects the host from a rogue VM running inside the QEMU. Seccomp is a configurable system call (syscall) filter which can be extended to allow or deny certain syscalls that are being called the VMs through QEMU and KVM. These syscalls can be particularly dangerous if they can be manipulated to place an attack on the host through QEMU. One such attack can be placed using syscall mprotect() (using CVE-2015-5165 and CVE-2015-7504) from a compromised VM to QEMU host, rendering the entire infrastructure vulnerable. Since seccomp is a static filter, the critical syscalls like mprotect() cannot be excluded once their usage is over. To overcome this limitation of seccomp, we propose a new mechanism for syscall filtering in QEMU. This mechanism is based on the syscalls that are called by QEMU in different phases of VM operation, for e.g. booting, running. From our experiments, we have found out that there are phases in which some syscalls are never called. To prevent these redundant syscalls to become a point of attack on the host, we have designed a dynamic syscall filter which can be configured according to the phase of VM. The policies can be configured according to the requirement of the syscalls in that particular phase. This can substantially reduce the QEMU attack surface based on syscalls. The transition of phases in QEMU can be tracked using a custom tracer. The tracer will continuously listen for the pre-identified transition point on the VM. Once the transition point occurs, the tracer will send the signal to the QEMU to switch to different policy according to the current phase in which VM is in presently.
