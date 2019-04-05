# RPS-Game---Projeto-Final-Udacity
Projeto Final para Udacity



import random

moves = ['rock', 'paper', 'scissors']


class Player():
    def __init__(self):
        self.score = 0

    def move(self):
        pass

    def learn(self, learn_move):
        pass


#Fiz a classe HumanPlayer conforme eu faria e saberia fazer, com "IF",
#entretanto, não está funcionando, eu digito a opção de jogada e ele não
#assimila, eu não sei porque.
class HumanPlayer(Player):
    def move(self):

        humanmove = input('rock, paper, scissors? >')
        if humanmove == 'rock':
            humanmove == moves[0]
        elif humanmove == 'paper':
            humanmove == moves[1]
        elif humanmove == 'scissors':
            humanmove == moves[2]
        else:
            print("Opção inválida! Tente novamente")
            humanmove = input('rock, paper, scissors? >')
            return (humanmove)

# Essa classe está funcionando normalmente, eu renomeei a variável justamente
# para identificar o 'move' de cada jogador diferente.

class RandomPlayer(Player):
    def move(self):
        randomove = random.choice(moves)
        return (randomove)


# Eu refiz essa classe com uma nomenclatura diferente mais para eu me achar
# no código e também para ela abarcar o jogo de melhor de 5, que acho melhor
# para extrapolar as opções, desta forma, a CyclePlayer começa jogando rock
# depois na quarta rodada começa jogando rock novamente e depois paper de novo.
class CyclePlayer(Player):
    def __init__(self):
        Player.__init__(self)
        self.turn = 0

    def move(self):
        cycmove = random.choice(moves)
        if self.turn == 0:
            cycmove = moves[0]
            self.turn = self.turn + 1
        elif self.turn == 1:
            cycmove = moves[1]
            self.turn = self.turn + 1
        elif self.turn == 2:
            cycmove = moves[2]
            self.turn = self.turn + 1
        elif self.turn == 3:
            cycmove = moves[0]
            self.turn = self.turn + 1
        else:
            self.turn == 4
            cycmove = moves[1]
            self.turn = self.turn + 1
        return (cycmove)

# Essa classe eu não sei o que fazer, eu tentei colocar uma função random para
# quando a jogada do HumanPlayer na primeira rodada fosse 'none', mas não
# funcionou, o intuito era que a primeira jogada do ReflectPlayer fosse aleatória
# também não sei porque não funcionou.
class ReflectPlayer(Player):
    def __init__(self):

        Player.__init__(self)
        self.learn_move = None

    def move(self):
        if self.learn_move is None:
            refmove = random.choice(moves)

        else:
            refmove = self.learn_move
            return (refmove)
#Eu testei essa função inúmeras vezes, eu não sei porque, mas ela não está
# contando a pontuação, mesmo comigo não conseguindo fazer a jogada do HumanPlayer
# não está contando vitória para o oponente.
    def learn(self, learn_move):
        self.learn_move = learn_move


class Game():
    def __init__(self, p2):
        self.p1 = HumanPlayer()
        self.p2 = p2


    def play_round(self):
        playermove1 = self.p1.move()
        playermove2 = self.p2.move()
        resultado = Game.play(playermove1, playermove2)
        self.p1.learn(playermove1)
        self.p2.learn(playermove2)


    def play_single(self):
        print("RPS game!")
        print (f"Rodada única:")
        self.play_round()
        if self.p1.score == self.p2.score:
            print('O jogo deu empate!')
        elif self.p1.score > self.p2.score:
            print('Jogador 1 ganhou!')
        else:
            print('Jogador 2 ganhou!')
        print('Placar final: ' + str(self.p1.score) + ' a ' +
              str(self.p2.score))

    def play_game(self):
        print("RPS game!")
        for round in [1, 2, 3, 4, 5]:
            print (f"Rodada {round}:")
            self.play_round()
        if self.p1.score == self.p2.score:
            print('O jogo deu empate!')
        elif self.p1.score < self.p2.score:
            print('Jogador 2 ganhou!')
        else:
            print('Jogador 1 ganhou!')
        print('Placar final: ' + str(self.p1.score) + ' a ' +
              str(self.p2.score))


    def play(self, playermove1, playermove2):
            print(f"Tu jogou {playermove1}")
            print(f"Oponente jogou {playermove2}")
            if beats(playermove1, playermove2):
                print (" JOGADOR 1 VENCEU ")
                print(f"Pontos: Jogador 1: {playermove1} x Jogador 2: {playermove2}\n\n")
                self.p1.score += 1
                return 1
            elif beats(playermove1, playermove2):
                print (" JOGADOR 2 VENCEU! ")

                print(f"Pontos: Jogador 1: {playermove1} x Jogador 2: {playermove2}\n\n")
                self.p2.score += 1
                return 2
            else:
                print ("EMPATOU!")
                print(f"Pontos: Jogador 1: {playermove1} x Jogador 2: {playermove2}\n\n")
                return 0


def beats(one, two):
    return ((one == 'rock' and two == 'scissors') or
            (one == 'scissors' and two == 'paper') or
            (one == 'paper' and two == 'rock'))

if __name__ == '__main__':

# Eu tirei os outros módulos de jogo e deixei só os que eu saberia fazer mesmo
# não vejo sentido no módulo 'rock' de sempre jogar a mesma jogada.
p2 = input('Modo de jogo?\
    1-Aleatório, 2-Cíclico, ou 3-Refletivo: >')

    if p2 == '1':
        p2 = RandomPlayer()
    elif p2 == '2':
        p2 = CyclePlayer()
    elif p2 == '3':
        p2 = ReflectPlayer()
    else:
        p2 = RandomPlayer()

    rounds = input('Aperte [u] para 1 rodada ou [c] para um jogo completo: >')
    Game = Game(p2)
    while True:
        if rounds == 'u':
            Game.play_single()
            break
        elif rounds == 'c':
            Game.play_game()
            break
        elif rounds != 'u' and rounds != 'c':
            print('Tente novamente')
            rounds = input('Aperte 1 para 1 rodada ou 2 para uma partida completa: >')
