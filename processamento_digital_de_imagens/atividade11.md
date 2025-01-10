<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

#**Relatório Atividade 11: Quantização Vetorial e Separação**

# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 10/01/2025

## 1. Introdução

Hoje em dia problemas de classificação são cada vez mais comuns de se ver, e desde o início do surgimento do aprendizado de maquina, muitos algoritmos
se popularizaram.
No processamento digital de imagens, também há a necessidade da separação de elementos em uma imagem, que seguem uma classificação, por exemplo objetos
de uma certa cor, ou de um certo formato, ainda.

---

## 2. Objetivo

O Objetivo dessa atividade é utilizar o algorítmo de quantização vetorial k-means para separar as cores de uma imagem, e assim verificar se o processo de classificação
utilizando esse algorítmo consegue separar bem as classes.

---


## 3. Metodologia

###  3.1. Algorítmo de quantização vetorial 

* Escolha k
 como o número de classes para os vetores xi
 de N
 amostras, i=1,2,⋯,N

* Escolha m1,m2,⋯,mk
 como aproximações iniciais para os centros das classes.

* Classifique cada amostra xi
 usando, por exemplo, um classificador de distância mínima (distância euclideana).

* Recalcule as médias mj
 usando o resultado do passo anterior.

Se as novas médias são consistentes (não mudam consideravelmente), finalize o algoritmo. Caso contrário, recalcule os centros e refaça a classificação.



* Código

```
#include <cstdlib>
#include <opencv2/opencv.hpp>

int main(int argc, char** argv) {
  int nClusters = 8, nRodadas = 1;  // nRodadas = 1 para uma única execução do k-means por rodada

  if (argc != 2) {
    std::cout << "Uso: kmeans_aleatorio entrada.jpg\n";
    exit(0);
  }

  cv::Mat img = cv::imread(argv[1], cv::IMREAD_COLOR);
  if (img.empty()) {
    std::cerr << "Erro ao carregar a imagem.\n";
    return -1;
  }

  cv::Mat samples(img.rows * img.cols, 3, CV_32F);
  for (int y = 0; y < img.rows; y++) {
    for (int x = 0; x < img.cols; x..++) {
      for (int z = 0; z < 3; z++) {
        samples.at<float>(y + x * img.rows, z) = img.at<cv::Vec3b>(y, x)[z];
      }
    }
  }

  for (int rodada = 0; rodada < 10; rodada++) {  // Executa 10 rodadas
    cv::Mat rotulos, centros;
    cv::kmeans(samples, nClusters, rotulos,
               cv::TermCriteria(cv::TermCriteria::EPS | cv::TermCriteria::COUNT, 10000, 0.0001),
               nRodadas, cv::KMEANS_RANDOM_CENTERS, centros);

    cv::Mat rotulada(img.size(), img.type());
    for (int y = 0; y < img.rows; y++) {
      for (int x = 0; x < img.cols; x++) {
        int indice = rotulos.at<int>(y + x * img.rows, 0);
        rotulada.at<cv::Vec3b>(y, x)[0] = (uchar)centros.at<float>(indice, 0);
        rotulada.at<cv::Vec3b>(y, x)[1] = (uchar)centros.at<float>(indice, 1);
        rotulada.at<cv::Vec3b>(y, x)[2] = (uchar)centros.at<float>(indice, 2);
      }
    }

    // Salva a imagem da rodada atual
    std::string nomeArquivo = "saida_" + std::to_string(rodada + 1) + ".jpg";
    cv::imwrite(nomeArquivo, rotulada);
  }

  std::cout << "Imagens segmentadas salvas como saida_1.jpg, saida_2.jpg, ..., saida_10.jpg.\n";
  return 0;
}

```



## 4. Resultados

### Gif resultante com as 10 iterações
Podemos ver que ao somar a borda a imagem continuamente, criou-se um efeito visual de borda forte e um expressionismo na imagem, causando
uma certa sensação estranha.

---

## 5. Conclusão

Vemos que o algorítmo k-means não é determinístico, uma vez que cada execução acaba gerando resultados ligeiramente diferentes, revelando a natureza estocástica do algorítmo, mas ainda é possível ver que ficam próximos uns dos outros.

---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
