# Escola de Segurança Cibernética - Natas
> Solved by @OppermannBruna
>
>> **Plataforma:** OverTheWire Wargames  
**Categoria:** Web Security / Server-Side Exploration  

---

### **Resumo**
Este documento detalha a resolução dos primeiros 16 níveis (0-15) do wargame *Natas*. O objetivo foi explorar vulnerabilidades em aplicações web simuladas, variando de falhas de configuração no lado do cliente (Client-Side) a injeções críticas no lado do servidor (Server-Side), como Command Injection e SQL Injection.

---

## Level 0
**Vulnerabilidade:** Exposição de informações sensíveis no HTML (Hardcoded Credentials).

**Análise:**
Ao acessar a página, nenhuma senha é apresentada visualmente. No entanto, em segurança web, o código-fonte (HTML) entregue ao navegador frequentemente contém comentários ou dados ocultos deixados por desenvolvedores.

**Explicação Técnica:**
O navegador renderiza o HTML para ser agradável visualmente, ocultando tags e comentários. A segurança não pode depender da renderização visual.

**Solução:**
Acessei o código-fonte da página (`Ctrl+U`). A flag foi encontrada dentro de um comentário HTML.

> **Flag encontrada:** `0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq`

---

## Level 1
**Vulnerabilidade:** Proteção superficial via JavaScript (Client-Side).

**Análise:**
A página exibe um aviso bloqueando o clique direito do mouse. Isso é feito através de um script JavaScript que intercepta o evento do mouse.

**Explicação Técnica:**
Qualquer restrição implementada no lado do cliente (navegador) pode ser contornada pelo usuário, pois o código já foi baixado e está sob controle total do visitante.

**Solução:**
Utilizei o atalho de teclado `Ctrl+U` para visualizar o código-fonte, ignorando a restrição de clique do mouse. A senha estava novamente em um comentário.

> **Flag encontrada:** `TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI`

---

## Level 2
**Vulnerabilidade:** Listagem de Diretórios (Directory Listing) habilitada.

**Análise:**
A página contém apenas um pixel transparente. Inspecionando o elemento, notei que a imagem reside em `/files/pixel.png`.

**Explicação Técnica:**
Servidores web (como Apache ou Nginx) podem ser configurados para listar todos os arquivos de uma pasta caso não exista um arquivo de índice (`index.php` ou `index.html`). Isso expõe arquivos que não deveriam ser públicos.

**Solução:**
Acessei o diretório raiz da imagem: `http://natas2.natas.labs.overthewire.org/files/`.
O servidor listou os arquivos, revelando o `users.txt` que continha a credencial.

> **Flag encontrada:** `3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`

---

## Level 3
**Vulnerabilidade:** Information Disclosure via `robots.txt`.

**Análise:**
O código-fonte não revelou nada. Recorri à verificação de arquivos padrões de servidores web.

**Explicação Técnica:**
O arquivo `robots.txt` é usado para instruir crawlers (como o Google) sobre quais pastas **não** devem ser indexadas. Ironicamente, desenvolvedores listam pastas secretas ali para "escondê-las", o que acaba revelando sua localização para atacantes.

**Solução:**
Acessei `http://natas3.natas.labs.overthewire.org/robots.txt`. O arquivo continha `Disallow: /s3cr3t/`.
Acessei a pasta `/s3cr3t/` e encontrei o arquivo `users.txt` com a flag.

> **Flag encontrada:** `QryZXc2e0zahULdHrtHxzyYkj59kUxLQ`

---

## Level 4
**Vulnerabilidade:** Controle de acesso baseado no cabeçalho `Referer`.

**Análise:**
O site bloqueia o acesso com a mensagem: "Authorized users should come only from http://natas5...".

**Explicação Técnica:**
O cabeçalho HTTP **Referer** informa ao servidor a URL de onde o usuário veio. Como é um dado enviado pelo cliente, pode ser facilmente falsificado com proxies.

**Solução:**
Utilizei o **Burp Suite** para interceptar a requisição e adicionei manualmente o cabeçalho:
`Referer: http://natas5.natas.labs.overthewire.org/`
O servidor validou a origem (falsificada) e liberou a senha.

> **Flag encontrada:** `0n35PkggAPm2zbEpOU802c0x0Msn1ToK`

---

## Level 5
**Vulnerabilidade:** Autenticação insegura baseada em Cookies não assinados.

**Análise:**
O site informa "You are not logged in". Inspecionando o armazenamento do navegador, encontrei um cookie chamado `loggedin`.

**Explicação Técnica:**
O cookie `loggedin` estava definido como `0` (false). O servidor confiava cegamente nesse valor para determinar o estado de autenticação, sem verificar sessão no backend.

**Solução:**
Editei o valor do cookie para `1` (true) através das ferramentas de desenvolvedor e recarreguei a página. O acesso foi concedido.

> **Flag encontrada:** `0RoJwHdSKWFTYR5WuiAewauSuNaBXned`

---

## Level 6
**Vulnerabilidade:** Exposição de lógica sensível (Source Code Disclosure).

**Análise:**
O desafio fornece o código-fonte PHP. Nele, há a linha: `include "includes/secret.inc"`.

**Explicação Técnica:**
O código compara o input do usuário com uma variável `$secret` definida nesse arquivo externo. Como o caminho do arquivo é relativo e conhecido, podemos tentar acessá-lo diretamente.

**Solução:**
Digitei na URL: `http://natas6.natas.labs.overthewire.org/includes/secret.inc`.
O servidor exibiu o conteúdo do arquivo com o segredo. Inseri o segredo no formulário para obter a flag.

> **Flag encontrada:** `bmg8SvU1LizuWjx3y7xkNERkHxGre0GS`

---

## Level 7
**Vulnerabilidade:** LFI (Inclusão Local de Arquivos).

**Análise:**
A navegação do site altera a URL para `index.php?page=home`. O código fonte sugere que a senha está em `/etc/natas_webpass/natas8`.

**Explicação Técnica:**
A aplicação pega o parâmetro `page` e o passa diretamente para uma função de leitura de arquivo sem sanitização. Isso permite ler qualquer arquivo do sistema operacional ao qual o servidor tenha permissão.

**Solução:**
Alterei a URL para incluir o caminho absoluto do arquivo de senha:
`index.php?page=/etc/natas_webpass/natas8`
O conteúdo do arquivo (a senha) foi exibido na tela.

> **Flag encontrada:** `xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q`

---

## Level 8
**Vulnerabilidade:** Uso de encoding como "criptografia".

**Análise:**
O código fonte revela que o segredo é verificado através da função: `bin2hex(strrev(base64_encode($secret)))`.

**Explicação Técnica:**
Isso não é criptografia, é apenas ofuscação. Como todas as funções são reversíveis, podemos descobrir o segredo original aplicando as funções inversas na ordem contrária.

**Solução:**
Criei um script PHP para reverter o processo:
1. `hex2bin` (Hexadecimal para Binário)
2. `strrev` (Reverter string)
3. `base64_decode` (Decodificar Base64)

Resultado do script: `oubWYf2kBq`. Inseri este valor no site.

> **Flag encontrada:** `ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t`

---

## Level 9
**Vulnerabilidade:** Injeção de Comandos via `passthru()`.

**Análise:**
O site funciona como um dicionário usando o comando `grep`. O código vulnerável é: `passthru("grep -i $key dictionary.txt")`.

**Explicação Técnica:**
A variável `$key` (input do usuário) não é sanitizada. No Linux, o caractere `;` permite executar múltiplos comandos sequenciais na mesma linha.

**Solução:**
Injetei o payload: `; cat /etc/natas_webpass/natas10`
O comando final executado foi: `grep -i ; cat /etc/natas_webpass/natas10 dictionary.txt`.
O servidor executou o `grep` (que falhou ou retornou vazio) e em seguida o `cat`, exibindo a senha.

> **Flag encontrada:** `t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu`

---

## Level 10
**Vulnerabilidade:** Command Injection com Bypass de Filtros.

**Análise:**
Similar ao nível 9, mas agora caracteres como `;` e `&` são bloqueados por uma expressão regular.

**Explicação Técnica:**
Não podemos iniciar um novo comando. Porém, podemos manipular os argumentos do comando `grep` existente. O `grep` aceita múltiplos arquivos para busca.

**Solução:**
Payload utilizado: `a /etc/natas_webpass/natas11`
O comando interpretado foi: `grep -i a dictionary.txt /etc/natas_webpass/natas11`.
Isso instruiu o grep a buscar a letra "a" dentro do arquivo de senhas, revelando o conteúdo.

> **Flag encontrada:** `UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk`

---

## Level 11
**Vulnerabilidade:** Criptografia XOR com chave reutilizada.

**Análise:**
O site usa um cookie criptografado com XOR para salvar preferências. Temos o texto original (JSON) e o texto cifrado (Cookie).

**Explicação Técnica:**
A operação XOR é simétrica (`A XOR B = C` -> `A XOR C = B`).
Fiz o XOR do texto original com o cookie cifrado para descobrir a chave de criptografia: `eDWo`.

**Solução:**
1. Criei um novo JSON: `{"showpassword":"yes", "bgcolor":"#ffffff"}`.
2. Apliquei XOR com a chave descoberta (`eDWo`).
3. Codifiquei em Base64 e substituí o cookie no navegador.
A página recarregada exibiu a senha.

> **Flag encontrada:** `yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB`

---

## Level 12
**Vulnerabilidade:** Unrestricted File Upload (Bypass de Extensão).

**Análise:**
O site permite upload de imagens JPEG, mas renomeia o arquivo. O código confia na extensão fornecida pelo usuário.

**Explicação Técnica:**
O servidor web decide como executar um arquivo baseado na extensão. Se conseguirmos subir um `.php`, podemos executar comandos no servidor (Web Shell).

**Solução:**
1. Criei um arquivo `shell.php` com o conteúdo: `<?php echo file_get_contents('/etc/natas_webpass/natas13'); ?>`.
2. Interceptei o upload com o **Burp Suite**.
3. Alterei a extensão no pacote de dados de `.jpg` para `.php`.
4. Acessei o link do arquivo gerado e o script foi executado.

> **Flag encontrada:** `trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC`

---

## Level 13
**Vulnerabilidade:** Verificação de tipo de arquivo incompleta.

**Análise:**
O site agora usa `exif_imagetype` para verificar se é uma imagem.

**Explicação Técnica:**
Essa função verifica apenas os "Magic Bytes" (assinatura hexadecimal) no início do arquivo. Para JPEG, a assinatura é `FF D8 FF E0`. O restante do arquivo pode conter código malicioso.

**Solução:**
Adicionei a assinatura JPEG no início do meu arquivo PHP (`shell.php`).
O servidor aceitou como imagem válida. Como forcei a extensão `.php` via Burp Suite, o código foi executado.

> **Flag encontrada:** `z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ`

---

## Level 14
**Vulnerabilidade:** SQL Injection na Autenticação.

**Análise:**
O código SQL concatena o input do usuário diretamente:
`SELECT * FROM users WHERE username="INPUT_USER" AND password="INPUT_PASS"`.

**Explicação Técnica:**
Podemos fechar as aspas da query e injetar uma condição que seja sempre verdadeira, ignorando a verificação de senha.

**Solução:**
Payload no campo username: `" OR 1=1 --`
Isso transforma a query em: `SELECT * FROM users WHERE username="" OR 1=1`.
O banco de dados retorna o primeiro usuário (admin) e libera o acesso.

> **Flag encontrada:** `SdqIqBsFcz3yotlNYErZSZwblkm0lrvx`

---

## Level 15
**Vulnerabilidade:** Blind SQL Injection (Boolean-based).

**Análise:**
O site apenas retorna se o usuário existe ou não, sem mostrar dados.

**Explicação Técnica:**
Precisamos extrair a senha caractere por caractere fazendo perguntas de "Sim ou Não" ao banco de dados.
Exemplo de pergunta SQL: "A senha do usuário natas16 começa com a letra 'a'?"

**Solução:**
Utilizei um script em Python para automatizar o ataque de força bruta. O script testou todos os caracteres possíveis para cada posição da senha.

> **Flag encontrada:** `hpkjkyvilqctew33qmuxl6edvfmw4sgo`

*Exemplo de lógica do script:*
```python
payload = 'natas16" AND password LIKE BINARY "a%" #'
# Se retornar "User exists", a primeira letra é 'a'.
