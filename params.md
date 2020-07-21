
Kernel Security Parameters
==
- `kernel.kptr_restrict = 2`
	-  Prevents leaking kernel memory addresses
- `kernel.randomize_va_space = 2`
	- Strong ASLR
- `net.ipv4.icmp_echo_ignore_broadcasts = 1`
- `net.ipv4.icmp_ignore_bogus_error_responses = 1`
	- Prevent using ICMP for enumeration
- `net.ipv4.tcp_syncookies = 1`
	- TCP SYN flood attack mitigations
- `net.ipv4.tcp_fin_timeout = 30`
	- Close sockets early to prevent DoS by file descriptor/port exhaustion
- `net.ipv4.tcp_rfc1337 = 1`
	- Use the mitigation defined as F1 from [RFC1337](https://tools.ietf.org/html/rfc1337) to avoid TIME_WAIT assassination attacks
- `net.ipv4.tcp_timestamps = 0`
	- Prevent leaking system uptime and system time, and side channel attacks resulting from them.
- `net.ipv4.conf.all.accept_redirects = 0`
- `net.ipv4.conf.default.accept_redirects = 0`
	- ICMP MITM attack mitigation
- `net.ipv4.conf.all.rp_filter = 1`
- `net.ipv4.conf.default.rp_filter = 1`
	- Strict [RFC 3704](https://tools.ietf.org/html/rfc3704) reverse path filtering
- `net.ipv4.conf.all.log_martians = 1`
- `net.ipv4.conf.default.log_martians = 1`
	- Log packets with obviously undeliverable source addresses (defined in [RFC 1812](https://tools.ietf.org/html/rfc1812))
