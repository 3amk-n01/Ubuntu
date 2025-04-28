# After Installing Your Desktop :

---

## Summary :

- This is a beginner guide and i hope it be useful for Ubuntu users .
- Don't open your browser before updating your system .
- Even that don't open any suspicious `links/email/website` and that not a case in the Ai area .

VIP Note : That this is not enough it just a start  { Defence in depth } .

---

## 1. Minimize Software to Minimize Vulnerability :

```bash
1.
sudo apt purge telnet cups cups-pdf cups-client cups-bsd cups-ppdc samba samba-libs libwbclient0 libsmbclient0 evolution rhythmbox bluez libreoffice-* ufw gnome-remote-desktop 

2.
sudo snap remove cups

```

---
## 2. Disable Unwanted Services :

```bash

sudo systemctl list-unit-files --type=service | grep enable

```

```bash

	# Eamples:
		sudo systemctl stop bluetooth.target 
		sudo systemctl stop bluetooth.service 
		sudo systemctl disable cups.service
		sudo systemctl disable cups-browsed.service
		sudo systemctl disable bluetooth.service
		sudo systemctl stop cups-browsed
		sudo systemctl stop cups.path
		sudo systemctl stop cups.service
		sudo systemctl disable speech-dispatcher.service
		sudo systemctl stop speech-dispatcher
		sudo systemctl disable postfix.service
		sudo systemctl disable avahi-daemon.service
		sudo systemctl stop avahi-daemon
		sudo systemctl disable avahi-daemon.socket
		sudo systemctl stop avahi-daemon.socket
		sudo systemctl disable ModemManager.servic
		sudo systemctl stop ModemManager.service
		sudo systemctl disable  minissdpd
		sudo systemctl stop  minissdpd
		
		sudo systemctl mask systemd-journal-remote
		sudo systemctl mask bluetooth.target 
		sudo systemctl mask cups.service
		sudo systemctl mask cups-browsed.service
		sudo systemctl mask cups.path
		sudo systemctl mask cups.service
		sudo systemctl mask speech-dispatcher.service
		sudo systemctl mask postfix.service
		sudo systemctl mask avahi-daemon.service
		sudo systemctl mask ModemManager.servic
		sudo systemctl mask  minissdpd
		sudo systemctl mask avahi-daemon.socket
		
```


---

## 3. sysctl.conf :

- Kernel hardening :
```
		dev.tty.ldisc_autoload = 0
		fs.protected_fifos = 2
		fs.protected_hardlinks = 1
		fs.protected_regular = 2
		fs.protected_symlinks = 1
		fs.suid_dumpable = 0
		vm.mmap_rnd_bits = 32
		vm.mmap_rnd_compat_bits = 16
		kernel.core_uses_pid = 1
		kernel.dmesg_restrict = 1
		kernel.kexec_load_disabled = 1
		kernel.kptr_restrict = 2
		kernel.perf_event_paranoid = 3
		kernel.randomize_va_space = 2
		kernel.sysrq = 0
		kernel.unprivileged_bpf_disabled = 1
		kernel.unprivileged_userns_clone = 0
		kernel.yama.ptrace_scope = 1
		kernel.modules_disabled = 1
		net.core.bpf_jit_harden = 2
		net.ipv4.conf.all.accept_redirects = 0
		net.ipv4.conf.all.accept_source_route = 0
		net.ipv4.conf.all.log_martians = 1
		net.ipv4.conf.all.proxy_arp = 0
		net.ipv4.conf.all.rp_filter = 1
		net.ipv4.conf.all.secure_redirects = 0
		net.ipv4.conf.all.send_redirects = 0
		net.ipv4.conf.all.shared_media = 0
		net.ipv4.conf.default.accept_redirects = 0
		net.ipv4.conf.default.accept_source_route = 0
		net.ipv4.conf.default.log_martians = 1
		net.ipv4.conf.default.rp_filter = 1
		net.ipv4.conf.default.secure_redirects = 0
		net.ipv4.conf.default.send_redirects = 0
		net.ipv4.conf.default.shared_media = 0
		net.ipv4.icmp_echo_ignore_all = 1
		net.ipv4.icmp_echo_ignore_broadcasts = 1
		net.ipv4.icmp_ignore_bogus_error_responses = 1
		net.ipv4.ip_forward = 0
		net.ipv4.tcp_max_syn_backlog = 2048
		net.ipv4.tcp_rfc1337 = 1
		net.ipv4.tcp_sack = 0
		net.ipv4.tcp_synack_retries = 2
		net.ipv4.tcp_syncookies = 1
		net.ipv4.tcp_syn_retries = 5
		net.ipv4.tcp_timestamps = 0
		net.ipv6.conf.all.accept_redirects = 0
		net.ipv6.conf.all.accept_source_route = 0
		net.ipv6.conf.all.disable_ipv6 = 1
		net.ipv6.conf.default.accept_ra_defrtr = 0
		net.ipv6.conf.default.accept_ra_pinfo = 0
		net.ipv6.conf.default.accept_ra_rtr_pref = 0
		net.ipv6.conf.default.accept_redirects = 0
		net.ipv6.conf.default.accept_source_route = 0
		net.ipv6.conf.default.autoconf = 0
		net.ipv6.conf.default.dad_transmits = 0
		net.ipv6.conf.default.disable_ipv6 = 1
		net.ipv6.conf.default.max_addresses = 1
		net.ipv6.conf.default.router_solicitations = 0
		net.ipv6.conf.lo.disable_ipv6 = 1


```

- Apply new settings:
```bash 
	sudo sysctl -p
```

---
## 4. Others :
 
 - Limited Users Access :
```bash
	sudo nano /etc/security/access.conf

# ADD 
-:root:ALL EXCEPT LOCAL
+:Your_UserName:LOCAL
-:ALL:ALL
```

- Default umask in /etc/login.defs could be more strict like 027
```bash
	
	sudo nano /etc/login.defs

	 # Change
	
		ERASECHAR	0177
		KILLCHAR	025
		UMASK		<Change_Here_To_027>
		 
```

```bash 
	sudo nano /etc/security/limits.conf   
	 // Disable  core dump To
	 
	 *                hard    core            0

```

```bash 
	
	sudo nano /etc/profile file as follows:
	
	# No core files by default
	# ulimit -S -c 0 > /dev/null 2>&1
	
```


---
## 5. Close ports and inspect traffic 

```bash 
		sudo lsof -i 
		#or 
		ss -pln 
		#or 
		sudo ss -tupla
```

---
## 6. Now Go Online:

#### Step 1: Update and Upgrade System
```bash
	sudo apt update && apt upgrade -y && apt autoremove -y
```

(( VIP Note )) : `Use this every time you start your PC`
```bash
	sudo apt update && sudo apt upgrade && sudo snap refresh
	
```

#### Step 2: Install Essential Security Tools

```bash
		1.
		sudo apt install apparmor-profiles apparmor-utils firejail firejail-profiles  

		2. 
		sudo firecfg
		
		3.
		sudo aa-enforce /etc/apparmor.d/*
		sudo apparmor_status

		4.
		sudo apt install < Your Other Apps >
```

