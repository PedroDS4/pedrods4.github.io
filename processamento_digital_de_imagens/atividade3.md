<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

#**Relatório Atividade 2: Serialização de dados**

# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 25/10

## 1. Introdução

No campo do processamento digital de imagens a serialização ou salvamento de dados de uma imagem é importante por causa da precisão que um formato tem, e em comparação com outros formatos analisar o melhor tipo de salvamento.
O estudo das imagens como funções bidimensionais de duas variáveis tem várias aplicações nos estudos de processamento digital de imagens e nas suas aplicações, principalmente das várias ferramentas
que as funções nos permitem usufruir.

---

## 2. Objetivo

O Objetivo dessa atividade é trabalhar a serialização de dados para imagens, e também gerar imagens como funções bidimensionais a partir de uma função de intensidade luminosa senoidal bidimensional.
---

## 3. Metodologia

### Imagens como Funções bidimensionais
Uma imagem em escala cinza é definida como uma função de duas variáveis que recebe uma posição, que é composta por uma linha(x) e uma coluna(y), e retorna o valor de intensidade de brilho nessa
mesma região, e pode ser representada como

$$
z = f(x, y)
$$

onde comumente z é um número entre 0 e 255, que são os números de bits(8) necessários para representar 256 valores

### 3.1. Implementação
As funções foram feitas usando 
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
