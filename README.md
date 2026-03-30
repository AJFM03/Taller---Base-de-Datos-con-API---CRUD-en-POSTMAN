# Taller---Base-de-Datos-con-API---CRUD-en-POSTMAN

Aaron Fehrenbach 8-955-295

Con base a la práctica realizada en clase, del uso de base de datos, conectores y API. Se solicita realizar:


I PARTE: En el cual debe hacer el CRUD(Create, Read, Update y Delete):
Vinculación del Código Python con Base de Datos
from flask import Flask, request, jsonify
from flask_mysqldb import MySQL

app = Flask(__name__)

# Configuración de la base de datos
app.config['MYSQL_HOST'] = '127.0.0.1'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = 'Aaron2600'
app.config['MYSQL_DB'] = 'presupuesto_demo'

mysql = MySQL(app)

Que pueda Insertar desde API - INSERT
# 1. INSERT — Crear proveedor (POST)
@app.route('/proveedores', methods=['POST'])
def crear_proveedor():
    datos = request.get_json()
    nombre    = datos['nombre']
    telefono  = datos['telefono']
    correo    = datos['correo']
    direccion = datos['direccion']
    cur = mysql.connection.cursor()
    cur.execute("""
        INSERT INTO proveedores (nombre, telefono, correo, direccion)
        VALUES (%s, %s, %s, %s)
    """, (nombre, telefono, correo, direccion))
    mysql.connection.commit()
    cur.close()
    return jsonify({'mensaje': 'Proveedor creado exitosamente'}), 201
Que pueda consultar desde API - SELECT
# 2. SELECT — Consultar todos (GET)
@app.route('/proveedores', methods=['GET'])
def obtener_proveedores():
    cur = mysql.connection.cursor()
    cur.execute("SELECT * FROM proveedores")
    filas = cur.fetchall()
    cur.close()
    proveedores = []
    for fila in filas:
        proveedores.append({
            'id': fila[0],
            'nombre': fila[1],
            'telefono': fila[2],
            'correo': fila[3],
            'direccion': fila[4]
        })
    return jsonify(proveedores), 200
# SELECT — Consultar uno por ID (GET)
@app.route('/proveedores/<int:id>', methods=['GET'])
def obtener_proveedor(id):
    cur = mysql.connection.cursor()
    cur.execute("SELECT * FROM proveedores WHERE id = %s", (id,))
    fila = cur.fetchone()
    cur.close()
    if fila is None:
        return jsonify({'error': 'Proveedor no encontrado'}), 404
    proveedor = {
        'id': fila[0],
        'nombre': fila[1],
        'telefono': fila[2],
        'correo': fila[3],
        'direccion': fila[4]
    }
    return jsonify(proveedor), 200
Que pueda editar desde API - UPDATE
# 3. UPDATE — Editar proveedor (PUT)
@app.route('/proveedores/<int:id>', methods=['PUT'])
def actualizar_proveedor(id):
    datos = request.get_json()
    nombre    = datos['nombre']
    telefono  = datos['telefono']
    correo    = datos['correo']
    direccion = datos['direccion']
    cur = mysql.connection.cursor()
    cur.execute("""
        UPDATE proveedores
        SET nombre=%s, telefono=%s, correo=%s, direccion=%s
        WHERE id=%s
    """, (nombre, telefono, correo, direccion, id))
    mysql.connection.commit()
    cur.close()
    return jsonify({'mensaje': 'Proveedor actualizado correctamente'}), 200
Que pueda eliminar desde API - DELETE
# 4. DELETE — Eliminar proveedor (DELETE)
@app.route('/proveedores/<int:id>', methods=['DELETE'])
def eliminar_proveedor(id):
    cur = mysql.connection.cursor()
    cur.execute("DELETE FROM proveedores WHERE id = %s", (id,))
    mysql.connection.commit()
    cur.close()
    return jsonify({'mensaje': 'Proveedor eliminado correctamente'}), 200
if __name__ == '__main__':
    app.run(debug=True)
II PARTE implementar los conceptos explicados sobre Postman  Con base de datos de proveedores usada en clase.
POST en Postman

GET en Postman (Se Introdujeron 10 Registros de Proveedores)

GET en Postman de 1 sólo Proveedor

PUT en Postman (Actualizar Registro 5)


DELETE en Postman (Eliminar Proveedor 5)


