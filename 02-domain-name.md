# Register a Domain Name


1. Register a domain name with any provider. 
2. On the registrar's site, navigate to the DNS management page for the domain.
3. Delete exiting DNS 'parking' records.
4. Add the records below - it can take from minutes to hours for changes to propagate.
5. Check DNS propagation at: https://dnschecker.org

```bash
# A Record - @ - Instance IPv4
site.com
# A Record - www - Instance IPv4
www.site.com

# AAAA Record - @ - Instance IPv6
site.com
# AAAA Record - www - Instance IPv6
www.site.com
```
