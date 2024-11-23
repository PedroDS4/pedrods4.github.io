<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

#**Relatório Atividade 8: Transformada de Fourier**

# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 25/11

## 1. Introdução

A transformada de fourier é uma ferramente muito utilizada em muitas áreas de engenharia, principalmente na área de processamento de sinais, onde se trabalham com sinais conhecidos como 
sinais periódicos, que seguem ciclos e se repetem.
No processamento digital de imagens ela também é uma ferramenta fundamental, utilizada para transformar a imagem de um domínio especial para um domínio de frequências bidimensionais.

---

## 2. Objetivo


O Objetivo dessa atividade é trabalhar como é feito o cálculo da transformada de fourier sobre uma imagem e quais os passos necessários para a exibição e manipulação.

---


## 3. Metodologia

### Imagens como Funções bidimensionais
Uma imagem em escala cinza é definida como uma função de duas variáveis que recebe uma posição, que é composta por uma linha(x) e uma coluna(y), e retorna o valor de intensidade de brilho nessa
mesma região, e pode ser representada como

$$
z = f(x, y)
$$
onde comumente z é um número entre 0 e 255, que são os números de bits(8) necessários para representar 256 valores.


### Função Imagem Senoidal
Uma função senoidal pode definir uma imagem periódica, especificamente, podemos ter uma função senoidal nas linhas ou nas colunas, definindo

$$
z = f(x, y) = Asen(\frac{2 \pi f x}{M}) + D
$$

ou uma função senoidal periódica nas colunas

$$
z = f(x, y) = Asen(\frac{2 \pi f y}{N}) + D
$$

onde M e N são os números de linhas e colunas, respectivamente.

### Transformada de fourier bidimensional
A transformada de fourier sobre uma imagem é definida como uma integral dupla sobre o círculo unitário, dada por

$$
F(u,v) = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x,y)e^{-2 \pi (ux+vy)} dx dy
$$

porém as imagens são discretas, e uma aproximação discretizada da expressão acima pode ser obtida, adimitindo a imagem como uma soma de um trem de impulsos contínuos de duas variáveis, tem-se

$$
f(x = iT_m, y = nT_n) = \overline{f}(x, y) = \sum_{i}^{} \sum_{n}^{} f(x,y)\cdot \delta(x - iT_m) \delta(y - nT_n)
$$

considerando um período constante e uniforme para as amostras da imagem, temos 

$$
T_m = 1/M
$$

e

$$
T_n = 1/N
$$

e ainda simplificando a expressão temos

$$
\overline{f}(x, y) = f(x,y) \sum_{i}^{} \sum_{n}^{} \delta(x - iT_m) \delta(y - nT_n)
$$

substituindo agora essa função na expressão da transformada de fourier temos

$$
F(u,v) = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} \sum_{i}^{} \sum_{n}^{} f(x,y) \delta(x - iT_m) \delta(y - nT_n) e^{-2 \pi (ux+vy)} dx dy
$$

podemos rearranjar agora a integral e os somatórios, e a expressão acima se torna

$$
F(u,v) = \int_{-\infty}^{\infty} \sum_{n}^{} \delta(y - nT_n)  \sum_{i}^{} \int_{-\infty}^{\infty} f(x,y) \delta(x - iT_m) e^{-2 \pi ux} dx e^{-2 \pi vy} dy
$$

e a integral interna pode ser resolvida utilizando a noção de integrais com impulsos, e é dada por 

$$
\int_{-\infty}^{\infty} f(x,y) \delta(x - iT_m) e^{-2 \pi ux} dx = f(iT_m,y) e^{-2 \pi uiT_m}
$$

assim ficamos com 

$$
F(u,v) = \int_{-\infty}^{\infty} \sum_{n}^{} \delta(y - nT_n)  \sum_{i}^{} f(iT_m,y) e^{-2 \pi uiT_m}  e^{-2 \pi vy}dy
$$

agora reorganizando mais uma vez e trocando a ordem do somatório com a integral

$$
F(u,v) = \sum_{i}^{} e^{-2 \pi uiT_m}  \sum_{n}^{}  \int_{-\infty}^{\infty} f(iT_m,y) \delta(y - nT_m) e^{-2 \pi vy}dy
$$

e utilizando denovo o conceito de integrais com impulso, finalmente temos

$$
F(u,v) = \sum_{i}^{} e^{-2 \pi uiT_m}  \sum_{n}^{}  f(iT_m,nT_n) e^{-2 \pi v\frac{n}{N}} = \sum_{i}^{} \sum_{n}^{} f(iT_m,nT_n) e^{-2 \pi (uiT_m + vnT_n)}
$$

e a expressão final normalizada se torna

$$
F(u,v) = \frac{1}{M} \frac{1}{N} \sum_{i}^{} \sum_{n}^{} f(i,n) e^{-2 \pi (u\frac{i}{M} + v\frac{n}{N})}
$$

que é conhecida como a transformada discreta de fourier bidimensional.


### 3.1. Implementação
Foi então utilizado o código do professor como referência para as operações computacionais com a DFT, tendo toda a preparação da imagem para tal, e assim com as devidas alterações o código ficou 

* Código

```



```


## 4. Resultados

### Exibição da transformada de fourier 
Percebeu-se que ao contrário do exemplo feito pelo professor, a imagem aqui requisitada era periódica nas linhas, pois dependia da i-ésima linha x (i).

![Imagem gerada pela função senoide](./imagens/imagem_periodica.png)



---

## 5. Conclusão

A manipulação e a serialização de dados é muito importante no processamento digital de imagens e nos pode dar uma noção das possíveis manipulações e formatos que podemos definir para uma imagem, e a implementação da imagem como uma função de duas variáveis também é capaz de nos dar uma melhor intuição ao trabalhar com imagens como se fosse funções de duas variáveis.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
