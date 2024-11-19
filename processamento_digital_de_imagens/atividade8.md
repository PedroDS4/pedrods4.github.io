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
\int_{-\infinity}^{\infinity} \int_{-\infinity}^{\infinity} f(x,y)e^{2 \pi (ux+vy)} dx dy
$$

porém as imagens são discretas, e uma discretização da expressão acima pode ser obtida, adimitindo a imagem como uma soma de um trem de impulsos de duas variáveis, tem-se

$$
f(x = mT_m, y = nT_n) = f(m,n) = \sum_{}^{} \sum_{}^{} f(x,y)\cdot \delta(x - T_m) \delta(x - T_n)
$$

substituindo agora essa função na integral temos

### 3.1. Implementação
Foi então implementado como no exemplo mostrado pelo professor, uma imagem com uma função senoidal dependende das linhas, e construído o gráfico no erro para uma linha fixa x = 5 para fins de comparação. 

* Código

```



```


## 4. Resultados

### Exibição da transformada de fourier 
Percebeu-se que ao contrário do exemplo feito pelo professor, a imagem aqui requisitada era periódica nas linhas, pois dependia da i-ésima linha x (i).

![Imagem gerada pela função senoide](./imagens/imagem_periodica.png)

o gráfico do erro tem uma diferença que para ser notada, precisou ser multiplicada por 100, que é a diferença entre os números salvos em yml(float) e em png(uchar), percebeu-se que o erro foi constante ao longo da linha, o que pode significa um erro pequeno nas casas decimais.

![Imagem do gráfico do erro em função da linha](./imagens/grafico_erro_linha.png)

---

## 5. Conclusão

A manipulação e a serialização de dados é muito importante no processamento digital de imagens e nos pode dar uma noção das possíveis manipulações e formatos que podemos definir para uma imagem, e a implementação da imagem como uma função de duas variáveis também é capaz de nos dar uma melhor intuição ao trabalhar com imagens como se fosse funções de duas variáveis.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
