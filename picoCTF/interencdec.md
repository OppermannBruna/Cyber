# Escola de Segurança Cibernética - interencdec (picoCTF)

> Solved by @OppermannBruna
>
>> This is a CTF about Cryptography

## Desafio: interencdec

**Introdução**

O desafio “interencdec” faz parte da plataforma picoCTF, voltada para exercícios práticos de segurança cibernética.
O foco desse exercício é a criptografia e a aplicação de múltiplos processos de codificação/decodificação, como o uso de Base64 e da Cifra de César.
O objetivo principal é treinar a identificação de mensagens escondidas em várias camadas de codificação e aplicar os métodos adequados para recuperá-las.

* [Página do Desafio](https://play.picoctf.org/practice/challenge/418)


**Análise Inicial**

Enunciado do desafio:

>*Can you get the real meaning from this file.
Download the file here.*

Dicas:

>*Engaging in various decoding processes is of utmost importance.*

**Interpretando as Dicas**

A dica sugere diretamente que a resolução passa por diferentes etapas de decodificação.
Ou seja, não basta aplicar apenas um método — é necessário testar várias técnicas de forma sequencial até chegar ao resultado final.

**Solução**

Ao baixar o arquivo disponibilizado pelo desafio e abri-lo no bloco de notas, encontramos um texto encriptado.

<img width="914" height="164" alt="Captura de tela 2025-09-09 122513" src="https://github.com/user-attachments/assets/92b7b6f3-04b6-422c-94ea-2b13907dc2e0" />

O primeiro passo foi verificar se a mensagem estava em Base64. Para isso, utilizei o [CyberChef](https://gchq.github.io/CyberChef/), uma ferramenta online bastante usada para conversões e decodificações.

Na aba Operations, selecionei From Base64 e inseri o texto encontrado.O resultado trouxe uma nova mensagem, ainda codificada.

<img width="1767" height="673" alt="Captura de tela 2025-09-09 124705" src="https://github.com/user-attachments/assets/8c3d06fe-2f12-4d0a-9a24-af4ddd45f2c2" />


<img width="1720" height="680" alt="Captura de tela 2025-09-09 125054" src="https://github.com/user-attachments/assets/e696459a-0373-4db3-8956-5e01e1b41f64" />

Essa segunda mensagem já estava parcialmente legível. Para avançar, testei a Cifra de César, também disponível no [CyberChef](https://gchq.github.io/CyberChef/). Pesquisando por ROT13, encontrei a operação adequada.

Inseri a mensagem no campo Input e comecei a ajustar o valor de deslocamento (Amount). Com 13 posições, ainda não obtive a flag, mas ao variar o número cheguei à configuração correta: 45 deslocamentos.

<img width="1617" height="645" alt="Captura de tela 2025-09-09 125204" src="https://github.com/user-attachments/assets/d4ad4173-cf5d-4253-9cb5-7c74a834abd6" />

**Conclusão**

Flag:

>picoCTF{caesar_d3cr9pt3d_ea60e00b}

Esse desafio demonstrou a importância de considerar que uma mensagem pode estar escondida em múltiplas camadas de codificação.
Ele reforçou a necessidade de testar diferentes métodos de decodificação e de reconhecer rapidamente padrões como Base64 ou cifras clássicas, como a de César.
Além disso, o exercício foi uma ótima prática de uso do CyberChef, ferramenta fundamental para analistas de segurança.
