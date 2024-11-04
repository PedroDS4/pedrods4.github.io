<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

# **Relatório Atividade 6: Filtragem no domínio espacial **

# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 4/11

## 1. Introdução

Uma das operações mais importantes ao se trabalhar com imagens bidimensionais é a convolução bidimensional, que tem inúmeras aplicações no campo do processamento digital de imagens, principalmente o de realizar uma filtragem, borramento ou detecção de bordas.

---

## 2. Objetivo

O Objetivo dessa atividade é explorar o uso da operação de convolução de uma imagem com "máscaras" de convolução predefinidas especiais que ja foram estudadas, e assim entender melhor como isso pode ser útil no processamento digital de imagens, serão explorados o uso do filtro de média para realizar borramento de uma imagem, e do filtro laplaciano para realçar as características de uma imagem.

---

## 3. Metodologia

### Exercício 1:
Utilizando o programa exemplos/convolucao.cpp como referência, implemente um programa convolucao2.cpp. O programa deverá realizar a convolução com o filtro da média para máscaras de tamanho 11×11
e 21×21 pixels. Compare os resultados obtidos com o filtro de tamanho 3×3 pixels.

Foi adaptado o código ```convolução.cpp``` fornecido pelo professor na pagina da disciplina para fazer o borramento com um filtro de tamanho 11x11 e 21x21.

### Exercício 2: 
Observe o efeito presente no vídeo Exemplo do efeito de produndidade de campo (arquivo vasos720p.mp4). Este efeito é conhecido como profundidade de campo. Ele ocorre porque o conjunto de lentes não permite, para a abertura do diafragma, que todos os objetos em diferentes distâncias do plano focal sejam focados simultaneamente. Há situações em que explorar a profundidade de campo é interessante, como em retratos, para deixar o fundo da cena desfocado. Em outros casos, como em imagens de microscopia, pode não ser possível ajustar as objetivas do microscópio para deixar todos os elementos da cena focados, dificultando a análise completa do objeto em estudo.

Entretanto, é possível oferecer alguma correção para o efeito de profundidade de campo em imagens digitais tomadas de uma cena sem movimento, como esta do vídeo apresentado. Utilizando o programa exemplos/convolucao.cpp como referência, implemente um programa depthoffield.cpp que funcione conforme os seguintes passos:

Capture um frame da cena do vídeo.

Converta o frame para tons de cinza.

Crie uma matriz para guardar os máximos dos laplacianos que serão calculados a seguir.

Aplique um filtro laplaciano de tamanho 3×3
 pixels na imagem cinzenta.

Compare o resultado do filtro com a matriz de máximos e selecione aqueles que possuem um valor maior que o correspondente na matriz de máximos. Para os pixels selecionados, copie para a imagem de saída os pixels coloridos da imagem capturada.

Adaptando o código convolucao.cpp para realizar a convoluçaõ com a máscara da aproximação do laplaciano, temos que 

---
### 3.1. Implementação



### Exercício 1:
* Código Implementado

```



```



### Exercício 3:
* Código Implementado

```



```

## 4. Resultados

### Exercício 1:
Foi possível observar, como na imagem abaixo, que quanto maior o valor do número de pixel N, maior o borramento, como mostram as imagens abaixo

![Imagem borrada com máscara 3x3](./imagens/borramento3x3.png)

*Figura 1: Imagem borrada com máscara 3x3.*


![Imagem borrada com máscara 11x11](./imagens/borramento11x11.png)

*Figura 1: Imagem borrada com máscara 11x11.*

![Imagem borrada com máscara 21x21](./imagens/borramento3x3.png)

*Figura 1: Imagem borrada com máscara 21x21.*


### Exercício 2: 

![Imagem borrada com máscara 3x3](./imagens/borramento3x3.png)


---

## 5. Conclusão

As aplicações de filtragem no domínio espacial demonstradas nesse relatório foram implementadas para fazerem operações muito interessantes, e que são muito úteis no dia a dia, assim sendo um conteúdo 

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
