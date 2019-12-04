# estudo-desafio2

Essa é a documentação inicial necessaria para fazer o projeto funcionar.

## Antes de começar
Antes de começar vamos instalar alguns requisitos para o melhor funcionamento.

### Instalando o necessario

#### Instalando no Linux - Debian
Conseguimos instalar no Debian usando o **pip** e pode ser feito da seguinte forma.
```sh
sudo easy_install pip
sudo pip install ansible
```

#### Instalando no Linux - Ubuntu
Conseguimos tambem instalar no Ubuntu usando **apt-get**.
```sh
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

### Chave SSH
Tudo o que precisamos nos clientes é que o usuario tenha autenticação por chave SSH, por exemplo:
```sh
ssh usuario@maquina
```
> Ele deve logar sem pedir a senha. Precisamos modificar o usuario do arquivo do **Ansible** e execute o playbook como indicado anteriormente.

É muito importante que o usuario não precise de senha para logar, podemos resolver isso adicionando a public key no **authorized_keys**.

Alem disso devemos configurar para que não seja necessario o uso de senha no **sudo**.

### Retirando acesso via senha por SSH
Na Distribuição Debian e seus derivados podemos alterar o arquivo de configuração do nosso servidor e assim retirar o login por senha.
```sh
sudo nano /etc/ssh/sshd_config
```

Vamos procurar por **PasswordAuthentication yes**, pode estar comentado, vamos descomentar e mudar seu valor para **no**.

Segue o exemplo
```sh
PasswordAuthentication no
```
> Dessa forma não vamos mais nos autenticar com o uso de senha.

### Criando e adicionando chave publica
Vamos na nossa maquina host criar uma chave publica, vamos usar
```sh
ssh-keygen
```
> Por padrão ele cria o arquivo **id_rsa**.

Podemos apertar enter para deixar o nome padrão **id_rsa**, ou então dar um nome para essa chave.

Vamos até o diretorio do **.ssh**, se estiver usando o Debian ou algum derivado vai ser parecido com esse caminho.
```sh
/home/greenmind/.ssh
```
> **greenmind** é meu nome de usuario, caso o seu seja **carlos** seria algo como **/home/carlos/.ssh**.

Nossa chave publica vai ter o final **.pub**. Veja o exemplo da minha chave:
```sh
cat key_teste.pub
```

O resultado vai ser algo parecido com o resultado abaixo:
```sh
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC7h89rHd96PC/9M3qG59HoncTGopoB3qpUXWxltp1lcE54iCKAVZfx7C4eEJO/dFDWRiNm5XyMyCuOsYmEelnXLopSFSFTT$%Y#W@423546ygdwdw3ef44444TqWGKBocsmX8IQTdHuWcYOREaxTs9+WPdvPdc5uYqQ5ggVo7rP6K1VeJm/KbnCOhp7un/VWrKLYDXaRupaodaIKpFJnrA/4/DeS5fumI1lva/87HTLN greenmind@abase
```

Agora vamos até a maquina que queremos acessar, ir até o diretorio do SSH, procurar pelo arquivo **authorized_keys**, caso não tenha vamos criar.
```sh
sudo nano /home/greenmind/.ssh/authorized_keys
```
> **greenmind** é meu nome de usuario, caso o seu seja **carlos** seria algo como **/home/carlos/.ssh**.

Vamos adicionar a chave publica criada anteriormente nesse arquivo **authorized_keys** e dessa forma não precisamos mais de senha para logar no servidor.

### Testando acesso
Agora só realizar um login ao nosso servidor, se der tudo certo ele não vai pedir a senha, apenas para aceitar o uso da chave.
> Não se esqueça de testar a conexão antes, pois pode ter problemas ao executar e excutar a chave.

```sh
ssh user@meuservidor
```

## Executando o projeto

### Criando arquivo hosts
Nosso arquivo hosts é resposavel por criar o grupo que vamos executar o playbook, por exemplo.

Aqui vamos criar dois grupos, um dos grupos é o **centos** e o outro é o **test**.
```sh
[centos]
3.213.226.214

[test]
127.0.0.1
```
> Podemos colocar qualquer nome no arquivos hosts, porem precisamos setar ele usando -i quando for executar a playbook.

### Executando playbook
```sh
ansible-playbook -i hosts web-wordpress.yml
```
