<!-- templates/registo.html -->
<h2>Registo de Utilizador</h2>
<form method="POST">
  <label>Nome de utilizador:</label><br>
  <input type="text" name="username" required><br><br>

  <label>Telefone:</label><br>
  <input type="text" name="phone" required><br><br>

  <label>Palavra-passe:</label><br>
  <input type="password" name="password" required><br><br>

  <button type="submit">Registar</button>
</form>from werkzeug.security import check_password_hash

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form["username"]
        password = request.form["password"]
        user = users.query.filter_by(username=username).first()

        if user and check_password_hash(user.password, password):
            session["user"] = username
            return redirect(url_for("dashboard"))
        return "Credenciais inválidas."

    return render_template("login.html")<!-- templates/login.html -->
<h2>Login</h2>
<form method="POST">
  <label>Nome de utilizador:</label><br>
  <input type="text" name="username" required><br><br>

  <label>Palavra-passe:</label><br>
  <input type="password" name="password" required><br><br>

  <button type="submit">Entrar</button>
</form># Still-going-on
Alone
from werkzeug.security import generate_password_hash

@app.route("/registo", methods=["GET", "POST"])
def registo():
    if request.method == "POST":
        username = request.form["username"]
        phone = request.form["phone"]
        password = generate_password_hash(request.form["password"], method='sha256')

        if users.query.filter_by(username=username).first():
            return "Nome de utilizador já existe."

        novo_user = users(username=username, phone=phone, password=password)
        db.session.add(novo_user)
        db.session.commit()
        return redirect(url_for("login"))

    return render_template("registo.html")@app.route("/dashboard")
def dashboard():
    if "user" not in session:
        return redirect(url_for("login"))
    return f"<h2>Bem-vindo, {session['user']}!</h2><a href='/logout'>Sair</a>"@app.route("/logout")
def logout():
    session.pop("user", None)
    return redirect(url_for("login"))