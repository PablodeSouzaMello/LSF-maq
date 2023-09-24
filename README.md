from tkinter import *
from tkinter import Tk, ttk
import pyfirmata
import time
board = pyfirmata.Arduino('COM3')
iterator = pyfirmata.util.Iterator(board)
iterator.start()
time.sleep(2)
z = board.get_pin('d:12:i')
x = board.get_pin('d:11:i')
y = board.get_pin('d:10:o')
a = board.get_pin("d:9:i")
b = board.get_pin('d:8:o')
import pandas as pd
tabela  = pd.read_excel("2.1 Lista LSF Térreo - Wander (1).xlsx",skiprows = range(0, 2))
def setup():
    print("inicio do setup")
    board.digital[8].write(1) 
    tela.quit()
    time.sleep(15)
    tela1= Tk()
    tela1.title('Máquina de Perfilar')
    start=Label(tela1, text="modo setup")
    botaoligar2= Button(tela1, text="encerar setup", command= setup_fim )
    botaoligar2.grid(column=0, row=3)
    tela1.mainloop()
def programa_base():
    import pandas as pd
    tabela  = pd.read_excel("2.1 Lista LSF Térreo - Wander (1).xlsx",skiprows = range(0, 2))
    for i in tabela.index:
        if tabela['DESCRIÇÃO'][i]!= "ME 90":
            tabela = tabela.drop(labels=i, axis=0)
    print ("Lista de tarefas carregada")
    tabela.reset_index(inplace=True, drop=True)
    time.sleep(2)
    w=0
    X = 0
    A = 0
    time.sleep(1)
    print("arduino carregado")
    board.digital[8].write(1)
    for i in tabela.index:
        if ((tabela['QTD,'][i]) == 0):
            print('fim')    
        else:
            while (tabela['QTD,'][i]) != 0:
                print(tabela['QTD,'][i]) 
                qtd = tabela['QTD,'][i]
                qtd = int(qtd)
                print(qtd)
                print("motor ligado")
                board.digital[8].write(1)
                while True:
                    if qtd >= 0:
                        while True:
                            if  qtd >= 0:
                                if ((tabela['COMPRIMENTO'][i]) >= w):
                                    c =board.digital[7].read()
                                    z =board.digital[12].read()
                                    time.sleep(0.015)
                                    if z !=0 :
                                        w =w +5.49778715
                                    print(w)
                                else:
                                    print(tabela['COMPRIMENTO'][i]) 
                                    print("Corte acionado")
                                    board.digital[10].write(1)#morsa
                                    time.sleep(0.5)
                                    x = board.digital[11].read()#fc morsa
                                    if x!=0:
                                        print("Guilhotina")                                 
                                        a = board.digital[9].read()#fcguilhotina
                                    if a!=0:
                                        print("fim do corte") 
                                        time.sleep(0.5)
                                        board.digital[10].write(0)#morsa
                                        time.sleep(0.5)
                                        if x==0:
                                            print("retorna morsa")
                                        (qtd)=(qtd)-1
                                        print(qtd)
                                        w = 0
                                    print("fim do corte")
                            else:
                                i=i+1
                                qtd = tabela['QTD,'][i]
                                qtd = int(qtd)
                                print (qtd)
                    else:
                        i=i+1
                qtd = qtd = tabela['QTD,'][i]
            i=i+1
            board.digital[8].write(0)
    board.digital[8].write(0)
def setup_fim():
    tela1 =Tk() 
    board.digital[8].write(0)
    time.sleep(10)
    board.digital[10].write(1)
    time.sleep(5)
    board.digital[10].write(0)
    print("Fim do Setup")
    tela1.quit()
    tela= Tk()
    tela.title('Máquina de Perfilar')
    start=Label(tela, text="Ligar")
    start.grid(column= 0, row= 0)

tela= Tk()
tela.title('Máquina de Perfilar')
start=Label(tela, text="Ligar")
start.grid(column= 0, row= 0)
botaoligar= Button(tela, text="Iniciar Ciclo", command=programa_base)
botaoligar.grid(column=0, row=1)
botaoligar1= Button(tela, text="Iniciar Setup", command=setup)
botaoligar1.grid(column=0, row=2)
tela.mainloop()
