<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)


# Universidade Federal do Rio Grande do Norte

# Projeto Final da Disciplina: Deconvolução de Imagens usando otimização

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 15/01/2025

## Introdução

O objetivo deste projeto é desenvolver uma técnica de deconvolução de imagens no domínio da frequência para estimar a imagem original borrada a partir de uma imagem observada \( Y \). A abordagem adotada envolve o uso de máscaras gaussianas como função de borramento, modeladas como uma combinação de funções de base radial. O problema central é estimar a imagem \( X \) original, além dos parâmetros da máscara de borramento, como os coeficientes de amplitude, centros e desvio padrão das gaussianas.

A deconvolução é realizada através da minimização de um erro entre a imagem observada \( Y \) e a imagem estimada \( G * X \), onde \( G \) é o filtro de borramento e \( X \) é a imagem original.

## Metodologia

### Definição do Problema

O problema de deconvolução pode ser formulado como a minimização da seguinte função de erro \( E(X, G) \):

\[
E(X, G) = || Y - G * X ||^2
\]

Onde:
- \( Y \) é a imagem observada,
- \( G \) é a máscara de borramento, modelada como uma combinação de gaussianas,
- \( X \) é a imagem original que queremos estimar.

### Máscaras Gaussianas

A máscara de borramento \( G \) é modelada como uma soma ponderada de gaussianas bidimensionais. Cada gaussiana tem parâmetros:
- \( a_i \) como coeficiente de amplitude,
- \( (u_i, v_i) \) como centro da gaussiana,
- \( \sigma_i \) como desvio padrão que controla a largura da gaussiana.

Assim, a máscara \( G(x, y) \) é expressa como:

\[
G(x, y) = \sum_{i} a_i \exp\left( -\frac{(x - u_i)^2 + (y - v_i)^2}{2 \sigma_i^2} \right)
\]

Onde \( (x, y) \) são as coordenadas da imagem.

### Estimativa dos Parâmetros

Os parâmetros da máscara de borramento, bem como a imagem \( X \), são estimados por meio de um processo de otimização. A função de erro \( E(X) \) é minimizada em relação a \( X \) e os parâmetros da máscara \( G \). O algoritmo de otimização utilizado neste projeto é a descida de gradiente.

A otimização é realizada de forma a ajustar tanto a imagem \( X \) quanto os parâmetros \( a_i, u_i, v_i, \sigma_i \) das gaussianas.

### Gradientes

A função de erro \( E(X) \) é diferenciada em relação a \( X \) e aos parâmetros de \( G \). A derivada de \( E(X) \) em relação a \( X \) é dada por:

\[
\frac{\partial E}{\partial X} = -2 G^T (Y - G * X)
\]

Além disso, as derivadas de \( E(X) \) em relação aos parâmetros da gaussiana são calculadas como:

\[
\frac{\partial E}{\partial a_i} = -2 \sum_{x, y} \left( Y(x, y) - (G * X)(x, y) \right) \cdot X(x, y) \cdot \exp\left( -\frac{(x - u_i)^2 + (y - v_i)^2}{2 \sigma_i^2} \right)
\]

\[
\frac{\partial E}{\partial u_i} = -2 \sum_{x, y} \left( Y(x, y) - (G * X)(x, y) \right) \cdot X(x, y) \cdot \frac{(x - u_i)}{\sigma_i^2} \exp\left( -\frac{(x - u_i)^2 + (y - v_i)^2}{2 \sigma_i^2} \right)
\]

De maneira análoga, temos a derivada em relação a \( v_i \) e \( \sigma_i \).

## Implementação

A implementação do projeto envolve as seguintes etapas principais:
1. **Pré-processamento da imagem**: A imagem observada \( Y \) é carregada e pré-processada para garantir que esteja em formato adequado para a aplicação de convolução e deconvolução.
2. **Criação da máscara de borramento \( G \)**: A máscara \( G \) é inicialmente estimada usando gaussianas de parâmetros arbitrários. Para isso, um conjunto inicial de valores para \( a_i, u_i, v_i, \sigma_i \) é escolhido.
3. **Otimização**: Um algoritmo de descida de gradiente é utilizado para minimizar a função de erro \( E(X) \) em relação a \( X \) e os parâmetros da máscara \( G \).
4. **Resultado**: A imagem estimada \( X \) é recuperada, e os parâmetros de borramento são ajustados.

## Resultados

Após a execução da otimização, a imagem \( X \) estimada é comparada com a imagem original para avaliar a eficácia do algoritmo de deconvolução. A precisão é medida com base na redução do erro quadrático médio entre a imagem original e a estimada.

Além disso, a reconstrução da máscara \( G \) mostra como os parâmetros das gaussianas se ajustaram para aproximar a máscara de borramento real utilizada para gerar a imagem observada.

## Conclusão

O projeto demonstrou a viabilidade de estimar uma imagem original borrada através da deconvolução no domínio da frequência, utilizando uma máscara de borramento composta por gaussianas. A abordagem baseada em otimização e gradientes mostrou-se eficaz para a recuperação da imagem original e para a estimativa dos parâmetros da máscara de borramento.

A metodologia pode ser expandida para diferentes tipos de filtros de borramento e aplicada a cenários mais complexos, como imagens com diferentes níveis de ruído ou com borramentos não uniformes.


## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
