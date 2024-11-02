<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../../index.md)
# **Relatório Simulação 1: Filtragem Remeez no Matlab**
## Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Sistemas de transmissão de dados**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Luiz Felipe Silveira  
**Data:** 25/10

## 1. Introdução
A filtragem é uma das técnicas de processamento digital de sinais fundamentais para tratar muitos tipos de dados do mundo real, e assim melhorar a qualidade dos sinais para uma determinada aplicação.
Os filtros digitais podem ser divididos em dois tipos:
* FIR: Filtros que possuem resposta ao impulso finita no tempo
* IIR  Filtros que possuem resposta com suporte infinito no tempo
Cada um desses filtros tem suas vantagens e desvantagens em aplicações reais, o filtro que será utilizado nessa simulação será um filtro FIR.

---

## 2. Objetivo
O objetivo dessa simulação é realizar a implementação de um filtro FIR utilizando as funções do matlab para filtrar um ruído impulsivo propositalmente adicionado em um sinal de áudio real gerado, e assim verificar o resultado da filtragem e comparar com o sinal original.


---

## 3. Metodologia e implementação

### Ruído Impulsivo 
Um ruído é chamado de impulsivo na frequência se assume a forma de uma senoide no tempo, e pode ser reprsentado por uma soma de senoides de diferentes frequências no tempo e é um sinal periódico. Para o ruído trabalhado nessa simulação, será usada a forma

$$
r(t) = a_1 cos(2 \pi f_1 t) + a_2 cos(2 \pi f_2 t) 
$$

### Largura de banda essencial de um sinal de áudio
Um sinal de áudio tem largura de banda essencial de 4kHz, por padrão, que são as frequências que conseguimos escutar de forma melhor, então, para um sinal de voz podemos considerar 

$$
v(f) = 0, \textbf{se } f>=4000Hz 
$$


### Contaminação do sinal de ruído
Para fazermos o experimento de filtragem, contaminamos o sinal de áudio com o ruído senoidal no tempo, então temos o sinal de voz corrompido $z(t)$ dado por

$$
z(t) = v(t) + r(t)
$$

### Amostragem dos sinais
Por mais que as operações anteriores foram definidas no domínio do tempo, no computador é necessário amostrar os sinais.
O sinal de áudio é amostrado assim que é recebido, ja o sinal de ruído pode ser gerado continuamente como uma função, mas pode ser amostrado e transformado num vetor, basta fazer

$$
r[n] = r(t = nT_s)
$$

onde T_s é o mesmo período de amostragem do sinal de voz.
e para amostrarmos o sinal $r[n]$, precisamos antes do vetor de tempo contínuo amostrado do sinal de voz $v(t)$, assim podemos definir o vetor de tempo baseado no número de amostras $N$ do sinal de voz, assim

$$
t = (0,1,...,N)\cdot T_s
$$

então finalmente o ruído amostrado se torna
### Importando sinais de áudio no matlab
No matlab a função 'audioread' pode ser usada para ler um arquivo de áudio e transformá-lo para um vetor, normalmente o sinal é transformado para dois canais, porém fazendo a média do sinal podemos
pegar apenas um canal(mono), além disso a função também retorna a frequência de amostragem, que por padrão é uma frequência alta de valor 48000Hz.
* Importando um áudio para o matlab

```
%Importação do áudio
[v, Fs] = audioread('Audio.wav');
%colocando o resposta em mono
v = mean(v,2);
```

### Filtro FIR no matlab
No matlab é possível projetar um filtro FIR a partir da função firpm, que aceita como parâmetros a ordem do filtro, um eixo de frequências noramlziadas pela largura de banda essencial, e com os respectivos ganhos de cada
frequência, e retorna a resposta ao impulso de um filtro FIR ótimo.
* Função firpm
```
 f = [0 fp1_n fs1_n fs2_n fp2_n 1];
a = [1 1 0 0 1 1];
M = 100;
h = firpm (M, f, a);
```

### Plot da resposta do filtro no tempo e frequência
Para plotar a resposta em frequência foi usada a função freqz do matlab, que retorna a transformada de fourier(módulo e fase) com seu respectivo eixo de frequências.
Para obter o módulo, foi usada a função abs(H(w)), e para obter a fase num período, foi usada a função uwnrap(angle(H(w))).
*Código da plotagem do espectro no matlab
```
[H,f] = freqz(h)

modH = abs(H);
FaseH = unwrap(angle(H));

%Plotagem dos Espectros
figure
plot(f,modH,'-cyan')
xlabel('f(Hz)');
ylabel('Magnitude');
title('|H(w)|');

figure
plot(f,FaseH,'-m')
xlabel('f(Hz)');
ylabel('Fase (radianos)');
title('<H(w)');

```

## 4. Resultados 

### Espectro de frequência dos sinais



---

## 5. Conclusão

A operação de negativo de uma imagem é uma técnica simples, mas eficaz, para a manipulação de imagens digitais. Ao substituir cada valor de intensidade de pixel pelo seu complemento, conseguimos inverter o contraste da imagem, o que pode ser útil em diversas aplicações, como em processamento médico, onde inversões de contraste podem revelar detalhes ocultos.

A inversão dos quadrantes, por sua vez, altera a disposição espacial dos pixels, criando uma nova organização visual da imagem. Essas técnicas juntas demonstram como manipulações básicas no processamento de imagens podem resultar em efeitos visuais significativos e úteis.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
