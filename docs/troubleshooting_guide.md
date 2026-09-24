

## 1. Network Disconnects / Port 9997 Unreachable
- **Symptom**: `Test-NetConnection` fails or times out.
- **Fix**: Verify both VMs in VMware settings are set to **Bridged Mode** (not NAT/Host-Only mismatch). Confirm receiving status on Ubuntu using `sudo ss -tulnp | grep 9997`.

## 2. Universal Forwarder Connected but 0 Events Indexed
- **Symptom**: Port 9997 connects, but `index="windows"` yields 0 results.
- **Fix**:
  1. Inspect `outputs.conf` for matching stanza names (`defaultGroup = default-autolb-group` matching `[tcpout:default-autolb-group]`).
  2. Inspect `inputs.conf` to verify single slash notation (`WinEventLog://`) and `disabled = 0`.
  3. Restart forwarder service: `Restart-Service SplunkForwarder`.