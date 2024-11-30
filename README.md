Zadanie - kurs JS gr. III
<!DOCTYPE html>
<!-- Nie działa local storage -->
<head>
  <meta charset="UTF-8">
  <style>

    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      height: 100vh;
      margin: 20px;
      background-color: lightgrey;
    }

    .dodawanie-zadań {
      margin-top: 20px;
      text-align: center;
      width: 40%;
      margin-left: 0;
      margin-right: auto;
    }

    .tablica-kanban {
      display: flex;
      justify-content: center;
      width: 90%;
      max-width: 1200px;
      margin-top: 50px;
      margin-right: 100px;
    }

    .kolumna {
      background-color: white;
      width: 30%;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      padding: 10px;
      margin: 0 10px;
      min-height: 300px;
      display: flex;
      flex-direction: column;
    }

    .kolumna h2 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 1.5em;
    }

    .zadanie {
      background-color: lightsteelblue;
      margin: 10px 0;
      padding: 10px;
      border-radius: 4px;
      cursor: pointer;
      user-select: none;
    }

    input[type="text"] {
      padding: 10px;
      width: 60%;
      margin-right: 10px;
      border-radius: 4px;
      border: 1px solid #ccc;
    }

    button {
      padding: 10px 20px;
      background-color: white;
      color: black;
      border: solid black;
      border-radius: 2px;
    }

    button:hover {
      background-color: lightgrey;
    }
  </style>

</head>

<body>
  <div class="dodawanie-zadań">
    <h1>Zadania</h1>
    <input type="text" id="nowe-zadanie" placeholder="Dodaj nowe zadanie...">
    <button id="dodaj-zadanie">Dodaj zadanie</button>
  </div>

  <div class="tablica-kanban">
    <div class="kolumna" id="do-zrobienia" ondrop="drop(event)" ondragover="dragOver(event)">
      <h2>Do zrobienia</h2>
    </div>

    <div class="kolumna" id="w-trakcie" ondrop="drop(event)" ondragover="dragOver(event)">
      <h2>W trakcie</h2>
    </div>

    <div class="kolumna" id="zrobione" ondrop="drop(event)" ondragover="dragOver(event)">
      <h2>Zrobione</h2>
    </div>
  </div>

  <script>
    const inputField = document.getElementById('nowe-zadanie');
    const addButton = document.getElementById('dodaj-zadanie');
    const doZrobieniaColumn = document.getElementById('do-zrobienia');
    const wTrakcieColumn = document.getElementById('w-trakcie');
    const zrobioneColumn = document.getElementById('zrobione');

    // let tasks = JSON.parse(localStorage.getItem('tasks')) || {
    // 'do-zrobienia': [],
    // 'w-trakcie': [],
    // 'zrobione': []
    // };

    function createTask(taskText, column, id) {
      const task = document.createElement('div');
      task.classList.add('zadanie');
      task.id = id;
      task.textContent = taskText;
      task.setAttribute('draggable', true);


      task.addEventListener('dragstart', (e) => {
        e.dataTransfer.setData('text', taskText);
        e.dataTransfer.setData('id', id);
      });


      task.addEventListener('dragend', () => {
      });

      column.appendChild(task);
      return task;
    }
      
    // function updateLocalStorage() {
    // localStorage.setItem('tasks', JSON.stringify(tasks));
    // } 

      addButton.addEventListener('click', () => {
      const taskText = inputField.value.trim();
      if (taskText !== "") {
        const taskId = crypto.randomUUID();
        createTask(taskText, doZrobieniaColumn, taskId);
        inputField.value = "";
        // updateLocalStorage();
      }
    });

    function dragOver(event) {
      event.preventDefault();
    }

    function drop(event) {
      event.preventDefault();


      const taskText = event.dataTransfer.getData('text');
      const taskId = event.dataTransfer.getData('id');
      const targetColumn = event.target;

      if (targetColumn.classList.contains('kolumna')) {
        const taskToRemove = document.getElementById(taskId);
        if (taskToRemove) {
          taskToRemove.remove();
        }
    // updateLocalStorage();

        createTask(taskText, targetColumn, taskId);
       }
    }
    // updateLocalStorage();
    
    // function displayLocalStorage(){
    //   const tasks = JSON.parse(localStorage.getItem('tasks'));
    // }

    // displayLocalStorage();

  </script>
</body>

</html>
