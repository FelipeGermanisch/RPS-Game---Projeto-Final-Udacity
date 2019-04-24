# RPS-Game---Projeto-Final-Udacity
# Projeto Final para Udacity
# Utilizei como inspiração o código do colega no link abaixo:
# "https://github.com/ctd1077/Rock-Paper-Scissors/blob/master/rps.py"

import random

moves = ['rock', 'paper', 'scissors']


class Player():
    def __init__(self):
        self.score = 0

    def move(self):
        pass

    def learn(self, learn_move):
        pass


class HumanPlayer(Player):
    def move(self):

        humove = input('rock, paper, scissors? >')

        while humove != 'rock'and humove != 'paper'and humove != 'scissors':
            print("Opção inválida! Tente novamente")
            humove = input('rock, paper, scissors? >')
        return (humove)

class RandomPlayer(Player):
    def move(self):
        randomove = random.choice(moves)
        return (randomove)


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


class ReflectPlayer(Player):

    def __init__(self, p1):
        self.p1 = p1
        Player.__init__(self)
        self.learn_move = None

    def learn(self, learn_move):
        self.learn_move = learn_move

    def move(self):
        if self.learn_move is None:
            return random.choice(moves)
        else:
            return self.learn_move


class RockPlayer(Player):
    def move(self):
        rockmove = moves[0]
        return (rockmove)


class Game():
    def __init__(self, p1, p2):
        self.p1 = p1
        self.p2 = p2

    def play_round(self):
        jogada1 = self.p1.move()
        jogada2 = self.p2.move()
        resultado = Game.play(jogada1, jogada2)
        self.p1.learn(jogada2)
        self.p2.learn(jogada1)

    def play_single(self):
        print("RPS game!")
        print(f"Rodada única:")
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
            print(f"Rodada {round}:")
            self.play_round()
        if self.p1.score == self.p2.score:
            print('O jogo deu empate!')
        elif self.p1.score < self.p2.score:
            print('Jogador 2 ganhou!')
        else:
            print('Jogador 1 ganhou!')
        print('Placar final: ' + str(self.p1.score) + ' a ' +
              str(self.p2.score))

    def play(self, jogada1, jogada2):
        print(f"Tu jogou {jogada1}")
        print(f"Oponente jogou {jogada2}")
        if onewins(jogada1, jogada2):
            print(" JOGADOR 1 VENCEU ")
            self.p1.score += 1
            print(f"Pontos: P1: {self.p1.score} x P2: {self.p2.score}\n\n")
            return 1
        elif (jogada1 == jogada2):
            print("EMPATOU!")
            print(f"Pontos: P1: {self.p1.score} x P2: {self.p2.score}\n\n")
            return 0
        else:
            print(" JOGADOR 2 VENCEU! ")
            self.p2.score += 1
            print(f"Pontos: P1: {self.p1.score} x P2: {self.p2.score}\n\n")
            return 2


def onewins(one, two):
    return ((one == 'rock' and two == 'scissors') or
            (one == 'scissors' and two == 'paper') or
            (one == 'paper' and two == 'rock'))


if __name__ == '__main__':
    p1 = HumanPlayer()
    p2 = input('Modo de jogo?\
   1-Aleatório, 2-Cíclico, 3-Refletivo ou 4-Rock: >')
if p2 == '1':
    p2 = RandomPlayer()
elif p2 == '2':
    p2 = CyclePlayer()
elif p2 == '3':
    p2 = ReflectPlayer(p1)
elif p2 == '4':
    p2 = RockPlayer()
else:
    p2 = RandomPlayer()

rounds = input('Tecle [u] para 1 rodada ou [c] para jogo completo: >')
Game = Game(p1, p2)
while True:
    if rounds == 'u':
        Game.play_single()
        break
    elif rounds == 'c':
        Game.play_game()
        break
    else:
        print('Opção inválida, tente novamente')
        rounds = input('Tecle [u] para 1 rodada ou [c] para jogo completo: >')
        break

