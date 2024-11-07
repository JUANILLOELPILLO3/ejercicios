import mysql.connector
import funciones as er

# Establecemos la conexión con la base de datos
conexion = mysql.connector.connect(
    host="localhost",       # Dirección del servidor (localhost para base de datos local)
    user="root",            # Usuario de la base de datos
    password="curso",       # Contraseña del usuario
    database="supermercado" # Nombre de la base de datos
)

if conexion.is_connected():
    print("Conexión exitosa a la base de datos")

cursor = conexion.cursor()
conexion.commit()
def menu():
# Menú de opciones
    while True:
        print("A continuación, selecciona una de las opciones:\n\
            1. Crear una nueva categoría\n\
            2. Leer categorías existentes\n\
            3. Actualizar una categoría\n\
            4. Eliminar una categoría\n\
            5. Salir")
            
        opcion = int(input("¿Cuál es tu elección?: "))
        
        if opcion == 1:
            id_categoria = int(input('Ingresa el nuevo ID para la nueva categoría: '))
            nombre_categoria = input('Ingresa el nombre de la nueva categoría: ')
            query = "INSERT INTO categoria (idcategoria, categoria) VALUES (%s, %s)"
            cursor.execute(query, (id_categoria, nombre_categoria))
            conexion.commit()
            print(f'La categoría "{nombre_categoria}" ha sido creada correctamente.')
        elif opcion == 2:
             cursor.execute("SELECT * FROM categoria")
             categorias = cursor.fetchall()
             print("Categorías existentes:")
             for categoria in categorias:
              print(f"ID: {categoria[0]}, Nombre: {categoria[1]}")
        elif opcion == 3:
            id_categoria = int(input('Ingresa el ID de la categoría a actualizar: '))
            nuevo_nombre = input('Ingresa el nuevo nombre de la categoría: ')
            query = "UPDATE categoria SET categoria = %s WHERE idcategoria = %s"
            cursor.execute(query, (nuevo_nombre, id_categoria))
            conexion.commit()
            print(f'La categoría con ID {id_categoria} ha sido actualizada a "{nuevo_nombre}".')
        elif opcion == 4:
            id_categoria = int(input('Ingresa el ID de la categoría que deseas eliminar: '))
            query = "DELETE FROM categoria WHERE idcategoria = %s"
            cursor.execute(query, (id_categoria,))
            conexion.commit()
            print(f'La categoría con ID {id_categoria} ha sido eliminada.')
                
        elif opcion == 5:
            # Salir del programa
            break
  
menu()
