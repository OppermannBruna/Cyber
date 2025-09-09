# Escola de Segurança Cibernética - Flag Hunters (picoCTF)

> Solved by @OppermannBruna
>
>> This is a CTF about Reverse Engineering

## Desafio: Flag Hunters

**Introdução**

O desafio “Flag Hunters” faz parte da plataforma picoCTF, conhecida por propor exercícios práticos de segurança cibernética.
O tema central aqui é Engenharia Reversa, explorando como entradas de usuário não tratadas podem ser manipuladas para alterar o fluxo de execução de um programa.
O propósito do exercício é identificar uma vulnerabilidade no código.

* [Página do Desafio](https://play.picoctf.org/practice/challenge/472)


**Análise Inicial**

Enunciado do desafio:

>*Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? There might be something in it for you.
The program's source code can be downloaded here.
Additional details will be available after launching your challenge instance.*

Dicas:

>*This program can easily get into undefined states. Don't be shy about Ctrl-C.*
>*Unsanitized user input is always good, right?*
>*Is there any syntax that is ripe for subversion?*

**Interpretando as Dicas**

A segunda dica é a mais importante: unsanitized user input. Isso significa que o programa não faz validação da entrada, permitindo que comandos injetados pelo usuário sejam interpretados diretamente.
Essa vulnerabilidade abre espaço para injeção de código, permitindo mudar o comportamento normal do programa e acessar informações que não seriam exibidas de outra forma.

**Solução**

O arquivo disponibilizado pelo desafio é um script em Python.
Ao abrir o código no **VS Code** (Visual Studio Code), percebemos que a flag aparece logo no início, mas o programa começa sua execução a partir do VERSO 1, ignorando o trecho onde o segredo está guardado.

<img width="571" height="323" alt="Captura de tela 2025-09-09 132805" src="https://github.com/user-attachments/assets/5d3025ff-78ce-4dfa-b772-c8116a8f90fe" />
<img width="593" height="195" alt="Captura de tela 2025-09-09 132721" src="https://github.com/user-attachments/assets/d3d47bbe-4c1f-47d4-8e1c-631a1abe5f8c" />

O comportamento do programa sugeria que havia uma forma de manipular o fluxo de execução. Isso ficou mais claro ao observarmos o uso do comando RETURN, que permitia redirecionar a leitura para uma linha específica do código.

Quando executamos o programa no Webshell - terminal do PicoCTF, percebemos que ele imprimia a música e, em determinado ponto, solicitava uma entrada do usuário:

<img width="536" height="260" alt="Captura de tela 2025-09-09 135259" src="https://github.com/user-attachments/assets/db4c20db-cab5-4cae-aaa8-2674ced0bf57" />

O primeiro teste foi digitar RETURN 0, mas a saída mostrou apenas o texto literal, sem efeito algum. Isso indicava que a entrada estava sendo interpretada como string.

Revisando novamente o código, identificamos que as instruções eram separadas por ponto e vírgula (;). Esse detalhe foi a chave: se inseríssemos qualquer texto antes, seguido de ; RETURN 0, o script passaria a interpretar o comando de fato, alterando o fluxo do programa.
Com isso, o programa voltou a imprimir desde a primeira linha, revelando também a parte oculta chamada secret_intro, onde estava a flag escondida.

<img width="592" height="626" alt="Captura de tela 2025-09-09 135440" src="https://github.com/user-attachments/assets/64a92922-5476-4641-883f-e0d33406ef14" />

**Conclusão**

Flag:

>picoCTF{70637h3r_for3v3r_a5202532}

Esse desafio ilustrou como uma entrada sem validação pode ser explorada para alterar o comportamento de um programa.
Na prática, foi um exercício de injeção de código em Python, mostrando como detalhes aparentemente simples — como a forma de separar instruções — podem ser usados para explorar falhas.
A principal lição é clara: em sistemas reais, validar dados de entrada é indispensável. Ignorar essa etapa pode permitir desde a exposição de informações até a execução de ataques muito mais graves.
