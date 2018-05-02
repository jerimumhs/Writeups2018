# WEB 1 - Conhecendo o alvo
**Flags by: Elb - f0lds
   FireShell Sec Team**
>Devido Ã  problemas na instalaÃ§Ã£o de outros desafios, a web1 nÃ£o funcionou como deveria.

Ao entrar no desafio nos damos de cara com uma pÃ¡gina de login, caracterizada por 2 inputs, um de usuÃ¡rio e outro de senha, e o simpÃ¡tico Guilherme, o gato charmoso da foto.
![1](https://i.imgur.com/FUd31bv.png)

Tentando logar com qualquer username, o site nos retorna um video, colocdo ali propositalmente, para induzir o participante a acessar o famoso robots.txt.

![2](https://i.imgur.com/WOIVaZV.png)

No robots.txt encontrariamos uma string, que seraia um encode em base64, decodando obteriamos:
> Username: adriano

Depois disso bastava sinebte logar com:
>User: adriano Pass: admin

![3](https://i.imgur.com/YqBgUCP.png)

# Explorando

Ao obtemos o login, o site avisa que *Somente o admin pode ver a flag :/*.
Com isso deveriamos procurar obter acesso administrador.
Logando no site um  segundo cookie seria criado:
>auth="YWRyaWFubw=="

Um base64 da palavra 'adriano'.

Alterando 'adriano' para 'admin' obtemos em base64 - 'YWRtaW4='.

ApÃ³s isso o truque seria trocar os base64  e bypassar o sistema.


![4](https://i.imgur.com/vqGEIzS.png)

> Javascript: document.cookie="auth=YWRtaW4="
> Curl: curl -v -H 'Cookie: auth=YWRtaW4=; PHPSESSID=SEUPHPSSID'  http://localhost/cp/chall.php

Flag 1: JHS{hacked_by_Adri4n0}  

## WEB 2 - XFF.

>ExplicaÃ§Ã£o sem detalhesposque o Kardec apagou o servidor e nÃ£o tinha backup.
>
A web 2 nos retornava uma mensagem mostrando o IP de onde estavamos acessando.

*Seu IP: 182.168.13.37 : Somente o localhost pode visualizar a flag*
Observando o nome do desafio, *xff*, com uma busca rÃ¡pida no google vemos que se trata de X-Fowarded-For:

EntÃ£o poderiamos tentar:
>curl -H "X-Forwarded-For: 127.0.0.1" -v "http://45.77.229.239/cp/flag2.php" 

Com isso a mensagem iria mudar para: *Voce nÃ£o tem privilegio de admin*

Como vimos na primeira, bastaria alterar o cookie para admin, e fazer a requisiÃ§Ã£o:

>curl -H "X-Forwarded-For: 127.0.0.1" -v "http://45.77.229.239/cp/flag2.php" â€”cookie "auth=YWRtaW4="

Flag 2: JHS{I_Know_XFF}
