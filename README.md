<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Task List with Edit/Delete and localStorage</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }
    input, button {
      padding: 10px;
      font-size: 16px;
    }
    #taskInput {
      width: 300px;
      margin-right: 10px;
    }
    ul {
      list-style: none;
      padding-left: 0;
      margin-top: 20px;
      width: 400px;
    }
    li {
      background: #f0f0f0;
      margin-bottom: 10px;
      padding: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-radius: 5px;
    }
    .task-text {
      flex-grow: 1;
      margin-right: 10px;
    }
    button.edit, button.delete {
      margin-left: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h2>Task List</h2>

  <input type="text" id="taskInput" placeholder="Enter a task" />
  <button id="addTaskBtn">Add Task</button>

  <ul id="taskList"></ul>

  <script>
    const taskInput = document.getElementById('taskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const taskList = document.getElementById('taskList');

    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

    function renderTasks() {
      taskList.innerHTML = '';
      tasks.forEach((task, index) => {
        const li = document.createElement('li');
        const taskSpan = document.createElement('span');
        taskSpan.textContent = task;
        taskSpan.className = 'task-text';
        li.appendChild(taskSpan);
        const editBtn = document.createElement('button');
        editBtn.textContent = 'Edit';
        editBtn.className = 'edit';
        editBtn.onclick = () => editTask(index);
        li.appendChild(editBtn);
        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'Delete';
        deleteBtn.className = 'delete';
        deleteBtn.onclick = () => deleteTask(index);
        li.appendChild(deleteBtn);
        taskList.appendChild(li);
      });
    }

    function addTask() {
      const task = taskInput.value.trim();
      if (task) {
        tasks.push(task);
        saveAndRender();
        taskInput.value = '';
        taskInput.focus();
      } else {
        alert('Please enter a task.');
      }
    }

    function editTask(index) {
      const newTask = prompt('Edit task:', tasks[index]);
      if (newTask !== null) {
        const trimmedTask = newTask.trim();
        if (trimmedTask.length > 0) {
          tasks[index] = trimmedTask;
          saveAndRender();
        } else {
          alert('Task cannot be empty.');
        }
      }
    }

    function deleteTask(index) {
      tasks.splice(index, 1);
      saveAndRender();
    }

    function saveAndRender() {
      localStorage.setItem('tasks', JSON.stringify(tasks));
      renderTasks();
    }

    addTaskBtn.addEventListener('click', addTask);

    taskInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        addTask();
      }
    });

    renderTasks();
  </script>
</body>
</html>
