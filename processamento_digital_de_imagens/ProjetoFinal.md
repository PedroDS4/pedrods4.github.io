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

Neste projeto, abordamos a recuperação de uma imagem limpa $$X$$ a partir de uma imagem borrada $$B$$ e de uma máscara de borramento $$G$$ conhecida. Este é um problema típico de deconvolução no processamento de imagens, no qual o objetivo é reconstruir $$F$$ de forma precisa, minimizando artefatos e ruídos.

A abordagem utiliza a minimização de um erro médio quadrático entre $$B$$ e a convolução $$G * F$$, adicionando uma regularização Laplaciana para suavizar a imagem recuperada.  

## Metodologia

### Formulação do Problema

O problema de deconvolução é formulado como a minimização da seguinte função de custo:

$$
E(F(i,j))  =  \sum_{i,j} ( B(i,j) - (G \ast F)(i,j) )^2 = \sum_{i,j}  ( B(i,j) - \sum_{u} \sum_{v} G(u,v) F(i-u,j-v) )^2 + \lambda \nabla^2 F(i,j)
$$

que representa o erro médio quadrático entre os pixels da imagem borrada e da imagem modelada pela convolução com a máscara de borramento.

Onde:
- $$B(i,j)$$: Pixel da imagem borrada.
- $$G \ast F$$: Convolução da máscara $$G$$ com a imagem limpa $$F$$.
- $$\nabla^2 X(i,j)$$: Laplaciano do pixel $$F(i,j)$$.
- $$\lambda$$: Fator de regularização que controla o peso da suavização.

### Gradiente da Função de Custo

Calculando o gradiente da função custo analiticamente, temos que

$$
\frac{\partial }{\partial F(i,j)} E(F(i,j)) = \frac{\partial }{\partial X(i,j)} \sum_{i,j} ( B(i,j) -  ( B(i,j) - \sum_{u} \sum_{v} G(u,v) F(i-u,j-v) ) )^2 
$$

utilizando a regra da cadeia, temos

$$
\frac{\partial }{\partial F(i,j)} E( F(i,j) ) =  2 \cdot    \sum_{i,j}  B(i,j) -  ( B(i,j) - \sum_{u} \sum_{v} G(u,v) F(i-u,j-v) )  \cdot 
\frac{\partial }{\partial F(i,j)} - \sum_{u} \sum_{v} G(u,v) F(i-u,j-v) 
$$


e finalmente temos que O gradiente da função de custo em relação a imagem a ser recuperada $$F(i,j)$$ é dado por:


$$
\frac{\partial E}{\partial F} = G^T \ast (G \ast F - B) - \lambda \nabla^2 F
$$

Onde $$G^T$$ é o kernel transposto de $$G$$.



### Algoritmo de Otimização

1. **Inicialização**:
   - Definir $$X_0$$ como um chute inicial (por exemplo, a imagem borrada $B$ ou uma imagem aleatória).
     
2. **Iteração**:
   - Calcular o gradiente $$\frac{\partial E}{\partial X}$$.
   - Atualizar $$X$$ usando descida de gradiente:
     $$
     X^{(k+1)} = X^{(k)} - \eta \frac{\partial E}{\partial X}
     $$
     Onde $$\eta$$ é a taxa de aprendizado.
     
3. **Convergência**:
   - Parar quando
    $$
    ||X^{(k+1)} - X^{(k)}||
    $$

   for menor que um limiar predefinido ou quando um certo número de iterações $$n_{iteraçôes}$$ for atingoido.


### Regularização Laplaciana

A regularização Laplaciana é implementada aplicando um filtro Laplaciano 3x3 à imagem $X$, definido como:

$$
\text{Kernel Laplaciano} =
\begin{bmatrix}
0 & -1 & 0 \\
-1 & 4 & -1 \\
0 & -1 & 0
\end{bmatrix}
$$

e serve para buscar uma solução mais suave para o problema, em busca das bordas da imagem.

### Implementação
O algoritmo foi implementado em C++ usando a biblioteca OpenCV. A função principal realiza as seguintes etapas:
1. Carregar a imagem borrada $$B$$ e a máscara $G$.
2. Inicializar a imagem $$X$$ com $$B$$.
3. Executar o loop de descida de gradiente até a convergência.

## Código C++

```cpp
#include <opencv2/opencv.hpp>
#include <iostream>

// Função para aplicar a convolução
cv::Mat convolve(const cv::Mat& image, const cv::Mat& kernel) {
    cv::Mat result;
    cv::filter2D(image, result, -1, kernel, cv::Point(-1, -1), 0, cv::BORDER_CONSTANT);
    return result;
}

// Função para calcular o Laplaciano da imagem
cv::Mat computeLaplacian(const cv::Mat& image) {
    cv::Mat laplacian;
    cv::Laplacian(image, laplacian, CV_32F, 1, 1, 0, cv::BORDER_CONSTANT);
    return laplacian;
}

// Função principal para deblurring
cv::Mat deblurImage(const cv::Mat& blurred, const cv::Mat& kernel, float lambda, int iterations, float learning_rate) {
    // Inicialize a imagem limpa como a imagem borrada
    cv::Mat clean = blurred.clone();
    clean.convertTo(clean, CV_32F);
    
    // Garantir que o kernel esteja no formato adequado
    cv::Mat kernelFlipped;
    kernel.convertTo(kernel, CV_32F);
    cv::flip(kernel, kernelFlipped, -1);  // Inverte o kernel para a convolução inversa

    for (int iter = 0; iter < iterations; ++iter) {
        // Convolução de G * F
        cv::Mat blurredEstimate = convolve(clean, kernel);
        cv::normalize(blurredEstimate, blurredEstimate, 0, 255, cv::NORM_MINMAX);

        // Gradiente da função de erro
        cv::Mat grad = convolve(blurredEstimate - blurred, kernelFlipped);

        // Regularização Laplaciana
        cv::Mat laplacian = computeLaplacian(clean);

        // Atualize a imagem limpa
        clean -= learning_rate * (grad + lambda * laplacian);
    }

    // Converta a imagem de volta para o formato original
    clean.convertTo(clean, blurred.type());
    cv::normalize(clean,clean,0,255,cv::NORM_MINMAX);
    return clean;
}

int main() {
    // Carregar a imagem limpa (em tons de cinza)
    cv::Mat clean = cv::imread("lena.png", cv::IMREAD_GRAYSCALE);
    if (clean.empty()) {
        std::cerr << "Erro ao carregar a imagem limpa!" << std::endl;
        return -1;
    }

    clean.convertTo(clean, CV_32F);

    // Criar a máscara (kernel de borramento) - pode ser personalizada
    int kernelSize = 5;
    cv::Mat mask = cv::getGaussianKernel(kernelSize, -1, CV_32F) *
                     cv::getGaussianKernel(kernelSize, -1, CV_32F).t();

    // Aplicar o borramento sintético
    cv::Mat blurred = convolve(clean, mask);
    cv::normalize(blurred, blurred, 0, 255, cv::NORM_MINMAX);

    // Parâmetros do algoritmo
    float lambda = 0.01;          // Fator de regularização
    int iterations = 700;        // Número de iterações
    float learning_rate = 0.1f;  // Taxa de aprendizado

    // Recuperar a imagem limpa a partir da imagem borrada
    cv::Mat recovered = deblurImage(blurred, mask, lambda, iterations, learning_rate);
    

    // Convertendo para uchar para exibir    
    recovered.convertTo(recovered,CV_8U);
    clean.convertTo(clean, CV_8U);
    blurred.convertTo(blurred, CV_8U);  // Convertendo para tipo adequado para exibição
    // Salvar e exibir as imagens
    cv::imwrite("recovered_image.jpg", recovered);
    cv::imshow("Clean Image", clean);
    cv::imshow("Blurred Image", blurred);
    cv::imshow("Recovered Image", recovered);
    cv::waitKey(0);

    return 0;
}


```

## Resultados e discussões
Aplicando a deconvolução a imagem da lena mostrada abaixo, com um borramento de uma máscara gaussiana de fórmula

$$
G(x,y) = e^{-\frac{x^2 + y²}{2 \sigma ²}}
$$

onde $$x = {-2,-1,0,1,2}$$ e $$y = {-2,-1,0,1,2}$$, pela imagem


![Imagem da lena](./imagens/lena.png)

*Figura 1: Imagem a ser borrada pelo filtro gaussiano.*

Foi aplicada a deconvolução com os seguintes parâmetros

$$
\begin{bmatrix}
\lambda = 0.01 \\
n_{iterações} = 700\\
\eta = 0.1
\end{bmatrix}
$$

observou-se que a imagem recuperada ficou muito próxima da imagem limpa, a não ser por um ganho nos tons de cinza, como é possível ver abaixo

![Imagem da lena recuperada](./imagens/imagem_lena_recuparada_vs_original_N_5.png)

*Figura 2: Comparação da Imagem recuperada pela deconvolução com $$N = 5$$.*


Porém ao aumentar a intensidade do borramento, fica cada vez mais difícil conseguir recuperar a imagem
com um borramento de $$N = 11$$, a recuperação ja ficou bem prejudicada
A solução para isso pode ser encontrada usando outra função de custo para os pixels da imagem, e talvez algum fator de restrição/regularização para o problema ficar melhor
posto e ter uma solução mais "suave".

A figura abaixo mostra o resultado para um borramento de $$N = 11$$


![Imagem da lena recuperada de um borramento N = 11](./imagens/imagem_lena_recuperada_vs_original_N_11.png)

*Figura 3: Imagem da lena recuperada de um borramento $$N = 11$$.*


## Referências
GONZALEZ, Rafael C.; WOODS, Richard E. Processamento Digital de Imagens. Pearson Prentice Hall, 2008.
OpenCV Documentation: https://docs.opencv.org/
