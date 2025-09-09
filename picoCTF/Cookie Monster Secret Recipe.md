# Escola de Segurança Cibernética - Cookie Monster Secret Recipe (picoCTF)
> Solved by @OppermannBruna
>
>> This is a CTF about Cookie Manipulation

## Desafio: Cookie Monster Secret Recipe

**Introdução**

O exercício “Cookie Monster Secret Recipe” faz parte da plataforma picoCTF, conhecida por propor problemas práticos de segurança da informação. O foco deste desafio é a exploração de cookies de navegação e o entendimento do uso de codificação Base64 em aplicações web.
O propósito é mostrar como dados aparentemente protegidos podem, na realidade, estar apenas ofuscados e facilmente acessíveis.

* [Página do Desafio](https://play.picoctf.org/practice/challenge/469)


**Análise Inicial**

Enunciado do desafio:

>*Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. As an aspiring cookie detective, your mission is to uncover this delectable secret. Can you outsmart Cookie Monster and find the hidden recipe?
Additional details will be available after launching your challenge instance.*

Dicas:

>Sometimes, the most important information is hidden in plain sight. Have you checked all parts of the webpage?

>Cookies aren't just for eating - they're also used in web technologies!

>Web browsers often have tools that can help you inspect various aspects of a webpage, including things you can't see directly.


**Interpretando as Dicas**

As dicas deixam claro que a chave está nos **cookies**. 
Como o enunciado reforça diversas vezes esse termo, é razoável concluir que a solução envolve esses arquivos de texto que os sites utilizam para registrar informações da navegação do usuário.

**Solução**

Somos redirecionados para a seguinte página quando começamos:

<img width="710" height="376" alt="Captura de tela 2025-09-09 110856" src="https://github.com/user-attachments/assets/ea1846f7-d63f-48b7-8f63-56b373a0ca2d" />


Fiz o login usando a palavra *teste* como usuário e senha.

Logo após, somos redirecionados para outra página, na qual nos diz que nosso acesso foi negado.

<img width="735" height="333" alt="Captura de tela 2025-09-09 110909" src="https://github.com/user-attachments/assets/b3640dac-966d-4669-9aeb-34aeca39d721" />

Na página de acesso negado, há uma mensagem que reforça a necessidade de analisar os cookies do site. Retomando as dicas iniciais, fica evidente que o próximo passo é inspecioná-los.

Para isso, utilizamos as **DevTools** do navegador (atalho F12). Dentro da aba *Application*, existe a seção **Cookies**, onde são listados os domínios que armazenam esses dados. É justamente nesse local que encontramos o valor que precisava ser decodificado.

<img width="693" height="606" alt="Captura de tela 2025-09-09 111008" src="https://github.com/user-attachments/assets/82f57955-cd81-4a82-8622-93619b43e70f" />

O domínio está codificado em Base64 (codifica binários em texto). Usei o site [base64decode.org](https://www.base64decode.org/) para fazer a decodificação e flag apareceu.

<img width="945" height="762" alt="Captura de tela 2025-09-09 113559" src="https://github.com/user-attachments/assets/4ea027d1-19ae-457e-9e98-599f2e5eefa4" />

**Conclusão**

Flag:

>picoCTF{c00k1e_m0nster_l0ves_c00kies_B3AD94C2}

Esse desafio mostrou na prática que nem sempre as informações guardadas em cookies estão realmente seguras. Muitas vezes, o que parece estar protegido está apenas “escondido” em Base64, um formato que qualquer pessoa consegue decodificar sem dificuldade.

Foi também uma boa oportunidade para treinar o uso das ferramentas do navegador na inspeção de cookies, percebendo como até pequenas mudanças podem impactar diretamente o site. Essa experiência deixa claro que vulnerabilidades simples podem comprometer dados importantes e colocar a aplicação em risco.

