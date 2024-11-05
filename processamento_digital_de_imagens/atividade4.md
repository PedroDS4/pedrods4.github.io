<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

# ** Relatório Atividade 4: Preenchendo Regiões **

# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 25/10

## 1. Introdução

No campo do processamento digital de imagens, existem vários algorítmos que exploram o conceito de vizinhança dos pixels de uma imagem, os mais conhecidos são os algoritmos do tipo floodFill ou seedFill, que são usados para percorrer a vizinhança de uma imagem a partir de um critério.
A utilidade desses algoritmos é variada porém são muito usados para achar certos objetos, certas regiões e até certas cores em alguma imagem, como nesta atividade.

---

## 2. Objetivo

O Objetivo dessa atividade é explorar o uso do algoritmo floodFill para uma situação bem específica de achar objetos em uma iamagem binária e classificá-los usando o algorítmo labeling, e assim adaptar o algoritmo fornecido pelo professor para achar objetos com buracos ou não, e que estão fora da borda.


---

## 3. Metodologia
Para primeiro exclir os objetos das bordas, foi realizado um floodFill prévio na imagem que classificou e eliminou(pintando de preto) os objetos brancos que tocavam a borda, para só depois executar o floodFill para achar os objetos que tinham buracos e também os que não tinham.
Para conseguir achar os objetos que tinham buracos, para cada objeto achado, é executado um segundo for para achar os buracos dentro desse objeto, ou seja, detectar uma mudança de branco para preto dentro do objeto.

### Exercício 1: 
Observando-se o programa labeling.cpp como exemplo, é possível verificar que caso existam mais de 255 objetos na cena, o processo de rotulação poderá ficar comprometido, visto que o tipo de dado usado para suportar imagens cinzentas permitem armazenar apenas um byte por pixel. Identifique a situação em que isso ocorre e proponha uma solução para este problema.

Caso a imagem tenha mais de 255 objetos a serem classificados, não será mais possível classificá-los a partir do tom de cinza incrementado a partir do zero, ja que existem apenas 255 tons de cinza possíveis, então a solução seria classificar os objetos não a partir do tom de cinza mas sim a partir de um número, por exemplo seria possível desenhar um número de um certo tamanho no centro da imagem, porém seria um algoritmo um pouco mais trabalhoso de implementar.

### Exercício 2:
Aprimore o algoritmo de contagem apresentado para identificar regiões com ou sem buracos internos que existam na cena. Assuma que objetos com mais de um buraco podem existir. Inclua suporte no seu algoritmo para não contar bolhas que tocam as bordas da imagem. Não se pode presumir, a priori, que elas tenham buracos ou não.

Os resultado foram satisfeito, ja que foram achados exatamente x objetos com buracos e y objetos sem buracos, que são exatamente o que existem na imagem.

---
### 3.1. Implementação
Foi então implementado a adapatação do algoritmo labeling.cpp fornecido pelo professor, para detectar objetos que estavam nas bordas, fazendo um floodfill na imagem procurando por objetos de bordas

```
// Flood Fill para retirar os objetos das bordas
  for (int i = 0; i < width; i++) {
    if (image.at<uchar>(0, i) == 255) cv::floodFill(image, Point(i, 0), 128);
    if (image.at<uchar>(height-1, i) == 255) cv::floodFill(image, Point(i, height-1), 128);
  }
  for (int i = 0; i < height; i++) {
    if (image.at<uchar>(i, 0) == 255) cv::floodFill(image, Point(0, i), 128);
    if (image.at<uchar>(i, width-1) == 255) cv::floodFill(image, Point(width-1, i), 128);
  }
    
```

os dois for separadamente percorrem primeiro as colunas de cima e de baixo, para procurar os pixels brancos de borda

e depois foi feita a verificação de existiam buracos na imagem, com o trecho

```
      bool has_holes = false;
        for (int y = i; y < height; y++) {
          for (int x = j; x < width; x++) {
            // Verifica se um ponto preto está cercado pela bolha
            if (image.at<uchar>(y, x) == 0 && 
               (image.at<uchar>(y, x+1) == nobjects)) {

              // Marca o buraco com uma cor temporária para evitar duplicações
              cv::floodFill(image, Point(x, y), 127); // 127 para diferenciar de outras áreas
              has_holes = true;
            }
          }
        }

```



Assim, o código final ficou
* Código final
  
```
#include <iostream>
#include <opencv2/opencv.hpp>

using namespace cv;

int main(int argc, char** argv) {
  cv::Mat image;
  int width, height;
  int nobjects = 0;
  int nobjects_with_holes = 0;
  cv::Point p;

  // Carrega a imagem em escala de cinza
  image = cv::imread("bolhas.png", cv::IMREAD_GRAYSCALE);

  if (!image.data) {
    std::cout << "imagem nao carregou corretamente\n";
    return (-1);
  }

  width = image.cols;
  height = image.rows;
  std::cout << width << "x" << height << std::endl;

  // Remove objetos conectados às bordas
  for (int i = 0; i < width; i++) {
    if (image.at<uchar>(0, i) == 255) cv::floodFill(image, Point(i, 0), 0);
    if (image.at<uchar>(height - 1, i) == 255) cv::floodFill(image, Point(i, height - 1), 0);
  }
  for (int i = 0; i < height; i++) {
    if (image.at<uchar>(i, 0) == 255) cv::floodFill(image, Point(0, i), 0);
    if (image.at<uchar>(i, width - 1) == 255) cv::floodFill(image, Point(width - 1, i), 0);
  }

  // Conta os objetos e verifica buracos
  for (int i = 0; i < height; i++) {
    for (int j = 0; j < width; j++) {
      if (image.at<uchar>(i, j) == 255) {
        // Encontrou uma nova bolha
        nobjects++;

        // Define um ID único para a bolha atual
        p.x = j;
        p.y = i;

        // Marca a bolha com o ID atual
        cv::floodFill(image, p, nobjects);

        // Verifica por buracos dentro da área da bolha
        bool has_holes = false;
        for (int y = i; y < height; y++) {
          for (int x = j; x < width; x++) {
            // Verifica se um ponto preto está cercado pela bolha
            if (image.at<uchar>(y, x) == 0 && 
               (image.at<uchar>(y, x+1) == nobjects)) {

              // Marca o buraco com uma cor temporária para evitar duplicações
              cv::floodFill(image, Point(x, y), 127); // 127 para diferenciar de outras áreas
              has_holes = true;
            }
          }
        }
       

        // Conta a bolha com buraco, se encontrado
        if (has_holes) nobjects_with_holes++; 
      }
    }
  }

  std::cout << "A figura tem " << nobjects << " bolhas que não tocam as bordas e " 
            << nobjects_with_holes << " bolhas com buracos. \n";

  cv::imshow("image", image);
  cv::imwrite("labeling.png", image);
  cv::waitKey();
  return 0;
}

```

## 4. Resultados

Foi então executado o novo código e verificamos que a contagem de objetos com e sem buracos foi implementada corretamente, resultando em 21 objetos e 7 objetos sem buracos.

![Contagem de objetos com e sem buracos](./imagens/buracos.png)


---

## 5. Conclusão

A detecção de objetos em uma imagem é uma aplicação importantíssima na industria de todas as engenharias, podendo ser muito útil e ter algoritmos adaptados para cada situação, e dessa maneira a exploração desses algorítmos são importantíssimos em uma disciplina de processamento digital de imagens.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
