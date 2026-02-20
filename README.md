# gestion-circonscription-
Logiciel de gestion d'une circonscription scolaire 
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Plateforme Académique PRO</title>

<style>
body{margin:0;font-family:Arial;background:#f4f6f9;}
header{background:#2c3e50;color:white;padding:15px;text-align:center;}
.sidebar{width:240px;background:#34495e;position:fixed;top:0;bottom:0;padding-top:60px;color:white;overflow:auto;}
.sidebar a{display:block;padding:12px;color:white;text-decoration:none;}
.sidebar a:hover{background:#1abc9c;}
.content{margin-left:240px;padding:20px;}
.card{background:white;padding:20px;margin-bottom:20px;border-radius:8px;box-shadow:0 3px 8px rgba(0,0,0,0.1);}
input, select, button{padding:8px;margin:5px 0;width:100%;}
button{background:linear-gradient(135deg,#2c3e50,#1abc9c);color:white;border:none;cursor:pointer;font-weight:bold;border-radius:6px;}
button:hover{opacity:0.9;}
table{width:100%;border-collapse:collapse;margin-top:10px;}
table, th, td{border:1px solid #ccc;}
th, td{padding:6px;text-align:left;font-size:14px;}
.stat-box{display:inline-block;width:30%;background:#1abc9c;color:white;padding:15px;margin:5px;text-align:center;border-radius:8px;}
</style>
</head>

<body>

<header>
<h2>Plateforme Académique Professionnelle</h2>
</header>

<div class="sidebar">
<a href="#" onclick="showSection('dashboard')">📊 Tableau de bord</a>
<a href="#" onclick="showSection('ecoles')">🏫 Écoles</a>
<a href="#" onclick="showSection('classes')">📘 Classes</a>
<a href="#" onclick="showSection('eleves')">👧 Élèves</a>
<a href="#" onclick="showSection('notes')">📝 Notes</a>
</div>

<div class="content">

<!-- DASHBOARD -->
<div id="dashboard" class="section">
<div class="card">
<h3>Vue Générale</h3>
<div class="stat-box"><h2 id="nbEcoles">0</h2>Écoles</div>
<div class="stat-box"><h2 id="nbClasses">0</h2>Classes</div>
<div class="stat-box"><h2 id="nbEleves">0</h2>Élèves</div>
</div>
</div>

<!-- ECOLES -->
<div id="ecoles" class="section" style="display:none;">
<div class="card">
<h3>Gestion des Écoles</h3>
<input type="text" id="nomEcole" placeholder="Nom de l'école">
<input type="text" id="localite" placeholder="Localité">
<button onclick="ajouterEcole()">Ajouter</button>

<table>
<thead>
<tr>
<th>Nom</th>
<th>Localité</th>
<th>Effectif Total</th>
<th>Filles</th>
<th>Garçons</th>
<th>Moyenne École</th>
</tr>
</thead>
<tbody id="tableEcoles"></tbody>
</table>
</div>
</div>

<!-- CLASSES -->
<div id="classes" class="section" style="display:none;">
<div class="card">
<h3>Gestion des Classes</h3>

<select id="ecoleClasse"></select>
<input type="text" id="nomClasse" placeholder="Nom classe (CP1, CE2...)">
<input type="text" id="nomMaitre" placeholder="Nom du maître">
<button onclick="ajouterClasse()">Ajouter</button>

<table>
<thead>
<tr>
<th>École</th>
<th>Classe</th>
<th>Maître</th>
<th>Effectif</th>
<th>Filles</th>
<th>Garçons</th>
<th>Moyenne Classe</th>
</tr>
</thead>
<tbody id="tableClasses"></tbody>
</table>
</div>
</div>

<!-- ELEVES -->
<div id="eleves" class="section" style="display:none;">
<div class="card">
<h3>Gestion des Élèves</h3>

<input type="text" id="nomEleve" placeholder="Nom élève">

<select id="genre">
<option value="F">Fille</option>
<option value="M">Garçon</option>
</select>

<select id="classeEleve"></select>

<button onclick="ajouterEleve()">Ajouter</button>

<table>
<thead>
<tr>
<th>Nom</th>
<th>École</th>
<th>Classe</th>
<th>Genre</th>
</tr>
</thead>
<tbody id="tableEleves"></tbody>
</table>
</div>
</div>

<!-- NOTES -->
<div id="notes" class="section" style="display:none;">
<div class="card">
<h3>Gestion des Notes (4 évaluations)</h3>

<select id="eleveNote"></select>

<input type="number" id="note1" placeholder="Note 1 /20">
<input type="number" id="note2" placeholder="Note 2 /20">
<input type="number" id="note3" placeholder="Note 3 /20">
<input type="number" id="note4" placeholder="Note 4 /20">

<button onclick="ajouterNotes()">Enregistrer</button>

<table>
<thead>
<tr>
<th>Élève</th>
<th>N1</th>
<th>N2</th>
<th>N3</th>
<th>N4</th>
<th>Moyenne</th>
</tr>
</thead>
<tbody id="tableNotes"></tbody>
</table>

</div>
</div>

</div>

<script>

let ecoles=[], classes=[], eleves=[], notes=[];

function showSection(id){
document.querySelectorAll('.section').forEach(sec=>sec.style.display="none");
document.getElementById(id).style.display="block";
}

/* ECOLES */

function ajouterEcole(){
let nom=document.getElementById("nomEcole").value;
let localite=document.getElementById("localite").value;
if(!nom||!localite) return alert("Remplir tous les champs");
ecoles.push({nom,localite,moyenneEcole:0});
document.getElementById("nomEcole").value="";
document.getElementById("localite").value="";
afficherEcoles();
}

/* CLASSES */

function ajouterClasse(){
let ecole=document.getElementById("ecoleClasse").value;
let nom=document.getElementById("nomClasse").value;
let maitre=document.getElementById("nomMaitre").value;
if(!ecole||!nom||!maitre) return alert("Remplir tous les champs");

classes.push({
id:Date.now(),
ecole,
nom,
maitre,
moyenneClasse:0
});

document.getElementById("nomClasse").value="";
document.getElementById("nomMaitre").value="";
afficherClasses();
}

/* ELEVES */

function ajouterEleve(){

let nom=document.getElementById("nomEleve").value;
let genre=document.getElementById("genre").value;
let classeNom=document.getElementById("classeEleve").value;

let classeObj=classes.find(c=>c.nom===classeNom);

if(!nom||!classeObj) return alert("Remplir tous les champs");

eleves.push({
nom,
genre,
classeId:classeObj.id,
classeNom:classeObj.nom,
ecole:classeObj.ecole
});

document.getElementById("nomEleve").value="";
afficherEleves();
afficherClasses();
afficherEcoles();
}

/* NOTES */

function ajouterNotes(){
let eleve=document.getElementById("eleveNote").value;
let n1=parseFloat(document.getElementById("note1").value)||0;
let n2=parseFloat(document.getElementById("note2").value)||0;
let n3=parseFloat(document.getElementById("note3").value)||0;
let n4=parseFloat(document.getElementById("note4").value)||0;

let moyenne=((n1+n2+n3+n4)/4).toFixed(2);

let exist=notes.find(n=>n.eleve===eleve);
if(exist){
exist.n1=n1;exist.n2=n2;exist.n3=n3;exist.n4=n4;exist.moyenne=moyenne;
}else{
notes.push({eleve,n1,n2,n3,n4,moyenne});
}

afficherNotes();
calculerMoyennes();
}

/* AFFICHAGES */

function afficherEcoles(){
let table="",select="";
ecoles.forEach(e=>{
let elevesEcole=eleves.filter(el=>el.ecole===e.nom);
let effectif=elevesEcole.length;
let filles=elevesEcole.filter(el=>el.genre==="F").length;
let garcons=elevesEcole.filter(el=>el.genre==="M").length;

table+=`<tr>
<td>${e.nom}</td>
<td>${e.localite}</td>
<td>${effectif}</td>
<td>${filles}</td>
<td>${garcons}</td>
<td>${e.moyenneEcole||0}</td>
</tr>`;

select+=`<option value="${e.nom}">${e.nom}</option>`;
});
document.getElementById("tableEcoles").innerHTML=table;
document.getElementById("ecoleClasse").innerHTML=select;
document.getElementById("nbEcoles").innerText=ecoles.length;
}

function afficherClasses(){
let table="",select="";
classes.forEach(c=>{
let elevesClasse=eleves.filter(e=>e.classeId===c.id);
let effectif=elevesClasse.length;
let filles=elevesClasse.filter(e=>e.genre==="F").length;
let garcons=elevesClasse.filter(e=>e.genre==="M").length;

table+=`<tr>
<td>${c.ecole}</td>
<td>${c.nom}</td>
<td>${c.maitre}</td>
<td>${effectif}</td>
<td>${filles}</td>
<td>${garcons}</td>
<td>${c.moyenneClasse||0}</td>
</tr>`;

select+=`<option value="${c.nom}">${c.nom}</option>`;
});
document.getElementById("tableClasses").innerHTML=table;
document.getElementById("classeEleve").innerHTML=select;
document.getElementById("nbClasses").innerText=classes.length;
}

function afficherEleves(){
let table="",select="";
eleves.forEach(e=>{
table+=`<tr>
<td>${e.nom}</td>
<td>${e.ecole}</td>
<td>${e.classeNom}</td>
<td>${e.genre==="F"?"Fille":"Garçon"}</td>
</tr>`;
select+=`<option value="${e.nom}">${e.nom}</option>`;
});
document.getElementById("tableEleves").innerHTML=table;
document.getElementById("eleveNote").innerHTML=select;
document.getElementById("nbEleves").innerText=eleves.length;
}

function afficherNotes(){
let table="";
notes.forEach(n=>{
table+=`<tr>
<td>${n.eleve}</td>
<td>${n.n1}</td>
<td>${n.n2}</td>
<td>${n.n3}</td>
<td>${n.n4}</td>
<td>${n.moyenne}</td>
</tr>`;
});
document.getElementById("tableNotes").innerHTML=table;
}

/* CALCUL MOYENNES */

function calculerMoyennes(){

classes.forEach(c=>{
let elevesClasse=eleves.filter(e=>e.classeId===c.id);
let moyennes=[];
elevesClasse.forEach(e=>{
let note=notes.find(n=>n.eleve===e.nom);
if(note) moyennes.push(parseFloat(note.moyenne));
});
c.moyenneClasse=moyennes.length>0?
(moyennes.reduce((a,b)=>a+b,0)/moyennes.length).toFixed(2):0;
});

ecoles.forEach(e=>{
let classesEcole=classes.filter(c=>c.ecole===e.nom);
let moyennes=[];
classesEcole.forEach(c=>{
if(c.moyenneClasse>0) moyennes.push(parseFloat(c.moyenneClasse));
});
e.moyenneEcole=moyennes.length>0?
(moyennes.reduce((a,b)=>a+b,0)/moyennes.length).toFixed(2):0;
});

afficherClasses();
afficherEcoles();
}

</script>

</body>
</html>
