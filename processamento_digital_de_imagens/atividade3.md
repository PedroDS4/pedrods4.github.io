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

### Construindo um gráfico em uma imagem bidimensional em escala cinza



### 3.1. Implementação
Foi então implementado como no exemplo mostrado pelo professor, uma imagem com uma função senoidal dependende das linhas, e construído o gráfico no erro para uma linha fixa x = 5 para fins de comparação. 

* Código 
```
#include <iostream>
#include <opencv2/opencv.hpp>
#include <sstream>
#include <string>

int SIDE = 256;
int PERIODOS = 4;

int main(int argc, char** argv) {
  std::stringstream ss_img, ss_yml;
  cv::Mat image;
  cv::Mat graphic;

  graphic = cv::Mat::zeros(SIDE,SIDE,CV_32FC1);

  ss_yml << "senoide-" << SIDE << ".yml";
  image = cv::Mat::zeros(SIDE, SIDE, CV_32FC1);

  cv::FileStorage fs(ss_yml.str(), cv::FileStorage::WRITE);

  for (int i = 0; i < SIDE; i++) {
    for (int j = 0; j < SIDE; j++) {
      image.at<float>(i, j) = 127 * sin(2 * M_PI * PERIODOS *i / SIDE) + 128;
    }
  }

  fs << "mat" << image;
  fs.release();

  cv::Mat image_yml = image.clone();

  cv::normalize(image, image, 0, 255, cv::NORM_MINMAX);
  image.convertTo(image, CV_8U);
  ss_img << "senoide-" << SIDE << ".png";
  cv::imwrite(ss_img.str(), image);



  fs.open(ss_yml.str(), cv::FileStorage::READ);
  fs["mat"] >> image;

  cv::normalize(image, image, 0, 255, cv::NORM_MINMAX);
  image.convertTo(image, CV_8U);

  int line = 5;
  for (int j = 0; j<SIDE ; j++){

        
        float linha = 1000*(abs(image.at<uchar>(line,j) - image_yml.at<float>(line,j)));
        graphic.at<float>(linha, j) = linha;

  }



  cv::imshow("Gráfico da imagem da senoide bidimensional", image);
  
  cv::imshow("Gráfico do erro em função da coluna",graphic);
  cv::waitKey();

  return 0;
}
```



## 4. Resultados

### Imagens periódica
Percebeu-se que ao contrário do exemplo feito pelo professor, a imagem aqui requisitada era periódica nas linhas, pois dependia da i-ésima linha x (i), e o gráfico do erro tem uma diferença que para ser notada, precisou ser multiplicada por 100, que é a diferença entre os números salvos em yml(float) e em png(uchar).


---

## 5. Conclusão

A manipulação e a serialização de dados é muito importante no processamento digital de imagens e nos pode dar uma noção das possíveis manipulações e formatos que podemos definir para uma imagem, e a implementação da imagem como uma função de duas variáveis também é capaz de nos dar uma melhor intuição ao trabalhar com imagens como se fosse funções de duas variáveis.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
