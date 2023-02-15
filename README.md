# turtle

Jogo criado através da biblioteca Turtle da linguagem python 
#Aluno(a): Maria Julia Siqueia de Souza
#Curso: Ciência da Computação

import turtle
import random



janela = turtle.Screen() #criei a janela
janela.title('jogo caça fantasma') #titulo da janela
janela.bgpic('cenario4.gif') #adicionar o gif
janela.tracer(0) #cancelar animação
janela.setup(710, 560) #definir comprimento e altura da janela

gasolina = 10
distancia = 0

#adicionar shapes
janela.addshape('cenario4.gif') #shapes da pasta para serem chamados em outra parte do código
janela.addshape('fantasma.gif')
janela.addshape('caça fantasma.gif')
janela.addshape('morcego.gif')
janela.addshape('fundo igreja.gif')
janela.addshape('fundo igreja2.gif')
janela.addshape('abobora 2.gif')


#personagem principal
fantasminha = turtle.Turtle()
fantasminha.speed(0) #velocidade
fantasminha.shape('fantasma.gif')
fantasminha.penup() #parar de riscar
fantasminha.goto(500,0)


#obstaculo (caça fantasma
obstaculo = turtle.Turtle()
obstaculo.speed(0)
obstaculo.penup()
obstaculo.shape('caça fantasma.gif')
obstaculo.goto(600, random.randint(-100, 100)) #retorna o obstaculo em uma coordenada aleatoria entre -100 e 100, coordenadas incluidas
#600 é a posição x do obstáculo

#gasolina
combustivel1 = turtle.Turtle()
combustivel1.speed(0) #velocidade do combustível
combustivel1.penup()
combustivel1.shape('morcego.gif') #chamar o addshape
combustivel1.goto(300, random.randint(-100, 100)) #aleatoriedade


#borda de abobora (parte esquerda
bor = turtle.Turtle()
bor.shape('fundo igreja.gif') #adicionei gif
bor.penup() #levantar caneta
bor.goto(-220,-142) #coordenadas da borda


#borda de abobora (parte direita
moldura = turtle.Turtle()
moldura.shape('fundo igreja.gif')
moldura.penup()
moldura.goto(140, -150) #coordenadas


#borda cima
borda = turtle.Turtle()
borda.shape('abobora 2.gif')
borda.penup()
borda.goto(-8, 158)

#score e gasolina (texto)
texto = turtle.Turtle()
texto.penup()
texto.color('white')
texto.goto(-320, 190) #coordenada do texto na tela
texto.ht() #hide turtle - sumir a caneta depois de riscar


def moverparabaixo():
    if fantasminha.ycor() >= -100: #se o fantasminha estiver na coordenada >= -100 vai executar
        y = fantasminha.ycor() #variavel temporaria criada dentro da função
        y -= 5 #diminui 5 da coordenada em que o fantasma está no momento
        fantasminha.sety(y) #setar = definir - estou definindo y do fantasma como o valor que está na variável y
    else:
        texto.clear()
        turtle.done()

def moverparacima():
    if fantasminha.ycor() <= 100:
        y = fantasminha.ycor()
        y += 5
        fantasminha.sety(y)
    else:
        texto.clear()
        turtle.done()


def colisao():
    global gasolina

    obstaculo.back(8) # "velocidade" que a animação cancelada aparece na tela
    combustivel1.back(4) # "velocidade que o combustivel aparece (em menor quantidade que o obstaculo)

    if obstaculo.xcor() <= -600:
        obstaculo.goto(300, random.randint(-100, 100))

    if obstaculo.xcor() + 30 >= fantasminha.xcor() - 30 and obstaculo.xcor() - 30 <= fantasminha.xcor() + 30 and obstaculo.ycor() + 25 >= fantasminha.ycor() - 25 and obstaculo.ycor() - 25 <= fantasminha.ycor() + 25:
        turtle.done()


    if combustivel1.xcor() <= -600:
        combustivel1.goto(300, random.randint(-100, 100))

    if combustivel1.xcor() + 20 >= fantasminha.xcor() - 20 and combustivel1.xcor() - 20 <= fantasminha.xcor() + 20 and combustivel1.ycor() + 25 >= fantasminha.ycor() - 25 and combustivel1.ycor() - 25 <= fantasminha.ycor() + 25:
        combustivel1.goto(300, random.randint(-100, 100))
        gasolina += 2


def atualizarTabela(): #função que atuzaliza direto
    global distancia #variavel criada fora da função e usando função global eu chamei para dentro da função
    global gasolina

    distancia = distancia + 0.05
    gasolina = gasolina - 0.02

    if gasolina <= 0: #se a gasolina acabar o jogo finaliza
        turtle.done()

    texto.clear() #limpar o texto sempre que for atualizado
    texto.write(f' SCORE: {{:.0f}}\n GASOLINA: {{:.0f}}'.format(distancia, gasolina), font=('Arial', 20, 'normal'))

def movimentacoes(): #função que fica atualizando os moviemntos dos peronagens toda hora #função que fica chamando as outras funçoes

    atualizarTabela()
    colisao()

    janela.onkeypress(moverparacima, 'w') #tecla definina para o movimento
    janela.onkeypress(moverparabaixo, 's') #tecla definida para o movimento
    janela.listen() #janela escuta os comandos

    janela.update() #janela atualizar a tela
    turtle.ontimer(movimentacoes, 1000 // 60) #comando para determinar de quanto em quanto tempo essa função vai ser executada


def start(): #função que inicia o jogo
    global gasolina #chamei a varivel dentro da função
    global distancia


    combustivel1.goto(300, random.randint(-100, 100))
    obstaculo.goto(300, random.randint(-100, 100))
    gasolina = 10
    distancia = 0


    fantasminha.goto(-300, 0)
    texto.clear()

    turtle.ontimer(movimentacoes, 1000 // 60)


janela.onkeypress(start, 'space')
janela.listen()



janela.mainloop()

