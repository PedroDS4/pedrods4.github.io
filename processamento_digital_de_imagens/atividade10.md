<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

#**Relatório Atividade 9: Arte com Processamento digital de imagens: O Pointilhismo**

# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 05/12

## 1. Introdução

O processamento digital de imagens nos permite a manipulação de imagens na camada mais profunda, que são as matrizes, e com isso 
é possível gerar muitos resultados bonitos e interessantes usando as técnicas certas, como por exemplo é comum a arte em
imagens.

---

## 2. Objetivo

O Objetivo dessa atividade é utilizar ferramentas de manipulação dos pixels de uma imagem para produzir efeitos curiosos
e bonitos em uma imagem, e aplicar uma técnica que gere uma imagem pointilhista.

---


## 3. Metodologia

### Descrevendo minha técnica
Para cada pixel da imagem
- Escolher um treshhold aleatório pro algorítmo de canny para aquela iteração
- Aplicar o algorítmo de canny para obter as bordas naquela iteração
- Gerar assim como o professor uma localização aleatória em torno do pixel para desenhar o circulo de perturbação
- Desenhar o círculo na localização perturbada calculada anteriormente
- Somar a imagem de pontos atual com as bordas dessa iteração


### 3.1. Implementação da técnica
Foi então utilizado o código do professor como referência para a extração das bordas com o algorítmo de canny e também para a implementação da técnica pontilhista, e finalmente feito a adaptação para a técnica descrita acima, que utiliza números aleatórios e as bordas para deixar a imagem com um efeito visual de borda forte.

* Código 

```


#include <algorithm>
#include <cstdlib>
#include <ctime>
#include <fstream>
#include <iomanip>
#include <iostream>
#include <numeric>
#include <opencv2/opencv.hpp>
#include <vector>
#include <chrono>
#include <random>

#define STEP 5
#define JITTER 3

int main(int argc, char** argv) {
  std::vector<int> yrange;
  std::vector<int> xrange;

  cv::Mat image, frame, points;
  cv::Mat border;
  int width, height, gray;
  int x, y;
  //int RAIO;
  image = cv::imread(argv[1], cv::IMREAD_GRAYSCALE);

  std::srand(std::time(0));

  if (image.empty()) {
    std::cout << "Could not open or find the image" << std::endl;
    return -1;
  }
    
  width = image.cols;
  height = image.rows;

  xrange.resize(height / STEP);
  yrange.resize(width / STEP);

  std::iota(xrange.begin(), xrange.end(), 0);
  std::iota(yrange.begin(), yrange.end(), 0);

  for (uint i = 0; i < xrange.size(); i++) {
    xrange[i] = xrange[i] * STEP + STEP / 2;
  }

  for (uint i = 0; i < yrange.size(); i++) {
    yrange[i] = yrange[i] * STEP + STEP / 2;
  }

  points = cv::Mat(height, width, CV_8U, cv::Scalar(255));

  unsigned seed = std::chrono::system_clock::now().time_since_epoch().count();
  std::shuffle(xrange.begin(), xrange.end(), std::default_random_engine(seed));

  for (auto i : xrange) {
  std::shuffle(yrange.begin(), yrange.end(), std::default_random_engine(seed));
    for (auto j : yrange) {

      int T = std::rand() % (JITTER) - JITTER + 1;
      cv::Canny(image, border, T, 3*T);
      double RAIO = std::rand()%(JITTER)+1;
      x = i + std::rand() % (2 * JITTER) - JITTER + 1;
      y = j + std::rand() % (2 * JITTER) - JITTER + 1;
      gray = image.at<uchar>(x, y);
      cv::circle(points, cv::Point(y, x), RAIO, CV_RGB(gray, gray, gray), cv::FILLED, cv::LINE_AA);
      cv::add(points, border, points);


    }
  }

  cv::imwrite("pontos.jpg", points);
  return 0;
}




```



## 4. Resultados

### Resultado da implementação da técnica de arte
Podemos ver que ao somar a borda a imagem continuamente, criou-se um efeito visual de borda forte e um expressionismo na imagem, causando
uma certa sensação estranha.


![Imagem da arte implementada](./imagens/pontos.jpg)

*Figura 1: Resultado da técnica desenvolvida.*

---

## 5. Conclusão

Vimos que a técnica desenvolvida, apesar de estranha a priori, produziu um efeito curioso, como muitas técnicas de arte, que tem o poder de encantar nosso cérebro e
nos dar uma percepção da beleza das coisas.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
