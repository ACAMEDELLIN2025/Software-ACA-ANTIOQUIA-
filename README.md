from flask import Flask, render_template, request, redirect, url_for, session import sqlite3 from werkzeug.security import generate_password_hash, check_password_hash

app = Flask(name) app.secret_key = 'clave_secreta'

Función para conectar con la base de datos

def get_db_connection(): conn = sqlite3.connect('base_de_datos.db') conn.row_factory = sqlite3.Row return conn

@app.route('/') def index(): if 'admin' in session: return redirect(url_for('panel_admin')) return redirect(url_for('login'))

@app.route('/login', methods=['GET', 'POST']) def login(): if request.method == 'POST': usuario = request.form['usuario'] contrasena = request.form['contrasena'] conn = get_db_connection() admin = conn.execute('SELECT * FROM administradores WHERE usuario = ?', (usuario,)).fetchone() conn.close() if admin and check_password_hash(admin['contrasena'], contrasena): session['admin'] = usuario return redirect(url_for('panel_admin')) return 'Usuario o contraseña incorrectos' return render_template('login.html')

@app.route('/admin') def panel_admin(): if 'admin' not in session: return redirect(url_for('login')) return render_template('admin.html')

@app.route('/logout') def logout(): session.pop('admin', None) return redirect(url_for('login'))

if name == 'main': app.run(debug=True)
from flask import Flask, render_template, request, redirect, url_for, session import sqlite3 from werkzeug.security import generate_password_hash, check_password_hash

app = Flask(name) app.secret_key = 'clave_secreta'

Función para conectar con la base de datos

def get_db_connection(): conn = sqlite3.connect('base_de_datos.db') conn.row_factory = sqlite3.Row return conn

@app.route('/') def index(): if 'admin' in session: return redirect(url_for('panel_admin')) return redirect(url_for('login'))

@app.route('/login', methods=['GET', 'POST']) def login(): if request.method == 'POST': usuario = request.form['usuario'] contrasena = request.form['contrasena'] conn = get_db_connection() admin = conn.execute('SELECT * FROM administradores WHERE usuario = ?', (usuario,)).fetchone() conn.close() if admin and check_password_hash(admin['contrasena'], contrasena): session['admin'] = usuario return redirect(url_for('panel_admin')) return 'Usuario o contraseña incorrectos' return render_template('login.html')

@app.route('/admin') def panel_admin(): if 'admin' not in session: return redirect(url_for('login')) return render_template('admin.html')

@app.route('/logout') def logout(): session.pop('admin', None) return redirect(url_for('login'))

if name == 'main': app.run(debug=True)

login.html

login_html = """

<!DOCTYPE html><html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Login Administrador</title>
</head>
<body>
    <h1>Iniciar sesión</h1>
    <form method="POST">
        <input type="text" name="usuario" placeholder="Usuario" required><br>
        <input type="password" name="contrasena" placeholder="Contraseña" required><br>
        <button type="submit">Entrar</button>
    </form>
</body>
</html>
"""admin.html

admin_html = """

<!DOCTYPE html><html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Panel Administrador</title>
</head>
<body>
    <h1>Bienvenido al Panel de Administración</h1>
    <a href="{{ url_for('logout') }}">Cerrar sesión</a>
</body>
</html>
"""requirements.txt

requirements_txt = """ flask werkzeug """



