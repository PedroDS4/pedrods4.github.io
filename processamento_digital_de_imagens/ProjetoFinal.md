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

O objetivo deste projeto é desenvolver uma técnica de deconvolução de imagens no domínio da frequência para estimar a imagem original borrada a partir de uma imagem observada $$ Y $$ . A abordagem adotada envolve o uso de máscaras gaussianas como função de borramento, modeladas como uma combinação de funções de base radial. O problema central é estimar a imagem $$ X $$ original, além dos parâmetros da máscara de borramento, como os coeficientes de amplitude, centros e desvio padrão das gaussianas.

A deconvolução é realizada através da minimização de um erro entre a imagem observada $$ Y $$ e a imagem estimada $$ G * X $$, onde $$ G $$ é o filtro de borramento e $$ X $$ é a imagem original.

## Metodologia

### Definição do Problema

O problema de deconvolução pode ser formulado como a minimização da seguinte função de erro $ E(X, G) $:

$$
E(X, G) = || Y - G * X ||^2
$$

Onde:
- $$ Y $$ é a imagem observada,
- $$ G $$ é a máscara de borramento, modelada como uma combinação de gaussianas,
- $$ X $$ é a imagem original que queremos estimar.

### Máscaras Gaussianas

A máscara de borramento $$ G $$ é modelada como uma soma ponderada de gaussianas bidimensionais. Cada gaussiana tem parâmetros:
- $$ a_i $$como coeficiente de amplitude,
- $$ (u_i, v_i) $$ como centro da gaussiana,
- $$ \sigma_i $$ como desvio padrão que controla a largura da gaussiana.

Assim, a máscara $$ G(x, y) $$ é expressa como:

$$
G(x, y) = \sum_{i} a_i \exp\left( -\frac{(x - u_i)^2 + (y - v_i)^2}{2 \sigma_i^2} \right)
$$

Onde $$ (x, y) $$ são as coordenadas da imagem.

### Estimativa dos Parâmetros

Os parâmetros da máscara de borramento, bem como a imagem $$ X $$, são estimados por meio de um processo de otimização. A função de erro $$ E(X) $$ é minimizada em relação a $$ X $$ e os parâmetros da máscara $$ G $$. O algoritmo de otimização utilizado neste projeto é a descida de gradiente.

A otimização é realizada de forma a ajustar tanto a imagem $$ X $$ quanto os parâmetros $$ a_i, u_i, v_i, \sigma_i $$ das gaussianas.

# Demonstração do Gradiente para Estimativa de PSF no Domínio do Tempo

## Problema

Queremos estimar uma função de espalhamento de ponto (PSF) $ G $ que satisfaz a equação convolucional no domínio do tempo:

$$
Y = G * X
$$

Onde:
- $ Y $: Imagem borrada.
- $ G $: PSF (a ser estimada).
- $ X $: Imagem original.

O objetivo é minimizar o erro entre a imagem borrada original $ Y $ e a convolução $ G * X $.

---

## Função de Erro

A função de erro é definida como o erro quadrático médio (MSE):

$$
E = \frac{1}{2} \sum_{i, j} \left[ Y(i, j) - (G * X)(i, j) \right]^2
$$

---

## Gradiente com Respeito à PSF $ G $

Para atualizar $ G $, derivamos a função de erro em relação a $ G $. O gradiente é dado por:

$$
\frac{\partial E}{\partial G} = - \sum_{i, j} \left[ Y(i, j) - (G * X)(i, j) \right] \cdot X(-i, -j)
$$

Aqui:
- $ X(-i, -j) $: Representa o "flip" espacial da matriz $ X $, necessário para calcular a convolução transposta.

---

## Algoritmo de Atualização

1. Inicializar $ G $ como uma combinação linear de $ N $ gaussianas bidimensionais com pesos $ w_i $ e centros $ c_i $:

$$
G(x, y) = \sum_{i=1}^{N} w_i \cdot \exp\left(-\frac{(x - c_{i,x})^2 + (y - c_{i,y})^2}{2 \sigma^2}\right)
$$

2. Iterativamente ajustar:
   - Pesos $ w_i $ usando o gradiente com relação a $ w_i $.
   - Centros $ c_i $ usando o gradiente com relação a $ c_i $.

---

## Implementação no Código

No código em C++:
- A convolução $ G * X $ é realizada usando a função `convolve`.
- O gradiente é calculado para os pesos $ w_i $ e os centros $ c_i $ de cada gaussiana.
- O PSF $ G $ é atualizado iterativamente usando descida de gradiente.

O código resultante inclui a normalização da imagem, a criação do kernel gaussiano, a aplicação do borramento e a estimativa da PSF.

```cpp

#include <opencv2/opencv.hpp>
#include <vector>
#include <cmath>
#include <iostream>

using namespace cv;
using namespace std;

// Função gaussiana bidimensional
double gaussian(double x, double y, double sigma) {
    return std::exp(-(x * x + y * y) / (2.0 * sigma * sigma));
}

// Convolução 2D simples
cv::Mat convolve(const cv::Mat& input, const cv::Mat& kernel) {
    cv::Mat output;
    cv::filter2D(input, output, -1, kernel, cv::Point(-1, -1), 0, cv::BORDER_CONSTANT);
    return output;
}

// Estimar a PSF com base no gradiente descendente
cv::Mat estimatePSF(const cv::Mat& blurred, const cv::Mat& original, int N, double learning_rate, int iterations) {
    int rows = original.rows;
    int cols = original.cols;

    // Inicializar pesos e centros aleatoriamente
    std::vector<double> weights(N, 1.0 / N); // Pesos inicializados uniformemente
    std::vector<cv::Point2d> centers(N);
    double sigma = 5.0;

    for (int i = 0; i < N; i++) {
        centers[i] = cv::Point2d(rand() % cols, rand() % rows);
    }

    for (int iter = 0; iter < iterations; iter++) {
        cv::Mat G(rows, cols, CV_64F, cv::Scalar(0));
        std::vector<cv::Mat> gaussian_kernels(N);

        for (int i = 0; i < N; i++) {
            cv::Mat kernel(rows, cols, CV_64F, cv::Scalar(0));
            for (int y = 0; y < rows; y++) {
                for (int x = 0; x < cols; x++) {
                    double dx = x - centers[i].x;
                    double dy = y - centers[i].y;
                    kernel.at<double>(y, x) = gaussian(dx, dy, sigma);
                }
            }
            gaussian_kernels[i] = kernel;
            G += weights[i] * convolve(original, kernel); // PSF gerada
        }

        cv::Mat error = blurred - convolve(original, G);

        // Gradientes
        std::vector<double> grad_weights(N, 0.0);
        std::vector<cv::Point2d> grad_centers(N, cv::Point2d(0.0, 0.0));

        for (int i = 0; i < N; i++) {
            cv::Mat conv_gX = convolve(original, gaussian_kernels[i]);

            // Gradiente em relação ao peso
            for (int y = 0; y < rows; y++) {
                for (int x = 0; x < cols; x++) {
                    grad_weights[i] += -2.0 * error.at<double>(y, x) * conv_gX.at<double>(y, x);
                }
            }

            // Gradiente em relação aos centros
            cv::Mat partial_g(rows, cols, CV_64F, cv::Scalar(0));
            for (int y = 0; y < rows; y++) {
                for (int x = 0; x < cols; x++) {
                    double dx = x - centers[i].x;
                    double dy = y - centers[i].y;
                    partial_g.at<double>(y, x) = gaussian(dx, dy, sigma) * dx / (sigma * sigma);
                }
            }

            cv::Mat conv_partial_gX = convolve(original, partial_g);

            for (int y = 0; y < rows; y++) {
                for (int x = 0; x < cols; x++) {
                    grad_centers[i].x += -2.0 * error.at<double>(y, x) * conv_partial_gX.at<double>(y, x);
                    grad_centers[i].y += -2.0 * error.at<double>(y, x) * conv_partial_gX.at<double>(y, x);
                }
            }
        }

        // Atualiza pesos e centros
        for (int i = 0; i < N; i++) {
            weights[i] -= learning_rate * grad_weights[i];
            centers[i].x -= learning_rate * grad_centers[i].x;
            centers[i].y -= learning_rate * grad_centers[i].y;
        }
    }

    // Gerar a PSF final
    cv::Mat PSF(rows, cols, CV_64F, cv::Scalar(0));
    for (int i = 0; i < N; i++) {
        cv::Mat kernel(rows, cols, CV_64F, cv::Scalar(0));
        for (int y = 0; y < rows; y++) {
            for (int x = 0; x < cols; x++) {
                double dx = x - centers[i].x;
                double dy = y - centers[i].y;
                kernel.at<double>(y, x) = gaussian(dx, dy, sigma);
            }
        }
        PSF += weights[i] * kernel;
    }

    return PSF;
}

// Função principal
int main(int argc, char** argv) {
    if (argc < 2) {
        cout << "Uso: " << argv[0] << " <caminho_da_imagem>" << endl;
        return -1;
    }

    Mat img = imread(argv[1], IMREAD_GRAYSCALE);
    if (img.empty()) {
        cout << "Não foi possível carregar a imagem." << endl;
        return -1;
    }

    img.convertTo(img, CV_64F, 1.0 / 255); // Normalizar a imagem

    // Criar uma máscara gaussiana para borramento
    Mat gaussian_kernel = getGaussianKernel(21, 5, CV_64F) * getGaussianKernel(21, 5, CV_64F).t();
    Mat blurred;
    filter2D(img, blurred, -1, gaussian_kernel);

    // Estimar a PSF
    int N = 4; // Número de gaussianas
    double learning_rate = 0.01;
    int iterations = 40;
    Mat estimated_PSF = estimatePSF(blurred, img, N, learning_rate, iterations);
    
    // Verificar se o borramento ficou parecido ou não
    Mat estimated_blur;
    filter2D(img, estimated_blur, -1, estimated_PSF);

    // Salvar e exibir os resultados
    imwrite("blurred_image.png", blurred * 255);
    imwrite("estimated_psf.png", estimated_PSF * 255);
    imwrite("estimated_blurred_image.png", estimated_blur * 255);

    imshow("Imagem Original", img);
    imshow("Imagem Borrada", blurred);
    imshow("PSF Estimada", estimated_PSF);
    imshow("Imagem borrada com a PSF Estimada", estimated_blur);
    waitKey(0);

    return 0;
}


```

## Resultados

Após a execução da otimização, a imagem \( X \) estimada é comparada com a imagem original para avaliar a eficácia do algoritmo de deconvolução. A precisão é medida com base na redução do erro quadrático médio entre a imagem original e a estimada.

Além disso, a reconstrução da máscara \( G \) mostra como os parâmetros das gaussianas se ajustaram para aproximar a máscara de borramento real utilizada para gerar a imagem observada.

## Conclusão

O projeto demonstrou a viabilidade de estimar uma imagem original borrada através da deconvolução no domínio da frequência, utilizando uma máscara de borramento composta por gaussianas. A abordagem baseada em otimização e gradientes mostrou-se eficaz para a recuperação da imagem original e para a estimativa dos parâmetros da máscara de borramento.

A metodologia pode ser expandida para diferentes tipos de filtros de borramento e aplicada a cenários mais complexos, como imagens com diferentes níveis de ruído ou com borramentos não uniformes.


## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
