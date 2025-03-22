<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Kb's To-Do-List</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #2e2e2e;
            margin: 0;
            padding: 0;
            background-size: cover;
            background-position: center;
            color: #DEB887;
        }

        .container {
            width: 300px;
            margin: 50px auto;
            padding: 20px;
            background: #fff;
            color: #000;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
        }

        #todo-input {
            width: 70%;
            padding: 10px;
        }

        #add-todo {
            padding: 10px;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            display: flex;
            justify-content: space-between;
        }

        .delete-btn {
            background-color: red;
            color: white;
            border: none;
            padding: 5px;
            cursor: pointer;
            border-radius: 3px;
        }

        .options {
            position: absolute;
            bottom: 20px;
            right: 20px;
        }

        button {
            cursor: pointer;
        }

        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.4);
        }

        .modal-content {
            background-color: #fefefe;
            margin: 15% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 300px;
        }

        .close {
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }

        .close:hover {
            color: red;
        }
    </style>
</head>
<body>
    <!-- Sound Effects -->
    <audio id="add-sound" src="https://www.soundjay.com/button/sounds/button-29.mp3"></audio>
    <audio id="delete-sound" src="https://www.soundjay.com/button/sounds/button-11.mp3"></audio>
    <audio id="self-destruct-sound" src="https://www.soundjay.com/button/sounds/beep-07.mp3"></audio>

    <div class="container">
        <h1>Kb's To-Do-List</h1>
        <div>
            <input type="text" id="todo-input" placeholder="Add a new task..." />
            <button id="add-todo">Add</button>
            <ul id="tasks"></ul>
        </div>
        <div class="options">
            <button id="settings-btn">Settings</button>
        </div>
    </div>

    <!-- Settings Modal -->
    <div id="settings-modal" class="modal">
        <div class="modal-content">
            <span class="close" id="close-settings">&times;</span>
            <h2>Settings</h2>
            <input type="text" id="username" placeholder="Enter your username" />
            <button id="create-account">Create Account</button>
            <hr />
            <button id="admin-login-btn">Admin Login</button>
        </div>
    </div>

    <!-- Admin Login Modal -->
    <div id="admin-login-modal" class="modal">
        <div class="modal-content">
            <span class="close" id="close-admin">&times;</span>
            <h2>Admin Login</h2>
            <input type="text" id="admin-username" placeholder="Admin Username" />
            <input type="password" id="admin-password" placeholder="Admin Password" />
            <button id="admin-login">Login</button>
        </div>
    </div>

    <!-- Admin Panel Modal -->
    <div id="admin-panel-modal" class="modal">
        <div class="modal-content">
            <span class="close" id="close-admin-panel">&times;</span>
            <h2>Admin Panel</h2>
            <ul id="user-list"></ul>
        </div>
    </div>

    <script>
        // Open settings modal
        document.getElementById('settings-btn').addEventListener('click', () => {
            document.getElementById('settings-modal').style.display = 'block';
        });

        // Close modals
        document.querySelectorAll('.close').forEach((btn) => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.modal').forEach((modal) => {
                    modal.style.display = 'none';
                });
            });
        });

        // Create account
        document.getElementById('create-account').addEventListener('click', () => {
            const username = document.getElementById('username').value.trim();
            if (username) {
                let users = JSON.parse(localStorage.getItem('users')) || [];
                if (!users.includes(username)) {
                    users.push(username);
                    localStorage.setItem('users', JSON.stringify(users));
                    alert(`Account created for ${username}!`);
                } else {
                    alert('Username already exists!');
                }
            } else {
                alert('Please enter a username.');
            }
        });

        // Add task + sound
        document.getElementById('add-todo').addEventListener('click', () => {
            const input = document.getElementById('todo-input');
            const task = input.value.trim();

            if (task) {
                const li = document.createElement('li');
                li.innerHTML = `${task} <button class="delete-btn">Delete</button>`;
                document.getElementById('tasks').appendChild(li);
                input.value = '';

                // Play add sound
                document.getElementById('add-sound').play();

                // Delete task + sound
                li.querySelector('.delete-btn').addEventListener('click', () => {
                    li.remove();
                    document.getElementById('delete-sound').play();
                });
            }
        });

        // Admin login
        document.getElementById('admin-login-btn').addEventListener('click', () => {
            document.getElementById('admin-login-modal').style.display = 'block';
        });

        document.getElementById('admin-login').addEventListener('click', () => {
            const adminUsername = document.getElementById('admin-username').value;
            const adminPassword = document.getElementById('admin-password').value;

            if (adminUsername === 'DarkFirewall_007' && adminPassword === '********') {
                openAdminPanel();
            } else if (adminUsername === 'hey' && adminPassword === 'suiii') {
                startSelfDestruct();
            } else {
                alert('Invalid Admin Credentials! Self-Destruct sequence will be initiated');
            }
        });

        function openAdminPanel() {
            const userList = document.getElementById('user-list');
            userList.innerHTML = '';
            const users = JSON.parse(localStorage.getItem('users')) || [];
            users.forEach((user) => {
                const li = document.createElement('li');
                li.textContent = user;
                userList.appendChild(li);
            });
            document.getElementById('admin-panel-modal').style.display = 'block';
        }

        // Self-destruct + sound
        function startSelfDestruct() {
            let countdown = 5;
            const interval = setInterval(() => {
                if (countdown > 0) {
                    alert(`💣 Self-destruct in ${countdown}...`);
                    countdown--;
                    document.getElementById('self-destruct-sound').play();
                } else {
                    clearInterval(interval);
                    alert('🔥 Self-destruct initiated! All data wiped!');
                    localStorage.clear();
                    location.reload();
                }
            }, 1000);
        }
    </script>
</body>
</html>

<!---
WhoAmI1426/WhoAmI1426 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
