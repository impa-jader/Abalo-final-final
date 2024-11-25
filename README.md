# Lista 8 - Programação - Jader Duarte
#Questão 1
from random import choice
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

class aproximador_de_nuvem_pol:
    def __init__(self, grau):
        self.grau= grau
        self.coefs= None

    def ajustar(self,pontos):
        x=np.array([i[0] for i in pontos])
        y=np.array([i[1] for i in pontos])
        self.coefs = np.polyfit(x, y, self.grau)

    def avaliar(self, x):
        if self.coefs is None:
            raise ValueError("O polinômio ainda não foi ajustado.")
        return np.polyval(self.coefs, x)

    def plotar(self, pontos):
        x=np.array([i[0] for i in pontos])
        y=np.array([i[1] for i in pontos])
        if self.coefs is None:
            raise ValueError("O polinômio ainda não foi ajustado.")

        x_plot = np.linspace(min(x), max(x), 500)
        y_plot = self.avaliar(x_plot)

        plt.scatter(x, y, color='red', label='Pontos originais')
        plt.plot(x_plot, y_plot, color='blue', label=f'Polinômio de grau {self.grau}')
        plt.legend()
        plt.xlabel('x')
        plt.ylabel('y')
        plt.title('Ajuste Polinomial')
        plt.show()

# Questão 2
def melhor_grau(pontos):
    x=np.array([i[0] for i in pontos])
    y=np.array([i[1] for i in pontos])
    def erro(grau):
        y_pred = np.polyval(np.polyfit(x, y, grau), x)
        return np.mean((y - y_pred) ** 2) # Calcula o erro absoluto médio
    grau=1
    ainda_n_achei= True
    while ainda_n_achei:
        feliz_dia=False
        for i in [grau + j for j in range(10)]:
            if erro(i)< erro(grau):
                grau=i
                feliz_dia=True
                break
        if not feliz_dia:
            ainda_n_achei=False #Se um grau, tendo menor erro que qualquer um de seus antecessores, tem menor erro doque seus 10 sucessores. digo que ele aproxima-se de algo ideal
    return grau
        



if __name__ == "__main__":
    possiveis_pontos_pra_teste= [ [(0, 2), (1, 3), (2, 5), (3, 4), (4, 9), (5, 16)],[(0, 1), (1, 2), (2, 2.8), (3, 3.5), (4, 5)],[(0, 1), (1, 3), (2, 7), (3, 13), (4, 21)],[(0, 0), (1, 1), (2, 8), (3, 27), (4, 64)],[(0, 1), (1, 1), (2, 2), (3, 3), (4, 5)]  ]
    # Criei diversos exemplos para serem sorteados e testados, visando sobretudo testar o funcionamento da questão 2.
    Pontos=np.array(possiveis_pontos_pra_teste[choice(range(len(possiveis_pontos_pra_teste)))])
    qual_questão_testar= int(input("Qual questão quer testar: "))
    if qual_questão_testar==1:
        grau = int(input("Informe o grau do polinômio: "))
    if qual_questão_testar==2:
        grau= melhor_grau(Pontos)
    modelo = aproximador_de_nuvem_pol(grau)
    modelo.ajustar(Pontos)

    # Plot do ajuste
    modelo.plotar(Pontos)
