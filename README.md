# Still-going-on
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

    return render_template("registo.html")