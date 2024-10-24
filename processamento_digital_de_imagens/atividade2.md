<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../../index.md)
#**Relatório Atividade 3: Esteganografia**
# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 25/10

## 1. Introdução

No campo do processamento digital de imagens, a esteganografia é uma técnica que ja foi muito utilizada para criptografar imagens e escondê-las em outras imagens, utilizando manipulações de bits.

---

## 2. Objetivo

O objetivo deste experimento é implementar e analisar a operação de negativo de uma imagem, além de realizar a inversão de quadrantes, observando os efeitos resultantes dessas transformações.

---

## 3. Metodologia

### Operação de negativo
A operação de negativo de uma imagem foi realizada utilizando o código escrito em [linguagem utilizada, ex.: Python]. A fórmula matemática utilizada para realizar o negativo de uma imagem em escala de cinza é expressa por:

$$
g(x, y) = 255 - f(x, y)
$$

Onde:
- $$f(x, y)$$  representa o valor de intensidade de cinza do pixel na posição $$(x, y)$$ da imagem original,
- $$g(x, y)$$ é o valor correspondente do pixel na imagem negativa,
- $$255$$ é o valor máximo de intensidade em uma imagem de 8 bits, o que permite obter o complemento do valor de intensidade original.

A operação foi aplicada pixel a pixel, modificando cada ponto da imagem de acordo com a equação acima.

### Operação de Inversão de quadrantes
Além da operação de negativo, foi implementada a técnica de inversão de quadrantes. Esta operação consiste em dividir a imagem em quatro quadrantes e trocar suas posições, de modo a reordenar visualmente a disposição dos pixels.
A equação matemática da inversão de quadrantes é simplesmente a troca das posições dos pixels de uma certa região por outra.
Temos que m é o número de linhas da imagem(eixo x) e n o número de colunas(eixo y), temos então que para uma nova imagem $$g(x,y)$$ :

* Primeiro quadrante 

$$
g(x,y) = f(x+m/2,y+m/2)  
$$

* Segundo quadrante

$$
g(x,y+m/2) = f(x+m/2,y)  
$$

* Terceiro quadrante

$$
g(x+m,y) = f(x,y+m/2)  
$$

* Quarto quadrante

$$
g(x+m/2,y+m/2) = f(x,y)  
$$

e conseguimos fazer isso tudo apenas em um laço.


### 3.1. Implementação
As funções foram feitas usando uma classe ponto para facilitar a entrada e a manipulação dos pixels.
As funções implementadas estão mostradas abaixo:
* Código da operação de negativo
```
include<iostream> 
```

* Código da operação de inversão de quadrantes
```
$include<iostream>
```

## 4. Resultados

### Operação de negativo
A operação de negativo produz um efeito visual interessante, onde as áreas mais escuras da imagem original tornam-se claras e vice-versa. Isso permite uma nova percepção da cena, podendo realçar detalhes que não são tão evidentes na imagem original.

Na imagem original, as áreas com maiores valores de $f(x, y)$ são transformadas em áreas de menor valor de $g(x, y)$, o que resulta em um efeito de contraste invertido. O efeito garante que o valor de intensidade de cinza de cada pixel seja subtraído de 255, criando essa inversão.


### Operação de inversão de quadrantes
A inversão dos quadrantes introduziu uma distorção espacial na imagem, onde as partes da imagem foram reorganizadas, criando um novo padrão visual que pode ser útil para efeitos artísticos ou análises de simetria.


---

## 5. Conclusão

A operação de negativo de uma imagem é uma técnica simples, mas eficaz, para a manipulação de imagens digitais. Ao substituir cada valor de intensidade de pixel pelo seu complemento, conseguimos inverter o contraste da imagem, o que pode ser útil em diversas aplicações, como em processamento médico, onde inversões de contraste podem revelar detalhes ocultos.

A inversão dos quadrantes, por sua vez, altera a disposição espacial dos pixels, criando uma nova organização visual da imagem. Essas técnicas juntas demonstram como manipulações básicas no processamento de imagens podem resultar em efeitos visuais significativos e úteis.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
