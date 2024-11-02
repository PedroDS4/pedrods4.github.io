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
Um sinal de áudio tem largura de banda essencial de $$4kHz$$, por padrão, que são as frequências que conseguimos escutar de forma melhor, então, para um sinal de voz na frequência podemos considerar 

$$
V(f) = 0, \textbf{se } f \geq 4000Hz 
$$


### Contaminação do sinal de ruído
Para fazermos o experimento de filtragem, contaminamos o sinal de áudio com o ruído senoidal no tempo, então temos o sinal de voz corrompido $$z(t)$$ dado por

$$
z(t) = v(t) + r(t)
$$

### Amostragem dos sinais
Por mais que as operações anteriores foram definidas no domínio do tempo contínuo, no computador é necessário amostrar os sinais.
O sinal de áudio é amostrado assim que é recebido, ja o sinal de ruído pode ser gerado continuamente como uma função, mas pode ser amostrado e transformado num vetor, basta fazer

$$
r[n] = r(t = nT_s)
$$

onde T_s é o mesmo período de amostragem do sinal de voz.
e para amostrarmos o sinal $$r[n]$$, precisamos antes do vetor de tempo contínuo amostrado do sinal de voz $$v(t)$$ , assim podemos definir o vetor de tempo baseado no número de amostras $$N$$ do sinal de voz, assim

$$
t_n = (0,1,...,N)\cdot T_s
$$

então finalmente o ruído amostrado se torna

$$
r(t_n) = r[n]
$$

Dessa maneira, tenis que o sinal corrompido $$z[n]$$ amostrado será

$$
z[n] = v[n] + r[n]
$$

### Filtro Rejeita-faixas
A resposta ao impulso de um filtro rejeita faixas é a transformada de fourier de uma combinação linear de funções retângulares defasadas na frequência, e pode ser modelada pela expressão matemática

$$
h[n] = \delta [n] + \frac{sin[w_{c1} n]}{\pi n} - \frac{sin[w_{c2} n]}{\pi n},  \textbf{ se } M \geq n \geq 0
$$

onde M é a ordem do filtro.
Então essa expressão pode ser implementada a partir da expressão da função acima truncada pelo número de termos que queiramos no filtro.

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

### Gráficos dos sinais de voz

![Sinal de voz original no tempo](./imagens/Sinal_de_voz_original.png)
*Figura 1: Sinal de voz original no domínio do tempo.*

![Sinal de voz corrompido pelo ruído senoidal](./imagens/Sinal_corrompido.png)
*Figura 2: Sinal de voz corrompido pela adição de ruído senoidal.*

![Sinal de voz corrompido pelo ruído senoidal](./imagens/Sinal_filtrado.png)
*Figura 3: Sinal de voz filtrado após a aplicação do filtro FIR.*

### Gráficos da resposta do filtro
![Magnitude da resposta em frequência do filtro](./imagens/Magnitude_resposta_em_frequencia_do_filtro.png)
*Figura 4: Magnitude da resposta em frequência do filtro FIR.*

![Fase da resposta em frequência do filtro](./imagens/Fase_da_resposta_em_frequencia_do_filtro.png)
*Figura 5: Fase da resposta em frequência do filtro FIR.*

---

## 5. Conclusão
A resposta em frequência do filtro rejeita-faixas projetado ficou como o esperado, como mostra os gráficos do espectro da resposta em frequência do filtro, assim foi possível perceber que a fase linear esta presente, e isso é uma das características de um filtro FIR implementado e também é uma característica básica de qualquer sistema digital real.
Foi possível observar que a filtragem não eliminou completamente o ruído, mas o atenuou ao ponto dele ficar com volume muito menor que a voz original, e algumas frequências do sinal de voz ficaram alteradas, como observado no gráfico do sinal de voz original no tempo.

---

## 6. Referências

Felipe, Luiz. *Slides Processamento Digital de Sinais e Sistemas de transmissão de dados*. **Processamento Digital de Sinais**. Universidade Federal do Rio Grande do Norte, 2024.
