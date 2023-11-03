# Batalla_Naval_Una_Fila
Consta de un arreglo de n cantidad de casillas donde se ubica cierta cantidad de barcos predeterminados donde estarán ocultos hasta acabar el juego derrumbado todos los barcos, también ubica una posición inicial aleatoria donde empezara el juego.

#############_______Presentacion de juego batalla naval________###############################
"""
Lenguajes automatizados

Integrantes:
Jandy Torres
Breiner Salinas
Anderson Larrahondo

Grupo: 801M
"""
import random

mar = " " # indica que no hay nada en esa casilla
punto = "P" # indica posicion de inicio
submaniro = "S"  # Ocupa una celda
destructor = "D"  # Ocupa dos celdas
disparo_fallido = "-" # Indica que el disparo fallo
disparo_acertado = "*" # Indica que el disparo acerto
barco_una_casilla = 6 # Cantidad de barcos
barco_dos_casilla = 5 # Cantidad de barcos
num_fila = 25 # Cantidad de posiciones del arreglo


def matriz_inicial():
    matriz=[]
    for i in range(num_fila):
        matriz.append(mar)

    return matriz

def posicion_aleatoria():
    return random.randint(0, num_fila-1)

def casilla_vacia(coord, matriz):
    return matriz[coord] == mar




##############_imprime matriz__#######################

def imprimir_matriz(matriz, deberia_mostrar_barcos):

    imprimir_separador_horizontal()
    for y in range(num_fila):
        print("| ", end="")
        celda = matriz[y]
        valor_real = celda
        if not deberia_mostrar_barcos and valor_real != mar and valor_real != disparo_fallido and valor_real != disparo_acertado:
            if valor_real == punto:
                valor_real = punto
            else:
                valor_real = " "
        print(f" {valor_real} ", end="")
    print("|")  # Salto de línea
    imprimir_separador_horizontal()
    imprimir_fila_de_numeros()
    imprimir_separador_horizontal()

def imprimir_separador_horizontal():
    # Imprimir un renglón dependiendo de las columnas
    for _ in range(num_fila):
        print("+----", end="")
    print("+")

def imprimir_fila_de_numeros():
    for x in range(num_fila):
        if x <= 8:
            print(f"|  {x+1} ", end="")
        else:
            print(f"| {x+1} ", end="")
    print("|")



#########_____movimiento__________##########################
def mover(matriz, punto_in):

    movimiento = punto_in
    print("Estas en la posicion ",movimiento+1)
    while True:
        print("dr = direccion a la derecha.")
        print("iz = direccion a la izquierda.")
        mov = input("Ingrese que direccion quieres tomar: ")
        cant = int(input("Cuantas casillas quieres moverte: "))
        

        if mov == "dr":
            movimiento = punto_in + cant
            if movimiento <= num_fila-1:
                if matriz[movimiento] == mar or matriz[movimiento] == punto:
                    matriz[movimiento] = disparo_fallido
                else:
                    matriz[movimiento] = disparo_acertado
            else:
                print("No es automata, esta en el final de la matriz.")
                continue

        
        elif mov == "iz":
            movimiento = punto_in - cant
            if movimiento < 0:
                print("No es automata, esta en el final de la matriz.")
                continue
            else:
                if matriz[movimiento] == mar or matriz[movimiento] == punto:
                    matriz[movimiento] = disparo_fallido
                else:
                    matriz[movimiento] = disparo_acertado
        else:
            print("Valor invalido, ingrese un valor correcto (dr) o (iz).") 

        #print("Estas en la casilla ",movimiento+1)
        
        return matriz, movimiento

###############_______Barcos hundidos__________#################

def todos_los_barcos_hundidos(matriz):
    for y in range(num_fila):
            celda = matriz[y]
            if celda == punto:
                matriz[y] = disparo_fallido
            # Si no es mar o un disparo, significa que todavía hay un barco por ahí
            elif celda != mar and celda != disparo_acertado and celda != disparo_fallido:
                
                return False
    # Acabamos de recorrer toda la matriz y no regresamos en la línea anterior. Entonces todos los barcos han sido hundidos
    
    return True

######_____Coloca_barcos__________############################################
def colocar_barcos(matriz):
    

    print= "Hay {} barcos de 1 casillas.".format(barco_una_casilla)
    print= "Hay {} barcos de 1 casillas.".format(barco_dos_casilla)

    matriz = coloca_barco_dos_casillas(barco_dos_casilla, destructor, matriz)
    matriz = coloca_barco_una_casillas(barco_una_casilla, submaniro, matriz)
    matriz, posicion = punto_inicio(1, punto, matriz)

    return matriz, posicion

def coloca_barco_una_casillas(cantidad, tipo_barco, matriz):
    barco_colocado = 0
    while True:
        pos = posicion_aleatoria()
        if casilla_vacia(pos, matriz):
            matriz[pos] = tipo_barco
            barco_colocado += 1
        if barco_colocado >= cantidad:
            break
    return matriz

def coloca_barco_dos_casillas(cantidad, tipo_barco, matriz):
    barco_colocado = 0
    while True:
        pos = posicion_aleatoria()
        pos2 = pos+1
        if pos >= 0 and pos <= num_fila-1 and pos2 >= 0 and pos2 <= num_fila-1 and casilla_vacia(pos, matriz) and casilla_vacia(pos2, matriz):
            matriz[pos] = tipo_barco
            matriz[pos2] = tipo_barco
            barco_colocado += 1
        if barco_colocado >= cantidad:
            break
    return matriz

############______Coloca punto punto de inicio__________################
def punto_inicio(cantidad, tipo_barco, matriz):
    punto = 0
    while True:
        pos = posicion_aleatoria()
        if casilla_vacia(pos, matriz):
            matriz[pos] = tipo_barco
            Pos_inicio = pos
            punto += 1
        if punto >= cantidad:
            break
    return matriz,Pos_inicio



########_________iniciar juego__________##########
pun=0
mat = matriz_inicial()
mat, pun = colocar_barcos(mat)

print(f"Hay {barco_una_casilla} barcos de 1 casillas.", end = "\n")
print(f"Hay {barco_dos_casilla} barcos de 2 casillas.", end = "\n")

while True:
    imprimir_matriz(mat, False)
    mat, pun = mover(mat, pun)
    
    if todos_los_barcos_hundidos(mat):

        print("-------------------------------------")
        print("Acabas de hundir todos los barcos")
        print("¡¡¡ FELIDADES GANASTE !!!")
        print("-------------------------------------")
        break
    else:
        print("No has terminado de hundir los barcos")
    
