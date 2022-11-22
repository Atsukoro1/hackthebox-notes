# HackTheBox Redpanda machine notes

## SSTI
- For some reason always returning process instead of the command response
- Possible SSTI (POST HTTP endpoint /search) payload = "name='*{ssti}'" (not possible with dollar sign for some reason)