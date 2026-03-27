
index.html
Html

Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>📝 To-Do List</h1>
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="Enter a new task">
            <button id="addTaskBtn">Add</button>
        </div>
        <ul id="taskList"></ul>
    </div>
    <script src="script.js"></script>
</body>
</html>
style.css
Css

Copy code
body {
    font-family: Arial, sans-serif;
    background: #f4f4f4;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.container {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
    width: 300px;
}

h1 {
    text-align: center;
}

.input-section {
    display: flex;
    margin-bottom: 10px;
}

#taskInput {
    flex: 1;
    padding: 8px;
}

#addTaskBtn {
    padding: 8px 12px;
    background: #28a745;
    color: white;
    border: none;
    cursor: pointer;
}

#addTaskBtn:hover {
    background: #218838;
}

ul {
    list-style: none;
    padding: 0;
}

li {
    padding: 8px;
    background: #f9f9f9;
    margin-bottom: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

li.completed {
    text-decoration: line-through;
    color: gray;
}

button.delete {
    background: red;
    color: white;
    border: none;
    padding: 4px 8px;
    cursor: pointer;
}
script.js
Javascript

Copy code
// Select elements
const taskInput = document.getElementById('taskInput');
const addTaskBtn = document.getElementById('addTaskBtn');
const taskList = document.getElementById('taskList');

// Load tasks from localStorage
document.addEventListener('DOMContentLoaded', loadTasks);

// Add task
addTaskBtn.addEventListener('click', () => {
    const taskText = taskInput.value.trim();
    if (taskText === '') {
        alert('Please enter a task.');
        return;
    }
    addTask(taskText);
    saveTask(taskText);
    taskInput.value = '';
});

// Add task to UI
function addTask(taskText, completed = false) {
    const li = document.createElement('li');
    li.textContent = taskText;
    if (completed) li.classList.add('completed');

    // Toggle complete
    li.addEventListener('click', () => {
        li.classList.toggle('completed');
        updateStorage();
    });

    // Delete button
    const delBtn = document.createElement('button');
    delBtn.textContent = 'X';
    delBtn.classList.add('delete');
    delBtn.addEventListener('click', (e) => {
        e.stopPropagation();
        li.remove();
        updateStorage();
    });

    li.appendChild(delBtn);
    taskList.appendChild(li);
}

// Save task to localStorage
function saveTask(taskText) {
    const tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    tasks.push({ text: taskText, completed: false });
    localStorage.setItem('tasks', JSON.stringify(tasks));
}

// Load tasks from localStorage
function loadTasks() {
    const tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    tasks.forEach(task => addTask(task.text, task.completed));
}

// Update localStorage after changes
function updateStorage() {
    const tasks = [];
    document.querySelectorAll('#taskList li').forEach(li => {
        tasks.push({
            text: li.firstChild.textContent,
            completed: li.classList.contains('completed')
        });
    });
    localStorage.setItem('tasks', JSON.stringify(tasks));
}

