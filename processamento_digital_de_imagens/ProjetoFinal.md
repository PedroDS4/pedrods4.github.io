<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

# Universidade Federal do Rio Grande do Norte

# Projeto Final da Disciplina: Deconvolução de Imagens usando Otimização

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 15/01/2025  

## Introdução

Neste projeto, abordamos a recuperação de uma imagem limpa \( X \) a partir de uma imagem borrada \( B \) e de uma máscara de borramento \( G \) conhecida. Este é um problema típico de deconvolução no processamento de imagens, no qual o objetivo é reconstruir \( X \) de forma precisa, minimizando artefatos e ruídos.

A abordagem utiliza a minimização de um erro médio quadrático entre \( B \) e a convolução \( G * X \), adicionando uma regularização Laplaciana para suavizar a imagem recuperada.  

## Metodologia

### Formulação do Problema

O problema de deconvolução é formulado como a minimização da seguinte função de custo:

\[
E(X) = \frac{1}{2} \sum_{i,j} \left[ B(i,j) - (G * X)(i,j) \right]^2 + \lambda \sum_{i,j} || \nabla^2 X(i,j) ||^2
\]

Onde:
- \( B(i,j) \): Pixel da imagem borrada.
- \( G * X \): Convolução da máscara \( G \) com a imagem \( X \).
- \( \nabla^2 X(i,j) \): Laplaciano do pixel \( X(i,j) \).
- \( \lambda \): Fator de regularização que controla o peso da suavização.

### Gradiente da Função de Custo

O gradiente da função de custo em relação a \( X \) é dado por:

\[
\frac{\partial E}{\partial X} = G^T * (G * X - B) - \lambda \nabla^2 X
\]

Onde \( G^T \) é o kernel transposto de \( G \).

### Algoritmo de Otimização

1. **Inicialização**:
   - Definir \( X_0 \) como uma aproximação inicial (por exemplo, a imagem borrada \( B \)).
2. **Iteração**:
   - Calcular o gradiente \( \frac{\partial E}{\partial X} \).
   - Atualizar \( X \) usando descida de gradiente:
     \[
     X^{(k+1)} = X^{(k)} - \eta \frac{\partial E}{\partial X}
     \]
     Onde \( \eta \) é a taxa de aprendizado.
3. **Convergência**:
   - Parar quando \( ||X^{(k+1)} - X^{(k)}|| \) for menor que um limiar predefinido.

### Regularização Laplaciana

A regularização Laplaciana é implementada aplicando um filtro Laplaciano 3x3 à imagem \( X \), definido como:

\[
\text{Kernel Laplaciano} =
\begin{bmatrix}
0 & -1 & 0 \\
-1 & 4 & -1 \\
0 & -1 & 0
\end{bmatrix}
\]

### Implementação

O algoritmo foi implementado em C++ usando a biblioteca OpenCV. A função principal realiza as seguintes etapas:
1. Carregar a imagem borrada \( B \) e a máscara \( G \).
2. Inicializar a imagem \( X \) com \( B \).
3. Executar o loop de descida de gradiente até a convergência.

## Código C++

```cpp
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

// Função para calcular a convolução de uma imagem com um kernel
Mat convolve(const Mat& image, const Mat& kernel) {
    Mat result;
    filter2D(image, result, -1, kernel, Point(-1, -1), 0, BORDER_CONSTANT);
    return result;
}

// Função principal para deconvolução
Mat deconvolve(const Mat& B, const Mat& G, double lambda, double eta, int max_iters) {
    // Inicializar X com a imagem borrada
    Mat X = B.clone();
    
    // Criar o kernel Laplaciano
    Mat laplacian_kernel = (Mat_<float>(3, 3) << 0, -1, 0, -1, 4, -1, 0, -1, 0);
    
    for (int iter = 0; iter < max_iters; ++iter) {
        // Calcular G * X
        Mat GX = convolve(X, G);
        
        // Calcular o erro
        Mat error = GX - B;
        
        // Gradiente da função de custo
        Mat grad = convolve(error, G.t()) - lambda * convolve(X, laplacian_kernel);
        
        // Atualizar X
        X -= eta * grad;
    }
    return X;
}

int main() {
    // Carregar a imagem borrada (B) e a máscara de borramento (G)
    Mat B = imread("blurred_image.jpg", IMREAD_GRAYSCALE);
    Mat G = getGaussianKernel(9, 1, CV_32F); // Exemplo de kernel gaussiano
    
    if (B.empty() || G.empty()) {
        cerr << "Erro ao carregar imagem ou kernel!" << endl;
        return -1;
    }
    
    // Configurações da deconvolução
    double lambda = 0.1; // Peso da regularização
    double eta = 0.01;   // Taxa de aprendizado
    int max_iters = 100; // Número máximo de iterações
    
    // Recuperar a imagem original
    Mat X = deconvolve(B, G, lambda, eta, max_iters);
    
    // Salvar e exibir o resultado
    imwrite("recovered_image.jpg", X);
    imshow("Imagem Recuperada", X);
    waitKey(0);
    
    return 0;
}
```


## Referências
GONZALEZ, Rafael C.; WOODS, Richard E. Processamento Digital de Imagens. Pearson Prentice Hall, 2008.
OpenCV Documentation: https://docs.opencv.org/
