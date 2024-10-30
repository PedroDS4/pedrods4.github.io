<script type="text/javascript" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

[Voltar para a página principal](../index.md)

# **Relatório Atividade 3: Esteganografia**
# Universidade Federal do Rio Grande do Norte

**Engenharia Mecatrônica**  
**Disciplina: Processamento Digital de Imagens**

**Aluno(a):** Pedro Arthur Oliveira dos Santos  
**Professor(a):** Agostinho Brito Junior  
**Data:** 25/10

## 1. Introdução

No campo do processamento digital de imagens, a esteganografia é uma técnica que ja foi muito utilizada para criptografar imagens e escondê-las em outras imagens, utilizando manipulações de bits.

---

## 2. Objetivo

O objetivo deste experimento é descobrir a imagem que foi escondida em uma imagem fornecida pelo professor, onde foi aplicada uma técnica de esteganografia para esconder nela uma imagem.

---

## 3. Metodologia

### Manipulação de bits e esteganografia
Em c++ e c podemos usar os operadores << e >> para deslocar bits para a esquerda ou para a direita, respectivamente, e para esconder uma imagem em outra, é preciso substituir os bits menos significativos
da imagem portadora pelos mais significativos da imagem que será escondida.

### Recuperando a imagem escondida
Para recuperarmos a imagem escondida, criamos uma imagem vazia g(x,y) com as mesmas dimensões da imagem portadora, para então manipularmos separadamente cada pixel e decompor os bits para os menos significativos da imagem para serem o mais significativos dessa, para isso usamos o operador <<.
Exemplo: Sabendo que os bits menos significativos são os três últimos, nesse caso que estamos trabalhando com a intensidade do pixel como sendo o tipo unsigned char, podemos ver abaixo como essa operação funciona

$$
I = [ 1 0 0 0 1 1 1 0]
I<<5

I = [1 1 0 0 0 0 0 0]
os três últimos bits foram movidos para a esquerda.


### 3.1. Implementação
Foi usada apenas o operador << para mover os bits mais significativos para a direita, e depois fixar esses bits como pixels na nova imagem vazia.
* Código usado para recuperar a imagem
```
#include <iostream>
#include <opencv2/opencv.hpp>

int main(int argc, char**argv) {
  cv::Mat imagemPortadora, imagemEscondida;
  cv::Vec3b valPortadora, valEscondida;
  int nbits = 3;

  imagemPortadora = cv::imread("desafio_esteganografia.png", cv::IMREAD_COLOR);
  if (imagemPortadora.empty()) {
    std::cerr << "Erro ao carregar a imagem!" << std::endl;
    return -1;
  }

  // Inicialize imagemEscondida com o mesmo tamanho e tipo de imagemPortadora
  imagemEscondida = cv::Mat::zeros(imagemPortadora.size(), imagemPortadora.type());

  for (int i = 0; i < imagemPortadora.rows; i++) {
    for (int j = 0; j < imagemPortadora.cols; j++) {

      valPortadora = imagemPortadora.at<cv::Vec3b>(i, j);
      valEscondida = imagemEscondida.at<cv::Vec3b>(i, j);

      valEscondida[0] = valPortadora[0] << (8-nbits);
      valEscondida[1] = valPortadora[1] << (8-nbits);
      valEscondida[2] = valPortadora[2] << (8-nbits);

      imagemEscondida.at<cv::Vec3b>(i, j) = valEscondida;
    }
  }

  cv::imshow("Imagem escondida na esteganografia", imagemEscondida);
  cv::waitKey(0);

  //cv::imwrite("imagem escondida.png", imagemEscondida);
  
  return 0;
}
```

O trecho de código mais importante aqui é

```
      valEscondida[0] = valPortadora[0] << (8-nbits);
      valEscondida[1] = valPortadora[1] << (8-nbits);
      valEscondida[2] = valPortadora[2] << (8-nbits);

      imagemEscondida.at<cv::Vec3b>(i, j) = valEscondida;
```

onde é feita essa manipulação para cada componente de cor RGB, deslocando 8-nbits = 8-3 = 5 bits para a esuqerda, para então atribuir isso a um outro vetor que será fornecido a imagem RGB.

## 4. Resultados

### A recuperação da imagem foi um sucesso, foi possível recuperar completamente a imagem e ainda com uma ótima qualidade, a imagem recuperada parece ser algum tipo de arte, e é mostrada abaixo



---

## 5. Conclusão

A esteganografia é uma técnica muito útil para codificar imagem e a operação de recuperar uma imagem é feita de maneira simples e objetiva usando manipulação de bits, porém hoje em dia ja existem técnicas melhores de criptografia que conseguem fazer isso de forma mais otimizada, então hoje em dia essa técnica quase não é usada.
---

## 6. Referências

GONZALEZ, Rafael C.; WOODS, Richard E. **Processamento Digital de Imagens**. 3. ed. São Paulo: Pearson Prentice Hall, 2010.
