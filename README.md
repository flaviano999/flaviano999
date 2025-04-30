## Hi there 👋

<!--
**flaviano999/flaviano999** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
omundopro/
├── app.py
├── templates/
│   ├── index.html
│   ├── convites.html
│   └── registrar_tempo.html
└── static/
    └── style.css
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

# Lista de profissionais
profissionais = [
    {"id": 1, "nome": "Maria Silva", "profissao": "Engenheira Civil", "pais": "Brasil"},
    {"id": 2, "nome": "John Smith", "profissao": "Desenvolvedor Web", "pais": "Estados Unidos"},
    {"id": 3, "nome": "Aiko Tanaka", "profissao": "Designer Gráfico", "pais": "Japão"},
]

# Lista de convites
convites = []  # cada item: {"prof_id": 1, "obra": "Construção Escola", "aceito": False}

# Registro de tempo trabalhado
tempos = []  # cada item: {"prof_id": 1, "obra": "Construção Escola", "horas": 5}

@app.route("/")
def index():
    return render_template("index.html", profissionais=profissionais)

@app.route("/convidar", methods=["GET", "POST"])
def convidar():
    if request.method == "POST":
        prof_id = int(request.form["prof_id"])
        obra = request.form["obra"]
        convites.append({"prof_id": prof_id, "obra": obra, "aceito": False})
        return redirect(url_for("convidar"))
    return render_template("convites.html", profissionais=profissionais, convites=convites)

@app.route("/aceitar_convite/<int:index>")
def aceitar_convite(index):
    if 0 <= index < len(convites):
        convites[index]["aceito"] = True
    return redirect(url_for("convidar"))

@app.route("/tempo", methods=["GET", "POST"])
def tempo():
    if request.method == "POST":
        prof_id = int(request.form["prof_id"])
        obra = request.form["obra"]
        horas = float(request.form["horas"])
        tempos.append({"prof_id": prof_id, "obra": obra, "horas": horas})
        return redirect(url_for("tempo"))
    return render_template("registrar_tempo.html", profissionais=profissionais, tempos=tempos)

if __name__ == "__main__":
    app.run(debug=True)
<h2>Convidar Profissional para Obra</h2>
<form method="post">
    <label>Profissional:</label>
    <select name="prof_id">
        {% for p in profissionais %}
        <option value="{{ p.id }}">{{ p.nome }}</option>
        {% endfor %}
    </select><br><br>
    <label>Obra:</label>
    <input type="text" name="obra"><br><br>
    <button type="submit">Convidar</button>
</form>

<h3>Convites Enviados</h3>
<ul>
    {% for c in convites %}
    <li>
        {{ profissionais[c.prof_id - 1].nome }} - {{ c.obra }} -
        {% if c.aceito %}
            <strong>✅ Aceito</strong>
        {% else %}
            <a href="{{ url_for('aceitar_convite', index=loop.index0) }}">Aceitar</a>
        {% endif %}
    </li>
    {% endfor %}
</ul>
<a href="/">Voltar</a>
<h2>Registrar Tempo Trabalhado</h2>
<form method="post">
    <label>Profissional:</label>
    <select name="prof_id">
        {% for p in profissionais %}
        <option value="{{ p.id }}">{{ p.nome }}</option>
        {% endfor %}
    </select><br><br>
    <label>Obra:</label>
    <input type="text" name="obra"><br><br>
    <label>Horas Trabalhadas:</label>
    <input type="number" step="0.1" name="horas"><br><br>
    <button type="submit">Registrar</button>
</form>

<h3>Histórico de Horas Trabalhadas</h3>
<ul>
    {% for t in tempos %}
    <li>{{ profissionais[t.prof_id - 1].nome }} - {{ t.obra }}: {{ t.horas }} horas</li>
    {% endfor %}
</ul>
<a href="/">Voltar</a>
<h1>🌍 O Mundo Pro – Lista Global de Profissionais</h1>
<ul>
    {% for prof in profissionais %}
    <li>{{ prof.nome }} – {{ prof.profissao }} ({{ prof.pais }})</li>
    {% endfor %}
</ul>

<a href="/convidar">Convidar para obra</a> | 
<a href="/tempo">Registrar tempo trabalhado</a>
