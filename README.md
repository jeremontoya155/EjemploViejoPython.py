# EjemploViejoPython.py
ultimo parcial facultad con las pruebas y errores anteriores
"""# REGISTROS

class Equipo:
    def __init__(self, conf, nom, punt, camp):
        self.confederacion = conf
        self.nombre = nom
        self.puntos = punt
        self.campeonatos = camp


class Clasificados:
    def __init__(self, nom, punt, camp):
        self.nombre = nom
        self.puntos = punt
        self.campeonatos = camp


# TABLAS


def cuerpo():
    linea = '{:<11}\t| {:<30} | {:^4}| {:^10}\t\t|'
    return linea.format('Confederacion', 'Nombre', 'Puntos', "Campeonatos")


def write(eq):
    linea = '{:^11} \t| {:<30} | {:^4}  | {:^15}  |'
    return linea.format(eq.confederacion, eq.nombre, eq.puntos, eq.campeonatos)


def cuerpo_clasificados():
    linea = '{:<35}\t{:>15}\t{:>15}'
    return linea.format('Nombre', 'Puntos', "Campeonatos")


def write_clasificados(eqs):
    linea = '{:<35}\t{:>15}\t{:>10}'
    return linea.format(eqs.nombre, eqs.puntos, eqs.campeonatos)


# SEPARACION

def str_toequipo(linea):
    token = linea.split(',')
    confederacion = int(token[0])
    nombre = token[1]
    puntos = int(token[2])
    campeonatos = int(token[3])
    equipo = Equipo(confederacion, nombre, puntos, campeonatos)
    return equipo


def str_toclasificados(linea):
    token = linea.split(',')
    nombre = token[0]
    puntos = int(token[1])
    campeonatos = int(token[2])
    equipos = Clasificados(nombre, puntos, campeonatos)
    return equipos


#-----------------------------------------------------------------------------------------------------------
import random
import pickle
import os.path
import os



# MENU DE OPCIONES

def menu():
    print(120 * "═")
    print("╚Ingrese la opción (1) para mostrar el listado completo de los paises")
    print("\n╚Ingrese la opción (2) para informar cual es el pais con mayor cantidad de campeonatos ganados ")
    print(
        "\n╚Ingrese la opción (3) para determinar para cada confederacion cuantos paises ganaron algun campeonato.")
    print(
        "\n╚Ingrese la opción (4) para generar un archivo con los paises de X confederacion.")
    print(
        "\n╚Ingrese la opción (5) para ingresar una confederacion X y ver cual fue su clasificacion")
    print(
        "\n╚Ingrese la opción (6) para realizar el fixture del proximo mundial.")
    print("\n╚Ingrese la opción (7) para mostrar que grupo le corresponde a un pais X")
    print("\n╚Ingrese la opción (0) para salir. ")
    print(120 * "═")
    return validar_rango(-1, 8, "\t\t■ Ingrese la opción que desee:")


# VALIDACIONES

def validar_rango(inf, sup, mensaje="Ingrese:"):
    n = inf - 1
    while n < inf or n > sup:
        n = int(input(mensaje))
        if n < inf or n > sup:
            print("\t\t■ Error")
    return n


def validate(inf, mensaje):
    n = inf - 1
    while n < inf:
        n = int(input(mensaje))
    return n


def validar_existencia(vector, x):
    ban = False
    for i in range(len(vector)):
        if vector[i].nombre == x:
            ban = True
            break
    return ban


# FUNCIONES CREADORAS

def generar_arreglo(fd):
    vector = []
    ban = True
    if os.path.exists(fd):
        m = open(fd, "rt")
        for linea in m:
            equipo = str_toequipo(linea)
            insertarOrdenado(vector, equipo)
        m.close()
        return vector
    else:
        return vector


def generar_arreglo_b(x, v):
    vector = []
    for j in range(len(v)):
        if v[j].confederacion == x:
            nom = v[j].nombre
            punt = v[j].puntos
            camp = v[j].campeonatos
            equipo = Clasificados(nom, punt, camp)
            insertarOrdenado(vector, equipo)

    return vector


def grabar_archivo(fd, v):
    m = open(fd, 'wb')
    for i in range(len(v)):
        pickle.dump(v[i], m)
    m.close()


def crear_matriz(v):
    filas, columnas = 8, 4
    matriz = [[0] * columnas for filas in range(filas)]
    con = 0
    for c in range(columnas):
        for f in range(filas):
            matriz[f][c] = v[con]
            con += 1
    return matriz


def vector_para_matriz(v, anf):
    primero = anf
    siete = []
    resto = []
    siete.append(primero)
    c = 0
    for j in range(32):
        if primero == v[j].nombre:
            v.pop(j)
    for j in range(len(v)):
        c += 1
        if j < 7:
            agregar = v[j].nombre
            siete.append(agregar)
        else:
            agrego = v[j].nombre
            resto.append(agrego)
        if c == 32:
            break
    random.shuffle(resto)
    # siete.extend(resto)
    suma = siete + resto
    return suma


def alterar_arreglo(v):
    confederaciones = ["UEFA", "CONMEBOL", "CONCACAF", "CAF", "AFC", "OFC"]
    for j in range(len(v)):
        conf = v[j].confederacion
        cambiamos = confederaciones[conf]
        v[j].confederacion = cambiamos


# FUNCIONES DE MUESTRA

def mostrar_referencias_num():
    mensaje = 120 * "═"
    mensaje += "\n"
    mensaje += " ASIGNACION NUMERICA PARA CONFEDERACIONES\n"
    mensaje += " [0] para UEFA\n"
    mensaje += " [1] para CONMEBOL\n"
    mensaje += " [2] para CONCACAF\n"
    mensaje += " [3] para CAF\n"
    mensaje += " [4] para AFC\n"
    mensaje += " [5] para OFC\n"
    mensaje += "═" * 120
    return mensaje


def mostrar_tabla(tabla):
    grupos = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
    for f in range(len(tabla)):
        print("\n")
        print("═" * 120)
        print('\n', "Grupo[", grupos[f], "]", sep='', end='\n')
        for c in range(len(tabla[f])):
            print("\n", tabla[f][c], sep='', end='')


def mostrar_lista(v, mensaje=""):
    print("═" * 36, mensaje, "═" * 60)
    print(cuerpo())
    for j in range(len(v)):
        print(write(v[j]))
    print("═" * 120)


def mostrar_lista_f(v, mensaje=""):
    print("═" * 36, mensaje, "═" * 60)
    print(cuerpo_clasificados())
    for j in range(len(v)):
        print(write_clasificados(v[j]))
    print("═" * 120)


def muestra_confederaciones(v):
    confederaciones = ["UEFA", "CONMEBOL", "CONCACAF", "CAF", "AFC", "OFC"]
    for j in range(len(v)):
        if v[j] != 0:
            mensaje = "\n\t\t■ La cantidad de paises campeones de la {} es de:{}".format(confederaciones[j], v[j])
            print(mensaje)


def leer_archivo(fd):
    if not os.path.exists(fd):
        print("\n\t\t■ Archivo no existente")
    else:
        m = open(fd, "rb")
        t = os.path.getsize(fd)
        print("═" * 49, fd, "═" * 49)
        print("_" * 120)
        print(cuerpo_clasificados())
        print("_" * 120)
        while m.tell() < t:
            vector = pickle.load(m)
            print(write_clasificados(vector))
        m.close()


# FUNCIONES DE ORDENAMIENTO

def insertarOrdenado(v, reg):
    n = len(v)
    pos = n
    izq, der = 0, n - 1
    while izq <= der:
        c = (izq + der) // 2
        if reg.puntos == v[c].puntos:
            pos = c
            break
        if reg.puntos > v[c].puntos:
            der = c - 1
        else:
            izq = c + 1
    if izq > der:
        pos = izq

    v[pos:pos] = [reg]


# FUNCIONES DE CONTEO

def contar_confederaciones_ganadoras(v):
    con = [0] * 6
    for i in range(len(v)):
        if v[i].campeonatos > 0:
            con[v[i].confederacion] += 1
    return con


def buscar_mayores(v, may):
    mayores = []
    c = 0
    for i in range(len(v)):
        if v[i].campeonatos == may:
            mayores.append(v[i])
            c += 1
    return mayores, c


# FUNCIONES DE BUSQUEDA

def buscar_grupo(tabla, x):
    ban = False
    grupos = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
    for f in range(len(tabla)):
        grupo = grupos[f]
        for c in range(len(tabla[f])):
            if tabla[f][c] == x:
                ban = True
                return grupo, ban
    return -1, ban


def buscar_mayor(v):
    mayor = v[0]
    for i in range(1, len(v)):
        if mayor.campeonatos < v[i].campeonatos:
            mayor = v[i]
    return mayor.campeonatos

#---------------------------------------------------------------------------------------------------------
def principal():
    fd = 'paises.csv'
    equipos = generar_arreglo(fd)
    ban = False
    opc = -1

    while opc != 0:

        opc = menu()

        if len(equipos) > 0:
            # Se realiza la muestra de los países procesados del archivo csv
            if opc == 1:
                equipos_2 = generar_arreglo(fd)
                alterar_arreglo(equipos_2)
                mostrar_lista(equipos_2, "Países")

            elif opc == 2:
                may = buscar_mayor(equipos)
                mayores, c = buscar_mayores(equipos, may)
                if c > 1:
                    mostrar_lista(mayores, "Listado de países con mayor cantidad de campeonatos ganados")
                else:
                    ganador = mayores[0].nombre
                    cantidad = mayores[0].campeonatos
                    mensaje = "\n\t\t■ El equipo con mas campeonatos es {} con {} campeonatos".format(ganador, cantidad)
                    print(mensaje)
            elif opc == 3:
                conf_ganadoras = contar_confederaciones_ganadoras(equipos)
                muestra_confederaciones(conf_ganadoras)

            elif opc == 4:
                mostrar_lista(equipos,"Paises")
                print("\n\t\t■ Guiandonos por las referencias pedimos que complete ingresando lo indicado:")
                referencias = mostrar_referencias_num()
                print(referencias)
                x = validar_rango(0, 5, "\n\t\t■ Ingrese la confederacion que desee:")
                fd_2 = "clasificacion[{}].dat".format(x)
                clasificados = generar_arreglo_b(x, equipos)
                #mostrar_lista_f(clasificados, fd)
                grabar_archivo(fd_2, clasificados)
                print("\n\t\t■ Se ha creado el archivo...")

            elif opc == 5:
                print("\n\t\t■ Guiandose por la referencias ingrese lo indicado:")
                referencias = mostrar_referencias_num()
                print(referencias)
                x = validar_rango(0, 5, "\n\t\t■ Ingrese la confederacion que desee buscar como archivo:")
                fd_3 = "clasificacion[{}].dat".format(x)
                if os.path.exists(fd_3):
                    leer_archivo(fd_3)
                else:
                    clasificados = generar_arreglo_b(x, equipos)
                    grabar_archivo(fd_3, clasificados)
                    print("\n\t\t■ Se ha grabado el archivo inexistente...")

            elif opc == 6:
                bandera = False
                while bandera != True:
                    anf = input("Ingrese nombre del anfitrion:")
                    bandera = validar_existencia(equipos, anf)
                    if bandera == True:
                        vec_fixture = vector_para_matriz(equipos, anf)
                        matriz = crear_matriz(vec_fixture)
                        mostrar_tabla(matriz)
                        ban = True
                    else:
                        print("\n\t\t■ No existe ese país vuelva a ingresar otro.")


            elif opc == 7:
                if ban:
                    x = input("\n\t\t■ Ingrese el pais que desee de manera correcta:")
                    grupo, ban_de_fixture = buscar_grupo(matriz, x)
                    if ban_de_fixture:
                        mensaje = "\n\t\t■ El pais {} participa y pertenece al grupo {}".format(x, grupo)
                        print(mensaje)

                    else:
                        print("\n\t\t■ No hubo apariciones en el fixture  del pais  nombrado.")
                else:
                    print("\n\t\t■ Aun no se ha generado el fixture del proximo mundial.")


            elif opc == 0:
                print("\n\t\t■ Gracias por usar el programa")
        else:
            print("\n\t\t■ El archivo no encontrado. Intente ubicando el archivo en la carpeta del programa.")
            print("\n\t\t■ Presione 0 para salir.")


if __name__== "__main__":
    principal()"""

import random
import pickle
import os.path
import os


# REGISTROS

class Equipo:
    def __init__(self, conf, nom, punt, camp):
        self.confederacion = conf
        self.nombre = nom
        self.puntos = punt
        self.campeonatos = camp


class Clasificados:
    def __init__(self, nom, punt, camp):
        self.nombre = nom
        self.puntos = punt
        self.campeonatos = camp


# TABLAS


def cuerpo():
    linea = '{:<11}\t| {:<30} | {:^4}| {:^15}\t|'
    return linea.format('Confederacion', 'Nombre', 'Puntos', "Campeonatos")


def write(eq):
    linea = '{:^11} \t| {:<30} | {:^4}  | {:^15}  |'
    return linea.format(eq.confederacion, eq.nombre, eq.puntos, eq.campeonatos)


def cuerpo_clasificados():
    linea = '{:<35}\t{:>15}\t{:>15}'
    return linea.format('Nombre', 'Puntos', "Campeonatos")


def write_clasificados(eqs):
    linea = '{:<35}\t{:>15}\t{:>10}'
    return linea.format(eqs.nombre, eqs.puntos, eqs.campeonatos)


# SEPARACION

def str_toequipo(linea):
    token = linea.split(',')
    confederacion = int(token[0])
    nombre = token[1]
    puntos = int(token[2])
    campeonatos = int(token[3])
    equipo = Equipo(confederacion, nombre, puntos, campeonatos)
    return equipo


def str_toclasificados(linea):
    token = linea.split(',')
    nombre = token[0]
    puntos = int(token[1])
    campeonatos = int(token[2])
    equipos = Clasificados(nombre, puntos, campeonatos)
    return equipos

# MENU DE OPCIONES

def menu():
    print("\n")
    print(120 * "═")
    print("╚Ingrese la opción (1) para mostrar el listado completo de los paises.")
    print("\n╚Ingrese la opción (2) para informar cual es el pais con mayor cantidad de campeonatos ganados.")
    print(
        "\n╚Ingrese la opción (3) para determinar para cada confederacion cuantos paises ganaron algun campeonato.")
    print(
        "\n╚Ingrese la opción (4) para generar un archivo con los paises de la confedarción que desee.")
    print(
        "\n╚Ingrese la opción (5) para ingresar una confederacion y ver cual fue su clasificacion.")
    print(
        "\n╚Ingrese la opción (6) para realizar el fixture del próximo mundial.")
    print("\n╚Ingrese la opción (7) para mostrar que grupo le corresponde a un pais que desee.")
    print("\n╚Ingrese la opción (0) para salir.")
    print(120 * "═")
    return validar_rango(-1, 8, "\t\t■ Ingrese la opción que desee (de 0 a 7):")


# VALIDACIONES

def validar_rango(inf, sup, mensaje):
    n = inf - 1
    while n <= inf or n >= sup:
        n = int(input(mensaje))
        if n <= inf or n >= sup:
            print("\t\t■ Error ingrese un valor de {} a {}".format(inf + 1, sup - 1))
    return n


def validar_existencia(vector, x):
    ban = False
    for i in range(len(vector)):
        if vector[i].nombre == x:
            ban = True
            break
    return ban


# FUNCIONES CREADORAS

def generar_arreglo(fd):
    vector = []
    if os.path.exists(fd):
        m = open(fd, "rt")
        for linea in m:
            equipo = str_toequipo(linea)
            insertarOrdenado(vector, equipo)
        m.close()
        return vector
    else:
        return vector


def generar_arreglo_b(x, v):
    vector = []
    for j in range(len(v)):
        if v[j].confederacion == x:
            nombre = v[j].nombre
            puntos = v[j].puntos
            campeonatos = v[j].campeonatos
            equipos = Clasificados(nombre, puntos, campeonatos)
            insertarOrdenado(vector, equipos)

    return vector


def grabar_archivo(fd, v):
    m = open(fd, 'wb')
    for i in range(len(v)):
        pickle.dump(v[i], m)
    m.close()


def crear_matriz(v):
    filas, columnas = 8, 4
    matriz = [[0] * columnas for filas in range(filas)]
    con = 0
    for c in range(columnas):
        for f in range(filas):
            matriz[f][c] = v[con]
            con += 1
    return matriz


"""def crear_matriz(v):
    filas, columnas = 9, 5
    mat = [[0] * columnas for i in range(filas)]
    for i in range(len(v)):
        mat[filas][columnas] += v[i]
    return mat"""


def vector_para_matriz(v, anf):
    # primero = anf
    siete = []
    resto = []
    siete.append(anf)
    c = 0
    # for j in range(32):
    #   if primero == v[j].nombre:
    #      v.pop(j)
    buscar_nombre_y_borrar(v, anf)
    for j in range(len(v)):
        c += 1
        if j < 7:
            agregar = v[j].nombre
            siete.append(agregar)
        else:
            agrego = v[j].nombre
            resto.append(agrego)
        if c == 32:
            break
    random.shuffle(resto)
    suma = siete + resto
    return suma


def alterar_arreglo(v):
    confederaciones = ["UEFA", "CONMEBOL", "CONCACAF", "CAF", "AFC", "OFC"]
    for j in range(len(v)):
        conf = v[j].confederacion
        cambiamos = confederaciones[conf]
        v[j].confederacion = cambiamos


# FUNCIONES DE MUESTRA

def mostrar_referencias_num():
    mensaje = "\n"
    mensaje += 120 * "═"
    mensaje += "\n"
    mensaje += " ASIGNACION NUMERICA PARA CONFEDERACIONES\n"
    mensaje += " [0] para UEFA\n"
    mensaje += " [1] para CONMEBOL\n"
    mensaje += " [2] para CONCACAF\n"
    mensaje += " [3] para CAF\n"
    mensaje += " [4] para AFC\n"
    mensaje += " [5] para OFC\n"
    mensaje += "═" * 120
    return mensaje


def mostrar_tabla(tabla):
    grupos = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
    for f in range(len(tabla)):
        print("\n")
        print("═" * 120)
        print('\n', "Grupo[", grupos[f], "]", sep='', end='\n')
        for c in range(len(tabla[f])):
            print("\t\n", tabla[f][c], sep='', end='')


def mostrar_lista(v, mensaje=""):
    print("═" * 36, mensaje, "═" * 60)
    print(cuerpo())
    for j in range(len(v)):
        print(write(v[j]))
    print("═" * 120)


def mostrar_lista_f(v, mensaje=""):
    print("═" * 36, mensaje, "═" * 60)
    print(cuerpo_clasificados())
    for j in range(len(v)):
        print(write_clasificados(v[j]))
    print("═" * 120)


def muestra_confederaciones(v):
    confederaciones = ["UEFA", "CONMEBOL", "CONCACAF", "CAF", "AFC", "OFC"]
    for j in range(len(v)):
        if v[j] != 0:
            mensaje = "\n\t\t■ La cantidad de paises campeones de la {} es de:{}".format(confederaciones[j], v[j])
            print(mensaje)


def leer_archivo(fd):
    if not os.path.exists(fd):
        print("\n\t\t■ Archivo no existente")
    else:
        m = open(fd, "rb")
        t = os.path.getsize(fd)
        print("═" * 49, fd, "═" * 49)
        print("_" * 120)
        print(cuerpo_clasificados())
        print("_" * 120)
        while m.tell() < t:
            vector = pickle.load(m)
            print(write_clasificados(vector))
        m.close()


# FUNCIONES DE ORDENAMIENTO

def insertarOrdenado(v, reg):
    n = len(v)
    pos = n
    izq, der = 0, n - 1
    while izq <= der:
        c = (izq + der) // 2
        if reg.puntos == v[c].puntos:
            pos = c
            break
        if reg.puntos > v[c].puntos:
            der = c - 1
        else:
            izq = c + 1
    if izq > der:
        pos = izq

    v[pos:pos] = [reg]


# FUNCIONES DE CONTEO

def contar_confederaciones_ganadoras(v):
    con = [0] * 6
    for i in range(len(v)):
        if v[i].campeonatos > 0:
            con[v[i].confederacion] += 1
    return con


"""def buscar_mayores(v, may):
    mayores = []
    c = 0
    for i in range(len(v)):
        if v[i].campeonatos == may:
            mayores.append(v[i])
            c += 1
    return mayores, c"""


# FUNCIONES DE BUSQUEDA

def buscar_grupo(tabla, x):
    ban = False
    grupos = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
    for f in range(len(tabla)):
        grupo = grupos[f]
        for c in range(len(tabla[f])):
            if tabla[f][c] == x:
                ban = True
                return grupo, ban
    return -1, ban


def buscar_mayor(v):
    mayor = v[0]
    mayores = [v[0]]
    for i in range(1, len(v)):
        if mayor.campeonatos < v[i].campeonatos:
            mayor = v[i]
            mayores = [mayor]
        elif mayor.campeonatos == v[i].campeonatos:
            mayores.append(v[i])

    return mayores


def buscar_nombre_y_borrar(v, x):
    for i in range(32):
        if x == v[i].nombre:
            v.pop(i)




def principal():
    fd = "paises.csv"
    equipos = generar_arreglo(fd)
    ban = False
    opc = -1

    while opc != 0:

        opc = menu()

        if len(equipos) > 0:
            # Se realiza la muestra de los países procesados del archivo csv
            if opc == 1:
                equipos_2 = generar_arreglo(fd)
                alterar_arreglo(equipos_2)
                mostrar_lista(equipos_2, "Países")

            elif opc == 2:
                mayores= buscar_mayor(equipos)
                #mayores, c = buscar_mayores(equipos, may)
                if len(mayores) > 1:
                    mostrar_lista(mayores, "Listado de países con mayor cantidad de campeonatos ganados")
                else:
                    ganador = mayores[0].nombre
                    cantidad = mayores[0].campeonatos
                    mensaje = "\n\t\t■ El equipo con más campeonatos es {} con {} campeonatos".format(ganador, cantidad)
                    print(mensaje)
            elif opc == 3:
                conf_ganadoras = contar_confederaciones_ganadoras(equipos)
                muestra_confederaciones(conf_ganadoras)

            elif opc == 4:
                print("\n\t\t■ Utilizando las referencias ingrese de cual confederacion crear su clasificación:")
                referencias = mostrar_referencias_num()
                print(referencias)
                x = validar_rango(-1, 6, "\n\t\t■ Confederacion que desea clasificar:")
                fd_2 = "clasificacion[{}].dat".format(x)
                clasificados = generar_arreglo_b(x, equipos)
                grabar_archivo(fd_2, clasificados)
                print("\n\t\t■ Se ha creado el archivo...")

            elif opc == 5:
                print("\n\t\t■ Utilizando las referencias ingrese de cual confederacion desea buscar su clasificación:")
                referencias = mostrar_referencias_num()
                print(referencias)
                x = validar_rango(-1, 6, "\n\t\t■ Ingrese la confederacion que desee buscar como archivo:")
                fd_3 = "clasificacion[{}].dat".format(x)
                if os.path.exists(fd_3):
                    leer_archivo(fd_3)
                else:
                    clasificados = generar_arreglo_b(x, equipos)
                    grabar_archivo(fd_3, clasificados)
                    print("\n\t\t■ Se ha grabado el archivo inexistente...")

            elif opc == 6:
                bandera = False
                while bandera != True:
                    anf = input("\n\t\t■ Ingrese nombre del anfitrion:")
                    bandera = validar_existencia(equipos, anf)
                    if bandera == True:
                        vec_fixture = vector_para_matriz(equipos, anf)
                        matriz = crear_matriz(vec_fixture)
                        mostrar_tabla(matriz)
                        ban = True
                    else:
                        print("\n\t\t■ No existe ese país vuelva a ingresar otro.")


            elif opc == 7:
                if ban:
                    x = input("\n\t\t■ Ingrese el pais que desee de manera correcta:")
                    grupo, ban_de_fixture = buscar_grupo(matriz, x)
                    if ban_de_fixture:
                        mensaje = "\n\t\t■ El pais {} participa y pertenece al grupo {}".format(x, grupo)
                        print(mensaje)

                    else:
                        print("\n\t\t■ No hubo apariciones en el fixture  del pais  nombrado.")
                else:
                    print("\n\t\t■ Aun no se ha generado el fixture del proximo mundial.")


            elif opc == 0:
                print("\n\t\t■ Gracias por usar el programa")
        else:
            print("\n\t\t■ Archivo no encontrado. Intente ubicando el archivo en la carpeta del programa.")
            print("\n\t\t■ Presione 0 para salir.")


if __name__ == "__main__":
    principal()
    
