<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

#**Relatório Atividade 9: Filtragem Homomórfica no domínio da frequência**

# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 29/11

## 1. Introdução

Na aquisição de imagens é muito difícil ter uma aquisição ideal, principalmente por limitação de dispositivos e também pela má iluminação de uma cena.
Para isso, existe um filtro capaz de corrigir a iluminação de uma cena utilizando ferramentas matemáticas e algumas aproximações.

---

## 2. Objetivo

O Objetivo dessa atividade é utilizar ferramentas matemáticas como a transformada de fourier e operações como logaritmo, para realizar a filtragem de uma imagem afim de corrigir
sua iluminação.

---


## 3. Metodologia

### Componentes de uma imagem
O modelo matemático mais simples para fazer a descrição de imagens é o que utiliza os conceitos de iluminação e reflectância, 
podemos dizer que uma imagem é dada pelo produto

$$
f(x,y) = i(x,y) \cdot r(x,y)
$$

onde i e r são a iluminação e a reflectância, respectivamente.
e podemos inferir que a componente de iluminação, que representa a luz da cena, é uma componente que varia suavemente(frequências baixas), enquanto a componente da reflectância
tem frequências mais altas.
Com isso é foi desenvolvida uma estratégia para separar os sinais de alta e baixa frequência em uma soma, e então podemos aplicar um filtro passa-baixas para filtrar a imagem.

### Cepstrum complexo
O cepstrum complexo é um domínio aonde levamos nossa imagem, a viagem até esse domínio tem como ponto final de passagem o domínio da frequência ja conhecido que somos levados pela transformada de fourier, depois basta pegarmos o logaritmo desse resultado e teremos nossos sinais separados, teoricamente.
Esse processo pode ser descrito matematicamente como

$$
Ln( f(x,y) = i(x,y) \cdot r(x,y) ) => z(x,y) = Ln(i(x,y)) + Ln(r(x,y))
$$

aplicando agora a transformada de fourier

$$
Z(u,v) = FFT ( Ln(i(x,y)) + Ln(r(x,y)) ) = FFT(Ln(i(x,y))) + FFT(Ln(r(x,y)))
$$

### Filtro homomórfico
Por fim aplicamos o filtro à imagem $$Z(u,v)$$

$$
S(u,v) = H(u,v) \cdot Z(u,v)
$$

onde H(u,v) é o filtro homomórfico, dado matematicamente por

$$
H(u,v) = \gamma_L + (\gamma_H - \gamma_L)(1 - e^{-c\frac{D(u,v)^2}{D_0^2} })
$$

e então para obter de volta a imagem, basta fazermos

$$
s(x,y) = IFFT(S(u,v))
$$

e finalmente

$$
f_{filtrada}(x,y) = exp(s(x,y))
$$

foi então adquirida uma cena com má iluminação, que é a imagem mostrada abaixo em tons

![Imagem má iluminada](./imagens/imagem_ma_iluminada.png)

*Figura 1: Imagem que será aplicada o filtro homomórfico para corrigir iluminação.*


### 3.1. Implementação do filtro homomórfico
Foi então utilizado o código do professor como referência para as operações computacionais com a DFT e aplicação da função de filtro, e foi feito uma função para aplicar o filtro homomórfico. 

* Código 

```
#include <iostream>
#include <vector>
#include <opencv2/opencv.hpp>

void swapQuadrants(cv::Mat& image) {
  cv::Mat tmp, A, B, C, D;

  // se a imagem tiver tamanho impar, recorta a regiao para o maior
  // tamanho par possivel (-2 = 1111...1110)
  image = image(cv::Rect(0, 0, image.cols & -2, image.rows & -2));

  int centerX = image.cols / 2;
  int centerY = image.rows / 2;

  // rearranja os quadrantes da transformada de Fourier de forma que 
  // a origem fique no centro da imagem
  // A B   ->  D C
  // C D       B A
  A = image(cv::Rect(0, 0, centerX, centerY));
  B = image(cv::Rect(centerX, 0, centerX, centerY));
  C = image(cv::Rect(0, centerY, centerX, centerY));
  D = image(cv::Rect(centerX, centerY, centerX, centerY));

  // swap quadrants (Top-Left with Bottom-Right)
  A.copyTo(tmp);
  D.copyTo(A);
  tmp.copyTo(D);

  // swap quadrant (Top-Right with Bottom-Left)
  C.copyTo(tmp);
  B.copyTo(C);
  tmp.copyTo(B);
}

void makeFilter(const cv::Mat &image, cv::Mat &filter){
  cv::Mat_<float> filter2D(image.rows, image.cols);
  int centerX = image.cols / 2;
  int centerY = image.rows / 2;
  int radius = 20;

  for (int i = 0; i < image.rows; i++) {
    for (int j = 0; j < image.cols; j++) {
      if (pow(i - centerY, 2) + pow(j - centerX, 2) <= pow(radius, 2)) {
        filter2D.at<float>(i, j) = 1;
      } else {
        filter2D.at<float>(i, j) = 0;
      }
    }
  }

  cv::Mat planes[] = {cv::Mat_<float>(filter2D), cv::Mat::zeros(filter2D.size(), CV_32F)};
  cv::merge(planes, 2, filter);
}

int main(int argc, char** argv) {
  cv::Mat image, padded, complexImage;
  std::vector<cv::Mat> planos; 

  image = imread(argv[1], cv::IMREAD_GRAYSCALE);
  if (image.empty()) {
    std::cout << "Erro abrindo imagem" << argv[1] << std::endl;
    return EXIT_FAILURE;
  }

  // expande a imagem de entrada para o melhor tamanho no qual a DFT pode ser
  // executada, preenchendo com zeros a lateral inferior direita.
  int dft_M = cv::getOptimalDFTSize(image.rows);
  int dft_N = cv::getOptimalDFTSize(image.cols); 
  cv::copyMakeBorder(image, padded, 0, dft_M - image.rows, 0, dft_N - image.cols, cv::BORDER_CONSTANT, cv::Scalar::all(0));

  // prepara a matriz complexa para ser preenchida
  // primeiro a parte real, contendo a imagem de entrada
  planos.push_back(cv::Mat_<float>(padded)); 
  // depois a parte imaginaria com valores nulos
  planos.push_back(cv::Mat::zeros(padded.size(), CV_32F));

  // combina os planos em uma unica estrutura de dados complexa
  cv::merge(planos, complexImage);  

  // calcula a DFT
  cv::dft(complexImage, complexImage); 
  swapQuadrants(complexImage);

  // cria o filtro ideal e aplica a filtragem de frequencia
  cv::Mat filter;
  makeFilter(complexImage, filter);
  cv::mulSpectrums(complexImage, filter, complexImage, 0);

  // calcula a DFT inversa
  swapQuadrants(complexImage);
  cv::idft(complexImage, complexImage);

  // planos[0] : Re(DFT(image)
  // planos[1] : Im(DFT(image)
  cv::split(complexImage, planos);

  // recorta a imagem filtrada para o tamanho original
  // selecionando a regiao de interesse (roi)
  cv::Rect roi(0, 0, image.cols, image.rows);
  cv::Mat result = planos[0](roi);

  // normaliza a parte real para exibicao
  cv::normalize(result, result, 0, 1, cv::NORM_MINMAX);

  cv::imshow("image", result);
  cv::imwrite("dft-filter.png", result * 255);

  cv::waitKey();
  return EXIT_SUCCESS;
}

```


## 4. Resultados

### Transformada de fourier Analítica vs DFT
Para comparativo da transformada de fourier calculada pela expressão da DFT utilizando o opencv, será calculada abaixo a transformada de fourier real de uma imagem da senoide dada por



![Imagem corrigida pelo filtro](./imagens/imagem_filtrada.png)

*Figura 2: Resultado da Filtragem homomórfica para corrigir iluminação.*

---

## 5. Conclusão

A Filtragem homomórfica pode ser muito útil, por mais que ela não seja igual as outras filtragens no domínio da frequência, que focam mais em ruídos periódicos, sua aplicação pode ser também muito usada em muitas áreas do processamento de imagens principalmente para corrigir a iluminação de cenas.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
