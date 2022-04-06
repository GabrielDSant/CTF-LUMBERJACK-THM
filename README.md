# CTF-LUMBERJACK-THM
**Dificuldade - Média**

>Opa! parece que eu lembrei a senha do github... 🤥

<a href="https://tryhackme.com/room/lumberjackturtle"><img src="https://user-images.githubusercontent.com/32500664/161400606-1bf15aab-36be-4994-a601-e8c666be82c6.png">

Finalmente de volta e nesse momento a maquina da vez se chama [Lumberjack Turtle](https://tryhackme.com/room/lumberjackturtle)

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

# 
  
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

  ###... e reação.
  ![11 recebendo shell](https://user-images.githubusercontent.com/32500664/162006254-d0cb090f-a2a5-4af3-bb1f-0d39ea7ddf3f.png)

  ### Cheguei chegando. 
  ![12 verificando user](https://user-images.githubusercontent.com/32500664/162014860-149567ca-7588-48cc-b18d-d9f8f7801eb8.png)
  
  ### Entramos aqui com um objetivo não ?
  ![13 Find na 1 flag](https://user-images.githubusercontent.com/32500664/162027118-017a73d1-d5e7-40f0-acc5-71ec1915ee20.png)
  
  ![14 pegando primeira flag](https://user-images.githubusercontent.com/32500664/162027152-1a6d4563-c6d4-4f2b-8582-34342b1a66bd.png)

Auto escola me quebro
