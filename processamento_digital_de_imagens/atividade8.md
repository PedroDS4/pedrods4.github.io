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
z = f(x, y) = Asen(\frac{2*\pi*f*x}{M}) + B
$$

ou uma função senoidal periódica nas colunas

$$
z = f(x, y) = Asen(\frac{2*\pi*f*y}{N}) + B
$$

onde M e N são os números de linhas e colunas, respectivamente.

### Transformada de fourier bidimensional
A transformada de fourier sobre uma imagem é definida como uma integral dupla sobre o círculo unitário, dada por

$$
F(u,v) = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x,y)e^{-2 \pi (ux+vy)} dx dy
$$

porém as imagens são discretas, e uma discretização da expressão acima pode ser obtida, adimitindo a imagem como uma soma de um trem de impulsos de duas variáveis, tem-se

$$
f(x = mT_m, y = nT_n) = f(m,n) = \sum_{i}^{} \sum_{k}^{} f(x,y)\cdot \delta(x - i) \delta(x - k)
$$

e ainda simplificando temos

$$
f(m,n) = f(x,y) \sum_{i}^{} \sum_{k}^{} \delta(x - i) \delta(y - k)
$$

substituindo agora essa função na expressão da transformada de fourier temos

$$
F(u,v) = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} \sum_{i}^{} \sum_{k}^{} f(x,y) \delta(x - i) \delta(y - k) e^{-2 \pi (ux+vy)} dx dy
$$

podemos rearranjar agora a integral e os somatórios, e a expressão acima se torna

$$
F(u,v) = \int_{-\infty}^{\infty} \sum_{k}^{} \delta(y - k)  \sum_{i}^{} \int_{-\infty}^{\infty} f(x,y) \delta(x - i) e^{-2 \pi ux} dx e^{-2 \pi vy}dy
$$

e a integral interna pode ser resolvida utilizando a noção de integrais com impulsos, e é dada por 

$$
\int_{-\infty}^{\infty} f(x,y) \delta(x - i) e^{-2 \pi ux} dx = f(i,y) e^{-2 \pi ui}
$$

assim ficamos com 

$$
F(u,v) = \int_{-\infty}^{\infty} \sum_{k}^{} \delta(y - k)  \sum_{i}^{} f(i,y) e^{-2 \pi ui}  e^{-2 \pi vy}dy
$$

agora reorganizando mais uma vez e trocando a ordem do somatório com a integral

$$
F(u,v) = \sum_{i}^{} e^{-2 \pi ui}  \sum_{k}^{}  \int_{-\infty}^{\infty} f(i,y) \delta(y - k) e^{-2 \pi vy}dy
$$

e utilizando denovo o conceito de integrais com impulso, finalmente temos

$$
F(u,v) = \sum_{i}^{} e^{-2 \pi ui}  \sum_{k}^{}  f(i,k) e^{-2 \pi vk} = \sum_{i}^{} \sum_{k}^{} f(i,k) e^{-2 \pi (ui + vk}
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
