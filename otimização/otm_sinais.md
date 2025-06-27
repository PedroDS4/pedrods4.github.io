##**Aproximação de um sinal por outro em um intervalo**
Em alguns problemas, é muito comum querer aproximar um sinal complexo de um sistema por um sinal que seja mais simples e fácil para trabalhar.
Usando técnicas de otimização, existem aproximações sofisticadas, e outras mais simples, como a que será tratada nesse notebook.

Seja um sinal g(t) um sinal qualquer, queremos aproximar g(t) por um sinal de forma conhecida para aproximá-lo por esse sinal num intervalo desejado, então

$$
g(t) ≈ c p(t)
$$

e

$$
t1 \leq t \leq t2
$$

onde $p(t)$ é um sinal conhecido bem definido, por exemplo uma senoide ou uma exponencial, e o intervalo conhecido é definido por t1 e t2
Agora vamos definir o erro entre os sinais

$$
e(t) = |g(t) - cp(t)|
$$

e a energia do sinal de erro é dada então por

$$
E_{e(t)} = \int_{t1}^{t2} e(t)^2 dt
$$

então queremos minimizar a energia do erro

$$

\begin{align}
\text{min } & \, E_{e(t)} \\
\text{subject to } & \, t_1 \leq t \leq t_2
\end{align}
$$

assim vemos que o problema é de programação não linear, então podemos derivar a energia do erro em função da constante c, para achar a constante c que melhor ajusta a aproximação, temos assim

$$
\frac{\partial}{\partial c}E_{e(t)} = \frac{\partial}{\partial c} \int_{t1}^{t2} e(t)^2 dt = \int_{t1}^{t2} \frac{\partial}{\partial c} (g(t) - cp(t))^2 dt = 0
$$

utilizando a regra da cadeia, temos

$$
 \frac{\partial}{\partial c} (g(t) - cp(t))^2 = -2p(t)(g(t) - cp(t))
$$

substituindo na integral e igualando a zero

$$
\int_{t1}^{t2} -2p(t)(g(t) - cp(t)) dt = -2 \int_{t1}^{t2} p(t)g(t)dt + 2\int_{t1}^{t2} cp(t)^2 dt
$$

e finalmente temos

$$
c = \frac{\int_{t1}^{t2} p(t)g(t)dt}{\int_{t1}^{t2} p(t)^2 dt}
$$

para o caso discreto que iremos trabalhar, temos similarmente

$$
c = \frac{\sum_{n1}^{n2} p[n] \cdot g[n]}{\sum_{n1}^{n2} p[n]^2}
$$


##**Sinais ortogonais**
Se os sinais g(t) e p(t) forem muito "diferentes", é possível observar que ocorrerá que o valor c se tornará próximo de zero, dizemos que se isso acontece, a integral $\int_{t1}^{t2} p(t)g(t)dt$ é nula e os vetores são ortogonais.


##**Aproximando uma parábola por uma função cosseno**
Seja a função $g(t) = 4-t^2$ centrada na origem, queremos aproximar essa parábola por uma senoide $p(t) = cos(t)$ no intervalo temporal $I = [-2,2]$.
Ou seja, almejamos que:

$$
g(t) = c p(t) = c cos(t)
$$

Assim calculamos a constante c através da expressaõ desenvolvida anteriormente

$$
c = \frac{\int_{-2}^{2} cos(t)(4 - t^2)dt}{\int_{-2}^{2} cos^2(t) dt}
$$

calculando primeiro o denominador, temos, utilizando a identidade trigonométrica $cos^2(t) = \frac{1 + cos(2t)}{2}$

$$
\int_{-2}^{2} cos^2(t) dt = \frac{1}{2} \int_{-2}^{2} 1+cos(2t) dt = \frac{2 - (-2)}{2} + \frac{ sen(2t) }{2}|_{-2}^{2} = 2 + sen(4) 
$$

e para o numerador precisamos utilizar integral por partes, ja que

$$
\int_{-2}^{2} cos(t)(4 - t^2)dt = \int_{-2}^{2} 4cos(t)dt - \int_{-2}^{2} t^2 cos(t)dt = 4 sen(t)|_{-2}^{2} - \int_{-2}^{2} t^2 cos(t)dt
$$

o termo

$$
\int_{-2}^{2} t^2 cos(t)dt
$$

pode ser calculado por integral por partes, de modo que


$$
\begin{cases}
u = t^2\\
dv = cos(t)dt
\end{cases}
$$

então

$$
\begin{cases}
du = 2tdt\\
v = sen(t)
\end{cases}
$$

agora aplicando na fórmula da integral por partes, temos

$$
\int_{}^{} u dv = u \cdot v - \int_{}^{} v du
$$

assim

$$
\int_{-2}^{2} t^2 cos(t)dt = t^2 sen(t)|_{-2}^{2} - \int_{-2}^{2} 2t sen(t) dt
$$

e a integral 

$$
\int_{-2}^{2} t sen(t) dt
$$

pode ser calculada mais uma vez por integral por partes


$$
\begin{cases}
u = 2t\\
dv = sen(t)dt
\end{cases}
$$

então

$$
\begin{cases}
du = 2dt\\
v = -cos(t)
\end{cases}
$$

então temos

$$
\int_{-2}^{2} 2t sen(t) dt = -2t cos(t)|_{-2}^{2} -  \int_{-2}^{2}-cos(t)  2dt = -2\cdot 2 cos(2) - (-2 \cdot -2 \cdot cos(-2) + 2( sen(2) - sen(-2) ) = -8cos(2) + 4sen(2)
$$

então

$$
\int_{-2}^{2} t^2 cos(t)dt = 2^2 sen(2) - ((-2)^2 sen(-2))  - 4sen(2) = 8sen(2) + 8cos(2) - 4sen(2) = 8cos(2) + 4sen(2)
$$

então

$$
4 sen(t)|_{-2}^{2} - \int_{-2}^{2} t^2 cos(t)dt = 8 sen(2) - 4sen(2) - 8cos(2) = 4sen(2) - 8cos(2) 
$$
