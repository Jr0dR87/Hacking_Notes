### What is LLMNR? (link-local multicast name resolution)

- Used to identify hosts when DNS fails
- Was previously NBT-NS
- The main flaw is that the service utilize a users username and NTLMv2 hash when appropriately responded to.

### Mitigations
- Disable LLMNR and NBT-NS
- If they cannot, require NAC (Network Access Control)
