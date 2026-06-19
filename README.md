# mini-project-2
Notes App with Trash Bin
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Notes App with Trash Bin</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,sans-serif;
}

body{
    background:#f4f4f4;
    padding:20px;
}

.container{
    max-width:900px;
    margin:auto;
}

h1{
    text-align:center;
    margin-bottom:20px;
    color:#333;
}

.input-box{
    display:flex;
    gap:10px;
    margin-bottom:20px;
}

textarea{
    flex:1;
    padding:10px;
    resize:none;
    height:80px;
    border:1px solid #ccc;
    border-radius:8px;
}

button{
    padding:10px 15px;
    border:none;
    border-radius:8px;
    cursor:pointer;
    color:white;
}

.add-btn{
    background:#28a745;
}

.delete-btn{
    background:#dc3545;
}

.restore-btn{
    background:#007bff;
}

.clear-btn{
    background:#6c757d;
}

.sections{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:20px;
}

.card{
    background:white;
    padding:15px;
    border-radius:10px;
    box-shadow:0 2px 6px rgba(0,0,0,0.1);
}

.note{
    background:#fff8c4;
    padding:10px;
    border-radius:8px;
    margin-bottom:10px;
}

.note p{
    margin-bottom:10px;
    word-wrap:break-word;
}

.actions{
    display:flex;
    gap:8px;
}

.empty{
    color:#777;
    text-align:center;
    padding:10px;
}
</style>
</head>
<body>

<div class="container">
    <h1>📝 Notes App with Trash Bin</h1>

    <div class="input-box">
        <textarea id="noteInput" placeholder="Write your note..."></textarea>
        <button class="add-btn" onclick="addNote()">Add Note</button>
    </div>

    <div class="sections">

        <div class="card">
            <h2>📒 Notes</h2>
            <br>
            <div id="notesList"></div>
        </div>

        <div class="card">
            <h2>🗑️ Trash Bin</h2>
            <br>
            <div id="trashList"></div>
        </div>

    </div>
</div>

<script>
let notes = JSON.parse(localStorage.getItem("notes")) || [];
let trash = JSON.parse(localStorage.getItem("trash")) || [];

function saveData(){
    localStorage.setItem("notes", JSON.stringify(notes));
    localStorage.setItem("trash", JSON.stringify(trash));
}

function addNote(){
    const input = document.getElementById("noteInput");
    const text = input.value.trim();

    if(text === ""){
        alert("Please write a note.");
        return;
    }

    notes.push(text);
    input.value = "";

    saveData();
    render();
}

function deleteNote(index){
    trash.push(notes[index]);
    notes.splice(index,1);

    saveData();
    render();
}

function restoreNote(index){
    notes.push(trash[index]);
    trash.splice(index,1);

    saveData();
    render();
}

function permanentlyDelete(index){
    trash.splice(index,1);

    saveData();
    render();
}

function render(){
    const notesList = document.getElementById("notesList");
    const trashList = document.getElementById("trashList");

    notesList.innerHTML = "";
    trashList.innerHTML = "";

    if(notes.length === 0){
        notesList.innerHTML =
        '<div class="empty">No notes available</div>';
    }

    if(trash.length === 0){
        trashList.innerHTML =
        '<div class="empty">Trash is empty</div>';
    }

    notes.forEach((note,index)=>{
        notesList.innerHTML += `
        <div class="note">
            <p>${note}</p>
            <div class="actions">
                <button class="delete-btn"
                onclick="deleteNote(${index})">
                Move to Trash
                </button>
            </div>
        </div>`;
    });

    trash.forEach((note,index)=>{
        trashList.innerHTML += `
        <div class="note">
            <p>${note}</p>
            <div class="actions">
                <button class="restore-btn"
                onclick="restoreNote(${index})">
                Restore
                </button>

                <button class="delete-btn"
                onclick="permanentlyDelete(${index})">
                Delete Forever
                </button>
            </div>
        </div>`;
    });
}

render();
</script>

</body>
</html>
