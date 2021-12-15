# Netfilter
Nftables learning


#4.3.1 - counter.conf
```bash
#!/usr/sbin/nft -f 

flush ruleset

table inet firewall {
    chain incoming {
        type filter hook input priority 0; policy drop;
        
        #counter for incoming
        counter
        
        ct state established,related counter accept
        ct state invalid counter accept
        tcp dport ssh counter accept 
        icmpv6 type { nd-neighbor-solicit, nd-router-advert } counter accept
        iif lo counter accept
        
        #counts and drops any traffic not covered by an earlier rule
        counter drop
    }
}
```

#4.3.2 icmp.conf
```bash
#!/usr/sbin/nft -f 

flush ruleset

table inet firewall {
	chain incoming {
		type filter hook input priority 0; policy drop;
		
		#counter for incoming
		counter
		
		ct state established,related counter accept
		ct state invalid counter accept
		tcp dport ssh counter accept 
		icmpv6 type { nd-neighbor-solicit, nd-router-advert } counter accept
		iif lo counter accept
		
		#gdzie 56 to numer grupy
		ip saddr 192.168.56.0/24 icmp type echo-request counter accept
		
		#counts and drops any traffic not covered by an earlier rule
		counter drop;
	}
	
}
```
