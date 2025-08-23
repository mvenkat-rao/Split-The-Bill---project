<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Split My Bill - A Complete Project</title>

    <!-- 
    ================================================================
    CSS STYLES
    This section contains all the styling for our single-page app.
    It's designed to make the web page look and feel like a native application.
    We'll style different "views" or "pages" that we will show/hide with JavaScript.
    This corresponds to the "Tailwind CSS / Chakra UI" part of your plan, 
    but implemented with vanilla CSS.
    ================================================================
    -->
    <style>
        /* --- General & Reset Styles --- */
        :root {
            --primary-color: #4a90e2;
            --secondary-color: #50e3c2;
            --background-color: #f4f7f6;
            --surface-color: #ffffff;
            --text-color: #333;
            --text-light-color: #888;
            --error-color: #d0021b;
            --success-color: #7ed321;
            --border-color: #e0e0e0;
            --shadow: 0 4px 12px rgba(0,0,0,0.08);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--background-color);
            color: var(--text-color);
            font-size: 16px;
        }

        /* --- Main App Container --- */
        #app-container {
            max-width: 800px;
            margin: 0 auto;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        /* --- Header Styles --- */
        header {
            background-color: var(--primary-color);
            color: white;
            padding: 1rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        header h1 {
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        #auth-section {
            font-size: 0.9rem;
        }

        #auth-section button {
            background: none;
            border: 1px solid white;
            color: white;
            padding: 0.5rem 1rem;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        #auth-section button:hover {
            background-color: rgba(255,255,255,0.2);
        }
        
        #user-info {
            display: flex;
            align-items: center;
            gap: 1rem;
        }
        
        #user-info button {
            background: var(--error-color);
            border: none;
        }
        
        /* --- Main Content & View Switching --- */
        main {
            flex-grow: 1;
            padding: 1.5rem;
        }

        .view {
            display: none; /* All views are hidden by default */
        }
        
        .view.active {
            display: block; /* The active view is shown */
            animation: fadeIn 0.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* --- Reusable Components (Buttons, Forms, Cards) --- */
        .card {
            background-color: var(--surface-color);
            border-radius: 8px;
            box-shadow: var(--shadow);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }

        h2 {
            font-size: 1.75rem;
            margin-bottom: 1rem;
            border-bottom: 2px solid var(--primary-color);
            padding-bottom: 0.5rem;
            color: var(--primary-color);
        }
        
        h3 {
            font-size: 1.25rem;
            margin-bottom: 1rem;
            color: #555;
        }

        button {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 0.75rem 1.5rem;
            border-radius: 5px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.2s, transform 0.1s;
        }
        
        button:hover {
            background-color: #3a80d2;
        }
        
        button:active {
            transform: scale(0.98);
        }
        
        button.secondary {
            background-color: #ccc;
            color: #333;
        }
        
        button.secondary:hover {
            background-color: #bbb;
        }
        
        button.success {
            background-color: var(--success-color);
        }
        button.success:hover {
            background-color: #6ec31a;
        }
        
        button.danger {
            background-color: var(--error-color);
        }
        button.danger:hover {
            background-color: #b00116;
        }

        form {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        input[type="text"],
        input[type="email"],
        input[type="password"],
        input[type="number"] {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid var(--border-color);
            border-radius: 5px;
            font-size: 1rem;
        }
        
        .form-message {
            padding: 1rem;
            border-radius: 5px;
            margin-top: 1rem;
            text-align: center;
        }
        
        .form-message.error {
            background-color: #fbeae9;
            color: var(--error-color);
            border: 1px solid var(--error-color);
        }
        
        .form-message.success {
            background-color: #f1fbe9;
            color: #5a7f20;
            border: 1px solid var(--success-color);
        }
        
        /* --- Specific View Styles --- */
        
        /* Auth View */
        #auth-view .tabs {
            display: flex;
            margin-bottom: 1.5rem;
            border-bottom: 1px solid var(--border-color);
        }
        
        #auth-view .tab {
            padding: 1rem 1.5rem;
            cursor: pointer;
            font-weight: bold;
            color: var(--text-light-color);
            border-bottom: 3px solid transparent;
        }
        
        #auth-view .tab.active {
            color: var(--primary-color);
            border-bottom-color: var(--primary-color);
        }
        
        #signup-form { display: none; }

        /* Dashboard View */
        #groups-list, #balances-summary-list {
            list-style: none;
            padding: 0;
        }
        
        .list-item {
            background: var(--surface-color);
            padding: 1rem 1.5rem;
            margin-bottom: 0.75rem;
            border-radius: 5px;
            border: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
            cursor: pointer;
            transition: box-shadow 0.2s, transform 0.2s;
        }
        
        .list-item:hover {
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            transform: translateY(-2px);
        }
        
        .balance-owed { color: var(--error-color); font-weight: bold; }
        .balance-due { color: var(--success-color); font-weight: bold; }

        /* Group View */
        #group-view-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }
        
        #expenses-list { list-style: none; }
        #expenses-list .expense-item {
            padding: 1rem;
            border-bottom: 1px solid var(--border-color);
        }
        #expenses-list .expense-item:last-child {
            border-bottom: none;
        }
        .expense-details {
            display: flex;
            justify-content: space-between;
        }
        .expense-description { font-weight: bold; }
        .expense-paid-by { font-size: 0.9rem; color: var(--text-light-color); }
        
        #balances-list .balance-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0.75rem 0;
        }
        
        #split-options label {
            display: block;
            margin-bottom: 0.5rem;
        }
        
        /* QR Code & Menu View */
        #qr-scanner-view .scanner-box {
            width: 300px;
            height: 300px;
            border: 5px solid var(--primary-color);
            margin: 2rem auto;
            position: relative;
            background-color: #e0e0e0;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            text-align: center;
        }
        #qr-scanner-view .scanner-box p {
            color: #666;
        }
        
        #menu-items { list-style: none; }
        #menu-items li {
            display: flex;
            align-items: center;
            padding: 1rem;
            border-bottom: 1px solid var(--border-color);
        }
        #menu-items input[type="checkbox"] {
            width: auto;
            margin-right: 1rem;
        }
        #menu-items .item-price {
            margin-left: auto;
            font-weight: bold;
        }
        
        /* Footer */
        footer {
            text-align: center;
            padding: 1.5rem;
            color: var(--text-light-color);
            font-size: 0.9rem;
            margin-top: 2rem;
            border-top: 1px solid var(--border-color);
        }
        
        /* Utility Classes */
        .hidden {
            display: none !important;
        }
        .text-center { text-align: center; }
        .mt-1 { margin-top: 1rem; }
        .mb-1 { margin-bottom: 1rem; }

    </style>
</head>
<body>

    <div id="app-container">
        <header>
            <h1 id="app-title">Split My Bill</h1>
            <div id="auth-section">
                <!-- This will be populated by JavaScript -->
            </div>
        </header>

        <main>
            <!-- 
            ================================================================
            HTML VIEWS
            Each div below represents a "page" in our Single Page Application.
            JavaScript will control which one is visible at any time.
            ================================================================
            -->

            <!-- VIEW 1: AUTHENTICATION (Login/Signup) -->
            <div id="auth-view" class="view">
                <div class="card">
                    <div class="tabs">
                        <div id="login-tab" class="tab active">Login</div>
                        <div id="signup-tab" class="tab">Sign Up</div>
                    </div>
                    
                    <!-- Login Form -->
                    <form id="login-form">
                        <input type="email" id="login-email" placeholder="Email" required>
                        <input type="password" id="login-password" placeholder="Password" required>
                        <button type="submit">Login</button>
                    </form>

                    <!-- Signup Form -->
                    <form id="signup-form">
                        <input type="text" id="signup-name" placeholder="Full Name" required>
                        <input type="email" id="signup-email" placeholder="Email" required>
                        <input type="password" id="signup-password" placeholder="Password" required>
                        <button type="submit">Create Account</button>
                    </form>
                    
                    <div id="auth-message" class="form-message hidden"></div>
                </div>
            </div>

            <!-- VIEW 2: DASHBOARD -->
            <div id="dashboard-view" class="view">
                <div class="card">
                    <h2>My Groups</h2>
                    <ul id="groups-list">
                        <!-- JS will render groups here -->
                    </ul>
                    <form id="create-group-form" class="mt-1">
                        <input type="text" id="new-group-name" placeholder="New Group Name" required>
                        <button type="submit">Create Group</button>
                    </form>
                </div>
                <div class="card">
                    <h2>Overall Balances</h2>
                    <p class="text-light-color mb-1">A summary of who you owe and who owes you across all groups.</p>
                    <ul id="balances-summary-list">
                        <!-- JS will render balance summary here -->
                    </ul>
                </div>
                 <div class="card">
                    <h2>Quick Actions</h2>
                    <button id="scan-qr-button">Scan Restaurant QR</button>
                </div>
            </div>

            <!-- VIEW 3: GROUP DETAILS -->
            <div id="group-view" class="view">
                <div id="group-view-header">
                    <h2 id="group-name-header"></h2>
                    <button id="back-to-dashboard" class="secondary"> ← Back</button>
                </div>
                
                <div class="card">
                    <h3>Expenses</h3>
                    <ul id="expenses-list">
                        <!-- JS will render expenses here -->
                    </ul>
                    <button id="show-add-expense-form-btn" class="mt-1">Add New Expense</button>
                    
                    <form id="add-expense-form" class="hidden mt-1">
                        <input type="text" id="expense-description" placeholder="Description (e.g., Dinner)" required>
                        <input type="number" id="expense-amount" placeholder="Total Amount" step="0.01" required>
                        <div>
                            <label for="expense-paid-by">Paid by:</label>
                            <select id="expense-paid-by" required></select>
                        </div>
                        <div>
                            <label>Split between:</label>
                            <div id="expense-split-with"></div>
                        </div>
                        <button type="submit">Add Expense</button>
                        <button type="button" id="cancel-add-expense-btn" class="secondary">Cancel</button>
                    </form>
                </div>
                
                <div class="card">
                    <h3>Group Balances</h3>
                    <ul id="balances-list">
                        <!-- JS will render group-specific balances -->
                    </ul>
                </div>
                
                 <div class="card">
                    <h3>Group Members</h3>
                    <ul id="members-list">
                        <!-- JS will render members here -->
                    </ul>
                     <form id="add-member-form" class="mt-1">
                        <input type="email" id="new-member-email" placeholder="Add member by email" required>
                        <button type="submit">Add Member</button>
                    </form>
                </div>
            </div>
            
            <!-- VIEW 4: QR CODE SCANNER (SIMULATION) -->
            <div id="qr-scanner-view" class="view">
                <h2>Scan Restaurant Menu</h2>
                <div class="scanner-box">
                    <p>Point your camera at a QR code</p>
                    <p>(This is a simulation)</p>
                </div>
                <div class="text-center">
                    <button id="simulate-scan-btn">Simulate Successful Scan</button>
                    <button id="cancel-scan-btn" class="secondary mt-1">Cancel</button>
                </div>
            </div>
            
            <!-- VIEW 5: RESTAURANT MENU -->
            <div id="menu-view" class="view">
                <h2 id="restaurant-name"></h2>
                <div class="card">
                    <h3>Select Your Items</h3>
                    <form id="order-form">
                        <ul id="menu-items">
                            <!-- JS will render menu items here -->
                        </ul>
                        <div class="mt-1">
                            <label for="add-order-to-group">Add this expense to group:</label>
                            <select id="add-order-to-group"></select>
                        </div>
                        <button type="submit">Place My Order & Add to Bill</button>
                        <button type="button" id="cancel-order-btn" class="secondary">Cancel</button>
                    </form>
                </div>
            </div>

        </main>
        
        <footer>
            Built following the project plan. Client-side simulation using HTML, CSS, and JavaScript.
        </footer>
    </div>

    <!-- 
    ================================================================
    JAVASCRIPT LOGIC
    This script contains the entire application's logic.
    It handles state, data persistence (via localStorage), UI rendering,
    and all user interactions, following your project roadmap phase by phase.
    ================================================================
    -->
    <script>
        document.addEventListener('DOMContentLoaded', () => {

            // ================================================================
            // --- GLOBAL APP STATE & DOM ELEMENTS ---
            // ================================================================

            const appState = {
                currentUser: null, // object { id, name, email }
                currentView: 'auth', // 'auth', 'dashboard', 'group', 'qr', 'menu'
                currentGroupId: null,
                data: { // This will act as our database, stored in localStorage
                    users: [],
                    groups: [],
                    expenses: []
                }
            };

            // DOM Element references
            const views = {
                auth: document.getElementById('auth-view'),
                dashboard: document.getElementById('dashboard-view'),
                group: document.getElementById('group-view'),
                qr: document.getElementById('qr-scanner-view'),
                menu: document.getElementById('menu-view'),
            };

            const authSection = document.getElementById('auth-section');
            const appTitle = document.getElementById('app-title');

            // ================================================================
            // --- DATA PERSISTENCE (localStorage as Database) ---
            // ================================================================

            function saveData() {
                localStorage.setItem('splitMyBillData', JSON.stringify(appState.data));
            }

            function loadData() {
                const savedData = localStorage.getItem('splitMyBillData');
                if (savedData) {
                    appState.data = JSON.parse(savedData);
                } else {
                    // Initialize with some demo data if nothing is saved
                    initializeAppWithDemoData();
                }
                const savedUser = localStorage.getItem('splitMyBillUser');
                if(savedUser) {
                    appState.currentUser = JSON.parse(savedUser);
                }
            }
            
            function initializeAppWithDemoData() {
                // To make the app usable on first load without signup
                appState.data.users = [
                    { id: 1, name: 'Alice', email: 'alice@example.com', password: 'password123' },
                    { id: 2, name: 'Bob', email: 'bob@example.com', password: 'password123' },
                    { id: 3, name: 'Charlie', email: 'charlie@example.com', password: 'password123' }
                ];
                appState.data.groups = [
                    { id: 101, name: 'Road Trip', memberIds: [1, 2] },
                    { id: 102, name: 'Apartment Mates', memberIds: [1, 3] }
                ];
                appState.data.expenses = [
                    { id: 1001, groupId: 101, description: 'Gas', amount: 50, paidById: 1, splitWithUserIds: [1, 2] },
                    { id: 1002, groupId: 101, description: 'Snacks', amount: 20, paidById: 2, splitWithUserIds: [1, 2] },
                    { id: 1003, groupId: 102, description: 'Groceries', amount: 90, paidById: 1, splitWithUserIds: [1, 3] }
                ];
                saveData();
            }

            // ================================================================
            // --- PHASE 1: MVP - CORE LOGIC (Auth, Groups, Expenses) ---
            // ================================================================

            // --- User Authentication ---
            function handleSignup(name, email, password) {
                if (appState.data.users.find(u => u.email === email)) {
                    return { success: false, message: 'Email already exists.' };
                }
                const newUser = {
                    id: Date.now(), // simple unique ID
                    name,
                    email,
                    password // In a real app, ALWAYS HASH THIS!
                };
                appState.data.users.push(newUser);
                saveData();
                return { success: true, message: 'Account created successfully! Please log in.' };
            }

            function handleLogin(email, password) {
                const user = appState.data.users.find(u => u.email === email && u.password === password);
                if (user) {
                    appState.currentUser = { id: user.id, name: user.name, email: user.email };
                    localStorage.setItem('splitMyBillUser', JSON.stringify(appState.currentUser));
                    return { success: true };
                }
                return { success: false, message: 'Invalid email or password.' };
            }

            function handleLogout() {
                appState.currentUser = null;
                localStorage.removeItem('splitMyBillUser');
                showView('auth');
            }
            
            // --- Group Management ---
            function createGroup(name) {
                if (!appState.currentUser) return;
                const newGroup = {
                    id: Date.now(),
                    name,
                    memberIds: [appState.currentUser.id]
                };
                appState.data.groups.push(newGroup);
                saveData();
            }
            
            function addUserToGroup(email, groupId) {
                const userToAdd = appState.data.users.find(u => u.email === email);
                const group = appState.data.groups.find(g => g.id === groupId);
                if (userToAdd && group && !group.memberIds.includes(userToAdd.id)) {
                    group.memberIds.push(userToAdd.id);
                    saveData();
                    return { success: true };
                }
                return { success: false, message: 'User not found or already in group.' };
            }

            // --- Expense Management ---
            function addExpense(groupId, description, amount, paidById, splitWithUserIds) {
                const newExpense = {
                    id: Date.now(),
                    groupId,
                    description,
                    amount,
                    paidById,
                    splitWithUserIds
                };
                appState.data.expenses.push(newExpense);
                saveData();
            }

            // --- Balance Calculation (The heart of the app) ---
            function calculateBalances(forGroupId = null) {
                const balances = {}; // { userId: amount }
                const expensesToConsider = forGroupId 
                    ? appState.data.expenses.filter(e => e.groupId === forGroupId)
                    : appState.data.expenses;
                
                // Initialize balances for all users involved
                const allUserIds = new Set();
                appState.data.users.forEach(u => allUserIds.add(u.id));
                allUserIds.forEach(id => balances[id] = 0);

                expensesToConsider.forEach(expense => {
                    const amount = expense.amount;
                    const paidById = expense.paidById;
                    const splitWithUserIds = expense.splitWithUserIds;
                    const numSplits = splitWithUserIds.length;
                    
                    if(numSplits === 0) return;
                    
                    const share = amount / numSplits;
                    
                    // The payer gets credited the full amount
                    balances[paidById] += amount;
                    
                    // Each person in the split gets debited their share
                    splitWithUserIds.forEach(userId => {
                        balances[userId] -= share;
                    });
                });
                return balances;
            }

            // ================================================================
            // --- UI RENDERING & VIEW MANAGEMENT ---
            // ================================================================

            function showView(viewName, params = {}) {
                appState.currentView = viewName;
                Object.values(views).forEach(view => view.classList.remove('active'));
                views[viewName].classList.add('active');

                // Run view-specific render logic
                switch (viewName) {
                    case 'auth': renderAuthView(); break;
                    case 'dashboard': renderDashboard(); break;
                    case 'group': renderGroupView(params.groupId); break;
                    case 'qr': renderQRScannerView(); break;
                    case 'menu': renderMenuView(params.restaurantName); break;
                }
                updateAuthSection();
            }

            function updateAuthSection() {
                if (appState.currentUser) {
                    authSection.innerHTML = `
                        <div id="user-info">
                            <span>Welcome, <strong>${appState.currentUser.name}</strong>!</span>
                            <button id="logout-btn">Logout</button>
                        </div>
                    `;
                } else {
                    authSection.innerHTML = `<span>Please log in or sign up</span>`;
                }
            }
            
            // --- Render Auth View ---
            function renderAuthView() {
                document.getElementById('login-form').reset();
                document.getElementById('signup-form').reset();
                document.getElementById('auth-message').classList.add('hidden');
                
                // Tab switching logic
                const loginTab = document.getElementById('login-tab');
                const signupTab = document.getElementById('signup-tab');
                const loginForm = document.getElementById('login-form');
                const signupForm = document.getElementById('signup-form');
                
                loginTab.onclick = () => {
                    loginTab.classList.add('active');
                    signupTab.classList.remove('active');
                    loginForm.style.display = 'flex';
                    signupForm.style.display = 'none';
                };
                 signupTab.onclick = () => {
                    signupTab.classList.add('active');
                    loginTab.classList.remove('active');
                    signupForm.style.display = 'flex';
                    loginForm.style.display = 'none';
                };
                
                loginTab.click(); // Start on login tab
            }

            // --- Render Dashboard View ---
            function renderDashboard() {
                if (!appState.currentUser) {
                    showView('auth');
                    return;
                }
                
                // Render Groups
                const groupsList = document.getElementById('groups-list');
                const userGroups = appState.data.groups.filter(g => g.memberIds.includes(appState.currentUser.id));
                
                if(userGroups.length === 0) {
                    groupsList.innerHTML = `<p class="text-light-color">You are not in any groups yet. Create one below!</p>`;
                } else {
                    groupsList.innerHTML = userGroups.map(group => `
                        <li class="list-item" data-group-id="${group.id}">
                            <span>${group.name}</span>
                            <span>→</span>
                        </li>
                    `).join('');
                }
                
                // Render Overall Balances
                const balancesSummaryList = document.getElementById('balances-summary-list');
                const overallBalances = calculateBalances();
                let summaryHTML = '';
                
                Object.entries(overallBalances).forEach(([userId, balance]) => {
                    if (parseInt(userId) === appState.currentUser.id) return; // Don't show balance with self
                    
                    const otherUser = appState.data.users.find(u => u.id === parseInt(userId));
                    if(!otherUser) return;
                    
                    // This is a simplified calculation for summary.
                    // We need to check who owes who. This requires a more complex calculation,
                    // but for this prototype, we'll simplify.
                    // Let's check balances with just one other person at a time.
                    
                    let userOwes = 0;
                    
                    appState.data.expenses.forEach(exp => {
                        if (exp.paidById == userId && exp.splitWithUserIds.includes(appState.currentUser.id)) {
                             userOwes += exp.amount / exp.splitWithUserIds.length;
                        }
                        if (exp.paidById == appState.currentUser.id && exp.splitWithUserIds.includes(parseInt(userId))) {
                             userOwes -= exp.amount / exp.splitWithUserIds.length;
                        }
                    });

                    if (Math.abs(userOwes) > 0.01) {
                         if (userOwes > 0) {
                             summaryHTML += `<li class="list-item">You owe <span class="balance-owed">${otherUser.name} $${userOwes.toFixed(2)}</span></li>`;
                         } else {
                             summaryHTML += `<li class="list-item">${otherUser.name} owes you <span class="balance-due">$${Math.abs(userOwes).toFixed(2)}</span></li>`;
                         }
                    }
                });

                balancesSummaryList.innerHTML = summaryHTML || `<p class="text-light-color">All settled up!</p>`;
            }
            
            // --- Render Group View ---
            function renderGroupView(groupId) {
                appState.currentGroupId = groupId;
                const group = appState.data.groups.find(g => g.id === groupId);
                if (!group) {
                    showView('dashboard');
                    return;
                }

                document.getElementById('group-name-header').textContent = group.name;

                // Render Members
                const membersList = document.getElementById('members-list');
                membersList.innerHTML = group.memberIds.map(id => {
                    const member = appState.data.users.find(u => u.id === id);
                    return `<li class="list-item">${member.name} ${member.id === appState.currentUser.id ? '(You)' : ''}</li>`;
                }).join('');
                
                // Render Expenses
                const expensesList = document.getElementById('expenses-list');
                const groupExpenses = appState.data.expenses.filter(e => e.groupId === groupId);
                expensesList.innerHTML = groupExpenses.length > 0 ? groupExpenses.map(expense => {
                    const paidBy = appState.data.users.find(u => u.id === expense.paidById);
                    return `
                        <li class="expense-item">
                            <div class="expense-details">
                                <span class="expense-description">${expense.description}</span>
                                <span class="expense-amount">$${expense.amount.toFixed(2)}</span>
                            </div>
                            <div class="expense-paid-by">Paid by ${paidBy.name}</div>
                        </li>
                    `;
                }).join('') : `<p class="text-light-color">No expenses yet.</p>`;

                // Render Group Balances
                const balancesList = document.getElementById('balances-list');
                const groupBalances = calculateBalances(groupId);
                let balancesHTML = '';
                const members = group.memberIds.map(id => appState.data.users.find(u => u.id === id));

                members.forEach(member => {
                    if (member.id === appState.currentUser.id) return;
                    
                    let netBalance = 0; // Positive if they owe me, negative if I owe them
                    groupExpenses.forEach(exp => {
                        const share = exp.amount / exp.splitWithUserIds.length;
                        if(exp.paidById === appState.currentUser.id && exp.splitWithUserIds.includes(member.id)){
                            netBalance += share;
                        }
                        if(exp.paidById === member.id && exp.splitWithUserIds.includes(appState.currentUser.id)){
                            netBalance -= share;
                        }
                    });
                    
                    if(Math.abs(netBalance) > 0.01) {
                        if (netBalance > 0) {
                            balancesHTML += `
                                <li class="balance-item">
                                    <span>${member.name} owes you <span class="balance-due">$${netBalance.toFixed(2)}</span></span>
                                    <button class="remind-btn" data-user-id="${member.id}">Remind</button>
                                </li>`;
                        } else {
                            balancesHTML += `
                                <li class="balance-item">
                                    <span>You owe ${member.name} <span class="balance-owed">$${Math.abs(netBalance).toFixed(2)}</span></span>
                                    <button class="settle-up-btn" data-user-id="${member.id}" data-amount="${Math.abs(netBalance).toFixed(2)}">Settle Up</button>
                                </li>`;
                        }
                    }
                });
                balancesList.innerHTML = balancesHTML || `<p class="text-light-color">Everyone is settled up in this group.</p>`;
                
                // Populate 'Add Expense' form dropdowns
                const paidBySelect = document.getElementById('expense-paid-by');
                const splitWithDiv = document.getElementById('expense-split-with');
                paidBySelect.innerHTML = members.map(m => `<option value="${m.id}">${m.name}</option>`).join('');
                splitWithDiv.innerHTML = members.map(m => `
                    <label>
                        <input type="checkbox" name="splitUser" value="${m.id}" checked> ${m.name}
                    </label>
                `).join('');
            }
            
            // ================================================================
            // --- PHASE 2: QR CODE ORDERING SYSTEM (SIMULATION) ---
            // ================================================================

            function renderQRScannerView() {
                // This view is mostly static HTML, just needs to be shown.
            }

            const fakeMenu = {
                'The Corner Bistro': [
                    { id: 201, name: 'Classic Burger', price: 12.50 },
                    { id: 202, name: 'Fries', price: 4.00 },
                    { id: 203, name: 'Soda', price: 2.50 },
                    { id: 204, name: 'Caesar Salad', price: 10.00 },
                ]
            };
            
            function renderMenuView(restaurantName) {
                document.getElementById('restaurant-name').textContent = restaurantName;
                const menuItemsList = document.getElementById('menu-items');
                const menu = fakeMenu[restaurantName] || [];
                
                menuItemsList.innerHTML = menu.map(item => `
                    <li>
                        <input type="checkbox" data-price="${item.price}" data-name="${item.name}">
                        <span class="item-name">${item.name}</span>
                        <span class="item-price">$${item.price.toFixed(2)}</span>
                    </li>
                `).join('');
                
                // Populate group dropdown
                const groupSelect = document.getElementById('add-order-to-group');
                const userGroups = appState.data.groups.filter(g => g.memberIds.includes(appState.currentUser.id));
                groupSelect.innerHTML = userGroups.map(g => `<option value="${g.id}">${g.name}</option>`).join('');
            }

            // ================================================================
            // --- EVENT LISTENERS & APP INITIALIZATION ---
            // ================================================================
            
            function setupEventListeners() {
                // Use event delegation on a main container for dynamic elements
                const appContainer = document.getElementById('app-container');
                
                appContainer.addEventListener('submit', (e) => {
                    e.preventDefault();
                    
                    // --- Auth Forms ---
                    if (e.target.id === 'login-form') {
                        const email = document.getElementById('login-email').value;
                        const password = document.getElementById('login-password').value;
                        const result = handleLogin(email, password);
                        if (result.success) {
                            showView('dashboard');
                        } else {
                           const msgEl = document.getElementById('auth-message');
                           msgEl.textContent = result.message;
                           msgEl.className = 'form-message error';
                        }
                    }
                    if (e.target.id === 'signup-form') {
                        const name = document.getElementById('signup-name').value;
                        const email = document.getElementById('signup-email').value;
                        const password = document.getElementById('signup-password').value;
                        const result = handleSignup(name, email, password);
                        const msgEl = document.getElementById('auth-message');
                        msgEl.textContent = result.message;
                        msgEl.className = `form-message ${result.success ? 'success' : 'error'}`;
                        if (result.success) {
                            document.getElementById('signup-form').reset();
                            document.getElementById('login-tab').click();
                        }
                    }
                    
                    // --- Dashboard Forms ---
                    if (e.target.id === 'create-group-form') {
                        const groupName = document.getElementById('new-group-name').value;
                        if(groupName) {
                            createGroup(groupName);
                            e.target.reset();
                            renderDashboard();
                        }
                    }
                    
                    // --- Group View Forms ---
                    if (e.target.id === 'add-member-form') {
                        const email = document.getElementById('new-member-email').value;
                        const result = addUserToGroup(email, appState.currentGroupId);
                        if (!result.success) alert(result.message);
                        e.target.reset();
                        renderGroupView(appState.currentGroupId);
                    }
                    
                    if (e.target.id === 'add-expense-form') {
                        const description = document.getElementById('expense-description').value;
                        const amount = parseFloat(document.getElementById('expense-amount').value);
                        const paidById = parseInt(document.getElementById('expense-paid-by').value);
                        const splitCheckboxes = document.querySelectorAll('#expense-split-with input:checked');
                        const splitWithUserIds = Array.from(splitCheckboxes).map(cb => parseInt(cb.value));
                        
                        addExpense(appState.currentGroupId, description, amount, paidById, splitWithUserIds);
                        e.target.reset();
                        e.target.classList.add('hidden');
                        document.getElementById('show-add-expense-form-btn').classList.remove('hidden');
                        renderGroupView(appState.currentGroupId);
                    }
                    
                    // --- Menu View Form ---
                    if (e.target.id === 'order-form') {
                        const selectedItems = Array.from(document.querySelectorAll('#menu-items input:checked'));
                        if (selectedItems.length === 0) {
                            alert('Please select at least one item.');
                            return;
                        }
                        
                        const totalAmount = selectedItems.reduce((sum, item) => sum + parseFloat(item.dataset.price), 0);
                        const description = "Restaurant Order: " + selectedItems.map(item => item.dataset.name).join(', ');
                        const groupId = parseInt(document.getElementById('add-order-to-group').value);
                        const paidById = appState.currentUser.id;
                        
                        // For this simulation, we assume the person ordering pays for themselves.
                        // A real app would have more complex real-time splitting.
                        const splitWithUserIds = [paidById];
                        
                        addExpense(groupId, description, totalAmount, paidById, splitWithUserIds);
                        alert(`Order for $${totalAmount.toFixed(2)} added to your group!`);
                        showView('group', { groupId });
                    }
                });

                appContainer.addEventListener('click', (e) => {
                    const target = e.target;
                    
                    // Header title click
                    if (target === appTitle && appState.currentUser) {
                        showView('dashboard');
                    }
                    
                    // Logout button
                    if (target.id === 'logout-btn') {
                        handleLogout();
                    }
                    
                    // Go to group view from dashboard
                    const groupItem = target.closest('.list-item[data-group-id]');
                    if (groupItem) {
                        const groupId = parseInt(groupItem.dataset.groupId);
                        showView('group', { groupId });
                    }
                    
                    // Group view navigation/actions
                    if (target.id === 'back-to-dashboard') showView('dashboard');
                    if (target.id === 'show-add-expense-form-btn') {
                        target.classList.add('hidden');
                        document.getElementById('add-expense-form').classList.remove('hidden');
                    }
                    if (target.id === 'cancel-add-expense-btn') {
                        document.getElementById('add-expense-form').classList.add('hidden');
                        document.getElementById('show-add-expense-form-btn').classList.remove('hidden');
                    }

                    // PHASE 4: Reminders (Simulation)
                    if(target.classList.contains('remind-btn')) {
                        const userId = parseInt(target.dataset.userId);
                        const userToRemind = appState.data.users.find(u => u.id === userId);
                        alert(`Reminder sent to ${userToRemind.name}! (This is a simulation)`);
                    }
                    
                    // PHASE 3: Payment Integration (Simulation)
                    if(target.classList.contains('settle-up-btn')) {
                        const toUserId = parseInt(target.dataset.userId);
                        const amount = parseFloat(target.dataset.amount);
                        const toUser = appState.data.users.find(u => u.id === toUserId);
                        
                        if(confirm(`This will simulate paying $${amount.toFixed(2)} to ${toUser.name} via a mock Payment Gateway. Proceed?`)) {
                            // Simulate payment by adding a "negative" expense
                            const description = `Payment from ${appState.currentUser.name} to ${toUser.name}`;
                            addExpense(appState.currentGroupId, description, amount, appState.currentUser.id, [toUser.id]);
                            alert('Payment successful! Balances have been updated.');
                            renderGroupView(appState.currentGroupId);
                        }
                    }

                    // QR Code Flow
                    if (target.id === 'scan-qr-button') showView('qr');
                    if (target.id === 'simulate-scan-btn') showView('menu', { restaurantName: 'The Corner Bistro' });
                    if (target.id === 'cancel-scan-btn') showView('dashboard');
                    if (target.id === 'cancel-order-btn') showView('group', { groupId: appState.currentGroupId });
                });
            }

            // --- Initializer ---
            function init() {
                loadData();
                setupEventListeners();
                if (appState.currentUser) {
                    showView('dashboard');
                } else {
                    showView('auth');
                }
            }
            
            // Start the application
            init();

        });
    </script>

</body>
</html>
