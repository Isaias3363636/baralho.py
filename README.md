# baralho.py

from random import shuffle

class Baralho:
    def __init__(self):
        self.__valores = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
        self.__naipes  = ['PAUS', 'OUROS', 'COPAS', 'ESPADAS']
        self.__cartas = self.__gerar_baralho()

    def __gerar_baralho(self):
        return [f'{self.__valor_str(valor)} de {naipe}' for naipe in self.__naipes for valor in self.__valores]

    def __valor_str(self, valor):
        match valor:
            case 1:
                return 'Ás'
            case 11:
                return 'Valete'
            case 12:
                return 'Dama'
            case 13:
                return 'Rei'
            case _:
                return str(valor)

    def embaralhar(self):
        shuffle(self.__cartas)
        
    def distribuir(self, jogador_a: list[str], jogador_b: list[str]):
        cartas_por_jogador = len(self.__cartas) // 2
        for _ in range(cartas_por_jogador):
            jogador_a.append(self.__cartas.pop(0))
            jogador_b.append(self.__cartas.pop(0))

    def obter_cartas(self):
        return self.__cartas
    

class Batalha:
    def __init__(self):
        self.baralho = Baralho()
        self.jogador_a = []
        self.jogador_b = []
        self.acumuladas = []
        
    def iniciar(self):
        nome_a = input('Nome do jogador 1: ')
        nome_b = input('Informe o nome do jogador 2: ')
        
        self.baralho.embaralhar()
        self.baralho.distribuir(self.jogador_a, self.jogador_b)
        
        print(f'Cartas distribuídas: Jogador 1 - {len(self.jogador_a)} cartas, Jogador 2 - {len(self.jogador_b)} cartas')
        
        rodada = 1
        while self.jogador_a and self.jogador_b and rodada <= self.n_max_rodadas:
            print(f'\n--- Rodada {rodada} ---')

            # Verificar se algum jogador não tem mais cartas
            if not self.jogador_a or not self.jogador_b:
                break  # Interrompe o loop se algum jogador ficar sem cartas
            print(f"Cartas restantes: Jogador 1 - {len(self.jogador_a)} cartas, Jogador 2 - {len(self.jogador_b)} cartas")
        
            carta_a = self.jogador_a.pop(0)
            carta_b = self.jogador_b.pop(0)
        
            print(f"{nome_a} jogou: {carta_a}")
            print(f"{nome_b} jogou: {carta_b}")
            
            valor_a = self.__extrair_valor(carta_a)
            valor_b = self.__extrair_valor(carta_b)
        
            if valor_a > valor_b:
                print(f'{nome_a} venceu a rodada')
                self.jogador_a.extend([carta_a, carta_b] + self.acumuladas)
                self.acumuladas.clear()
            elif valor_b > valor_a:
                print(f'{nome_b} venceu a rodada')
                self.jogador_b.extend([carta_a, carta_b] + self.acumuladas)
                self.acumuladas.clear()
            else:
                print('Deu empate!')
                self.acumuladas.extend([carta_a, carta_b])
        
            rodada += 1

        self.exibir_vencedor(nome_a, nome_b)
        
    def __extrair_valor(self, carta: str) -> int:
        partes = carta.split()
        valor_str = partes[0]
        
        valores = {
            'Ás': 1, 'Valete': 11, 'Dama': 12, 'Rei': 13
        }
    
        if valor_str in valores:
            return valores[valor_str]
    
        try:
            return int(valor_str)
        except ValueError:
            raise ValueError(f"Valor da carta '{valor_str}' inválido")


    def exibir_vencedor(self, nome_a, nome_b):
        print("\n--- x --- x ---")
        if self.jogador_a:
            print(f'\n{nome_a} venceu o jogo')
            print(f'{len(self.jogador_a)} cartas restantes')
        elif self.jogador_b:
            print(f'\n{nome_b} venceu o jogo')
            print(f'{len(self.jogador_b)} cartas restantes')
        else:
            print('\nEmpatado')

if __name__ == "__main__":
    batalha = Batalha(numero_jogadores=2, n_max_rodadas=2000)
    batalha.iniciar()
