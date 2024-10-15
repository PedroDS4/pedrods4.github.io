# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

### **Título do Relatório: Operação de Negativo e Inversão de Quadrantes de Imagem**

**Aluno(a): [Pedro Arthur Oliveira dos Santos]**  
**Professor(a): [Agostinho Brito Junior]**  
**Data: [25/10]**

---

## 1. Introdução

No campo do processamento digital de imagens, a operação de negativo de uma imagem é uma das transformações mais simples e amplamente utilizadas para realçar certas características visuais. O negativo de uma imagem consiste em uma operação que altera os valores de intensidade de cada pixel, invertendo suas cores. Em uma imagem em escala de cinza, isso significa substituir o valor de intensidade de cada pixel por seu complementar na escala de 0 a 255, de forma que áreas claras se tornam escuras e áreas escuras se tornam claras.

O objetivo deste relatório é apresentar a implementação da operação de negativo aplicada a uma imagem digital e discutir seus efeitos visuais, além de abordar a inversão de quadrantes da imagem como uma forma adicional de manipulação.

---

## 2. Objetivo

O objetivo deste experimento é implementar e analisar a operação de negativo de uma imagem, além de realizar a inversão de quadrantes, observando os efeitos resultantes dessas transformações.

---

## 3. Metodologia

A operação de negativo de uma imagem foi realizada utilizando o código escrito em [linguagem utilizada, ex.: Python]. A fórmula matemática utilizada para realizar o negativo de uma imagem em escala de cinza é expressa por:

\[
g(x, y) = 255 - f(x, y)
\]

Onde:
- \(f(x, y)\) representa o valor de intensidade de cinza do pixel na posição \((x, y)\) da imagem original,
- \(g(x, y)\) é o valor correspondente do pixel na imagem negativa,
- 255 é o valor máximo de intensidade em uma imagem de 8 bits, o que permite obter o complemento do valor de intensidade original.

A operação foi aplicada pixel a pixel, modificando cada ponto da imagem de acordo com a equação acima.

Além da operação de negativo, foi implementada a técnica de inversão de quadrantes. Esta operação consiste em dividir a imagem em quatro quadrantes e trocar suas posições, de modo a reordenar visualmente a disposição dos pixels.

---

## 4. Resultados

A operação de negativo produz um efeito visual interessante, onde as áreas mais escuras da imagem original tornam-se claras e vice-versa. Isso permite uma nova percepção da cena, podendo realçar detalhes que não são tão evidentes na imagem original.

Na imagem original, as áreas com maiores valores de \(f(x, y)\) são transformadas em áreas de menor valor de \(g(x, y)\), o que resulta em um efeito de contraste invertido. A fórmula \(g(x, y) = 255 - f(x, y)\) garante que o valor de intensidade de cinza de cada pixel seja subtraído de 255, criando essa inversão.

A inversão dos quadrantes introduziu uma distorção espacial na imagem, onde as partes da imagem foram reorganizadas, criando um novo padrão visual que pode ser útil para efeitos artísticos ou análises de simetria.

---

## 5. Conclusão

A operação de negativo de uma imagem é uma técnica simples, mas eficaz, para a manipulação de imagens digitais. Ao substituir cada valor de intensidade de pixel pelo seu complemento, conseguimos inverter o contraste da imagem, o que pode ser útil em diversas aplicações, como em processamento médico, onde inversões de contraste podem revelar detalhes ocultos.

A inversão dos quadrantes, por sua vez, altera a disposição espacial dos pixels, criando uma nova organização visual da imagem. Essas técnicas juntas demonstram como manipulações básicas no processamento de imagens podem resultar em efeitos visuais significativos e úteis.

---

## 6. Referências

(Nesse campo, você pode adicionar as referências bibliográficas que utilizar, de acordo com as normas da ABNT.)
