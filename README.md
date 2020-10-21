# bind-shell-attack

Nessa atividade foi necessário simular um bind shell attack, e após isso, no computador da vitima, recuperar o arquivo binário deletado desse ataque e analisar esse processo suspeito.

- Nesse ataque de backdoor foi utilizado o netcat, por mais que existam outras maneiras de realizar esse tipom de ataque.

Em um bind shell attack o computador da vítima é capaz de transmitir o executavel do bash, ou cmd, através de uma porta específica, para o computador da pessoa que deseja realizar esse ataque. No caso do netcat o atacante necessita fazer com que a vítima rode uma linha de comandos, transmitindo o bash para o computador do atacante. No caso do bind shell, o computador da vítima fica esperando que o computador de ataque se conecte ao ip e a porta.

Ex:
