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
    return redirect(url_for("login"))# 🧠 Still-going-on

Projeto web desenvolvido com Flask, SQLAlchemy e visualizações de dados para análise da pandemia COVID-19.

## 🚀 Funcionalidades

- Registo e login de utilizadores
- Visualização de mapas de calor por região
- Leitura de QR codes
- Dashboard com dados de casos e mortes

## 🛠️ Tecnologias Usadas

- Python
- Flask
- SQLAlchemy
- OpenCV (`cv2`)
- Pyzbar
- MySQL

## 📦 Instalação

```bash
git clone https://github.com/isthenotalone/Still-going-on.git
cd Still-going-on
pip install -r requirements.txt
python main.py# 🧠 Still-going-on

Aplicação web desenvolvida com **Flask**, **SQLAlchemy** e visualizações interativas para análise da pandemia COVID-19. Criado por [Isthenotalone](https://github.com/isthenotalone), este projeto combina dados, mapas de calor, autenticação de utilizadores e leitura de QR codes.

---

## 🚀 Funcionalidades

- 🔐 Registo e login de utilizadores com segurança (hash de palavra-passe)
- 📊 Visualização de dados por região (casos, mortes, previsões)
- 🌍 Mapas de calor gerados com `plotregions`
- 📱 Leitura de QR codes com `pyzbar` e `cv2`
- 🧠 Dashboard personalizado para cada utilizador

---

## 🛠️ Tecnologias Usadas

- **Python 3**
- **Flask**
- **SQLAlchemy**
- **OpenCV** (`cv2`)
- **Pyzbar**
- **MySQL**
- **Werkzeug Security**

---

## 📦 Instalação

1. Clona o repositório:
```bash
git clone https://github.com/isthenotalone/Still-going-on.git
cd Still-going-on
pip install -r requirements.txt
python main.py ---

