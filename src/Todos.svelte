<script>

    import TodoItem from './TodoItem.svelte';

    let newTodoTitle = '';
    let currFilter = 'all';
    let nextId = 4;

    let todos = [
        {
            id: 1,
            title: 'My first todo',
            completed: false
        },
         {
            id: 2,
            title: 'My second todo',
            completed: false
        },
         {
            id: 3,
            title: 'My third todo',
            completed: false
        }
    ];

    function addTodo(event) {
        if (event.key === 'Enter')
        {
            todos = [...todos, {
                id: nextId,
                completed: false,
                title: newTodoTitle
            }];

            nextId++;
            newTodoTitle = '';
        }
    }

    function checkAllTodos(event) {
        todos.forEach(todo => todo.completed = event.target.checked);
        todos = todos;
    }

    function updateFilter(newFilter) {
        currFilter = newFilter;
    }

    function clearCompleted() {
        todos = todos.filter(todo => !todo.completed);
    }

    function handleDeleteTodo(event) {
        todos = todos.filter(todo => todo.id !== event.detail.id);
    }

    function handleToggleComplete(event) {
        const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
        const updatedTodo = {...todos[todoIndex], completed: !todos[todoIndex].completed}

        todos = [...todos.slice(0, todoIndex), updatedTodo, ...todos.slice(todoIndex+1)];
    }

    $: todosRemaining = filteredTodos.filter(todo => !todo.completed).length
    $: filteredTodos = currFilter === 'all' ? todos : currFilter === 'completed' 
        ? todos.filter(todo => todo.completed) : todos.filter(todo => !todo.completed)

</script>

<style>
    .container {
        max-width: 800px;
        margin: 10px auto;
        border: 2px solid  rgb(24, 75, 151);
        border-radius: 5px;
    }

    .todo-input {
        width: 100%;
        font-size: 18px;
        margin-bottom: 20px;
    }

    .inner-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 15px;
        border-top: 1px solid lightgrey;
        margin-bottom: 10px;
    }

    .inner-container, .inner-container-input {
        padding: 20px;
    }

    .todo-input {
        width: 95%;
        display: block;
        margin: 5px auto;
    }

    .todo-item {
        width: 94%;
        display: block;
        margin: 0 auto;
        margin-top: 10px;
    }

    .inner-container-input {
        margin-right: 12px;
    }

    h2 {
        text-align: center;
    }

    button {
        font-size: 15px;
        background: white;
        appearance: none;
    }

    button:hover, .active {
        background: rgb(24, 75, 151);
        color: white;
    }

</style>

<div class="container">
    <h2>Svelte Todo App</h2>
    <input type="text" class="todo-input" placeholder="Insert todo item ..." bind:value={newTodoTitle} on:keydown={addTodo} >

    {#each filteredTodos as todo}
        <div class="todo-item">
            <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} >
            
            </TodoItem>
        </div>
    {/each}

    <div class="inner-container">
        <div><label><input class="inner-container-input" type="checkbox" on:change={checkAllTodos}> Check All</label></div>
        <div>{todosRemaining} items left</div>
    </div>

    <div class="inner-container">
        <div>
            <button on:click={() => updateFilter('all')} class:active="{currFilter === 'all'}">All</button>
            <button on:click={() => updateFilter('active')} class:active="{currFilter === 'active'}">Active</button>
            <button on:click={() => updateFilter('completed')} class:active="{currFilter === 'completed'}">Completed</button>
        </div>

        <div>
            <button on:click={clearCompleted}>Clear Completed</button>
        </div>
    </div>

</div>