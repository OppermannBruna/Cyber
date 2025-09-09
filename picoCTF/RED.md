# Escola de Segurança Cibernética - RED (picoCTF)

> Solved by @OppermannBruna
>
>> This is a CTF about Steganography

## Desafio: RED

**Introdução**

O desafio “RED” faz parte da plataforma picoCTF, que oferece exercícios práticos voltados para segurança cibernética. O foco deste exercício é a esteganografia, ou seja, a prática de esconder informações dentro de arquivos aparentemente comuns, como imagens. Neste desafio, a mensagem secreta está codificada em Base64 e escondida nos bits menos significativos (LSB) de uma imagem. O objetivo é demonstrar como dados podem ser ocultos de forma sutil e ainda assim serem detectáveis com as ferramentas corretas.

* [Página do Desafio](https://play.picoctf.org/practice/challenge/460)


**Análise Inicial**

Enunciado do desafio:

>*RED, RED, RED, RED.*

<img width="916" height="594" alt="Captura de tela 2025-09-09 143235" src="https://github.com/user-attachments/assets/c6a9ceab-0879-472a-9772-50f16f92b5ba" />

Dicas:

>*The picture seems pure, but is it though?*
>*Red?Ged?Bed?Aed?*
>*Check whatever Facebook is called now.*

**Interpretando as Dicas**

A primeira dica sugere que precisamos analisar o próprio arquivo da imagem. Ao abrir a imagem em um editor de texto avançado, como o [Notepad++](https://notepad-plus-plus.org/downloads/), percebemos que havia conteúdo escondido no início da imagem: um poema.
Ao observar as letras maiúsculas do poema, conseguimos montar a frase: CHECK LSB. Essa indicação reforça que os dados secretos estão nos bits menos significativos (LSB) da imagem.

<img width="996" height="680" alt="Captura de tela 2025-09-09 144206" src="https://github.com/user-attachments/assets/baf0c9f2-cb6b-4259-a8a7-e8a55d53a206" />

**Solução**

Para extrair a informação, utilizamos a ferramenta online [CyberChef](https://gchq.github.io/CyberChef/), no menu *Operations*, selecionamos a opção *Extract LSB*, configuramos o Colour Pattern de acordo com a dica “Red?Ged?Bed?Aed”, que indica quais canais de cor e bits foram usados para esconder os dados, o resultado revelou várias strings idênticas codificadas em Base64.

IMAGEM<img width="1918" height="865" alt="Captura de tela 2025-09-09 144714" src="https://github.com/user-attachments/assets/26dd7420-bce5-4d0f-90a8-7870264d6f6c" />


Basta copiar uma dessas strings e decodificá-la em Base64, usando o [base64decode.org](https://www.base64decode.org/), após a decodificação, a flag é revelada:

<img width="807" height="680" alt="Captura de tela 2025-09-09 145025" src="https://github.com/user-attachments/assets/d3acb947-456c-4bf0-bfdc-642b67bbdd5c" />


**Conclusão**

Flag:

>picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}

Este desafio mostra como informações podem estar escondidas de forma sutil em arquivos de imagem. Ele reforça o uso de ferramentas forenses digitais e o conceito de esteganografia, evidenciando que nem sempre arquivos visualmente inofensivos estão totalmente vazios de dados sensíveis.
