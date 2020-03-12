<template>
  <div class="container-fluid" style="height: 100vh;">
    <div class="row position-absolute w-100" style="height: 100vh;">
      <div class="col-md-6" v-bind:class="[nowState.bgColor]"></div>
      <div class="col-md-6 d-flex" v-bind:class="[nowState.bgColor]">
        <div class="w-25 h-100" v-bind:class="[nowState.bgColor]"></div>
        <div class="w-75 bg-dark h-100"></div>
      </div>
    </div>
    <div class="container h-100">
        <div class="row h-100">
          <div class="col-md-5 py-4 d-flex flex-column align-items-center justify-content-between h-100">
            <div class="input-group my-3">
              <input v-model="todo" type="text" v-bind:class="[nowState.textSecondaryColor]" class="form-control rounded-0 border-0" placeholder="Want to do" @keyup.enter="addTodo">
              <div class="input-group-append">
                <button class="btn btn-primary rounded-0 bg-white border-0" type="button" id="add" @click="addTodo"><span v-bind:class="[nowState.textSecondaryColor]" class="h6">+</span></button>
              </div>
            </div>
            <div class="now-todo w-100 mt-5">
              <div class="d-flex mt-5">
                <div class="text-dark rounded-circle mx-2" style="border: 2px solid #003164; width: 48px; height: 48px;" v-if="countDown.todo !== ''" ></div><p class="mb-2 mt-1">{{ countDown.todo }}</p>
              </div>
              <p v-bind:class="[nowState.textSecondaryColor]" class="font-weight-bold w-100" style="font-size: 160px; line-height: 1;">{{ countDown.show }}</p>
            </div>
              
            <div class="w-100 mb-1">
                <ul class="nav nav-tabs row no-gutters border-0" id="myTab" role="tablist">
                  <li class="col-4 nav-item">
                    <a class="nav-link border-0 text-dark text-center active" id="todo" data-toggle="tab" href="#todo" role="tab" v-on:click="todoShowStatus = 'todo'">待辦事項</a>
                  </li>
                  <li class="col-4 nav-item">
                    <a class="nav-link border-0 text-dark text-center" id="complete" data-toggle="tab" href="#complete" role="tab" v-on:click="todoShowStatus = 'complete'">完成事項</a>
                  </li>
                  <li class="col-4 nav-item">
                    <a class="nav-link border-0 text-dark text-center" id="all" data-toggle="tab" href="#all" role="tab" v-on:click="todoShowStatus = 'all'">全部事項</a>
                  </li>
                </ul>
                <div class="tab-content" id="todo" style="min-height: 200px; max-height: 200px; overflow-y: scroll;">
                    <ul class="list-group">
                        <li  v-for="(item, index) in todoListStatus" :key="item.todo" v-bind:class="[nowState.bgColor]" class="list-group-item border-0 rounded-0 py-1 mt-1" style="border-bottom: 1px solid #dee2e6 !important;">
                            <div class="d-flex align-items-center">
                                <input type="checkbox" v-model="item.done" :key="item.todo">
                                <p class="my-0 ml-3 mr-auto">{{ item.todo }}</p>
                                <a href="#" v-if="!item.done" class="text-decoration-none text-dark d-flex align-items-center" @click="addToCountDown(item)"><i class="material-icons">play_circle_outline</i></a>
                                <a href="#" class="text-decoration-none text-dark d-flex align-items-center" @click="deleteTodo(index)"><i class="material-icons">delete_outline</i></a>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
          </div>
          <div class="col-md-6 d-flex align-items-center justify-content-end">
            <div v-bind:class="[nowState.bgSecondaryColor]" class="rounded-circle d-flex align-items-center justify-content-center" style="width: 540px; height: 540px;">
              <p v-bind:class="[nowState.textColor]" @click="countdown($event)"><i class="material-icons" style="cursor: pointer; font-size: 96px;">{{ countDown.icon }}</i></p>
            </div>
          </div>
          <div class="col-md-1 d-flex flex-column align-items-center h-100 py-4">
            <p v-bind:class="[nowState.textColor]" class="text-right mt-3" style="writing-mode: tb-rl; letter-spacing: 2px;">專注的，做好一件事。</p>
            <h3 class="text-mode-vertical text-white mb-0 mt-auto">POMODORO</h3>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nowState: {
        state: 'work',
        bgColor: 'bg-primary',
        bgSecondaryColor: 'bg-secondary',
        textColor: 'text-primary',
        textSecondaryColor: 'text-secondary',
      },
      countDown: {
        todo: '',
        todoDone: false,
        time: 1500,
        status: false,
        show: '25:00',
        icon: 'play_circle_filled'
      },
      countController: '',
      todoShowStatus: 'todo',
      todo: '',
      todoList:  [],
      todoLen: 0
    };
  },
  methods: {
    addTodo () {
      const vm = this;
      if (vm.todo !== '') {
        const todoObj = {
          todo: vm.todo,
          done: false
        } 
        vm.todoList.push(todoObj)
        vm.todoLen++
        vm.todo = ''; 
        localStorage.setItem('pomodoroTodo', JSON.stringify(vm.todoList));
      }
    },
    deleteTodo (index) {
      const vm = this;
      vm.todoList.splice(index, 1);
      localStorage.setItem('pomodoroTodo', JSON.stringify(vm.todoList));
    },
    addToCountDown (todoDetail) {
      const vm = this;
      vm.countDown.todo = todoDetail.todo;
      vm.countDown.todoDone = todoDetail.done;
      vm.countDown.time = 10;
      vm.countDown.status = false;
      vm.countDown.show = '25:00';
      vm.countDown.icon = 'play_circle_filled';
    },
    workTime() {
      const vm = this;
      vm.countDown.todo = '';
      vm.countDown.icon = 'play_circle_filled';
      vm.countDown.time = 1500;
      const min = Math.floor(vm.countDown.time / 60);
      const sec = vm.countDown.time % 60;
      vm.countDown.show = `${min}:${sec < 10 ? '0' : ''}${sec}`;
      
      vm.nowState = {
        state: 'work',
        bgColor: 'bg-primary',
        bgSecondaryColor: 'bg-secondary',
        textColor: 'text-primary',
        textSecondaryColor: 'text-secondary',
      };
    },
    breakTime() {
      const vm = this;
      vm.countDown.todo = '完成事項了！休息一下吧！';
      vm.countDown.icon = 'play_circle_filled';
      vm.countDown.time = 5;
      const min = Math.floor(vm.countDown.time / 60);
      const sec = vm.countDown.time % 60;
      vm.countDown.show = `${min}:${sec < 10 ? '0' : ''}${sec}`;

      vm.nowState = {
        state: 'break',
        bgColor: 'bg-primary-b',
        bgSecondaryColor: 'bg-secondary-b',
        textColor: 'text-primary-b',
        textSecondaryColor: 'text-secondary-b',
      };
    },
    countdown(e) {
      e.preventDefault();
      const vm = this;
      vm.countDown.status = !vm.countDown.status;
      if (vm.countDown.status) {
        vm.countDown.icon = 'pause_circle_filled';
        vm.countController = setInterval(() => {
          vm.countDown.time -= 1;
          const min = Math.floor(vm.countDown.time / 60);
          const sec = vm.countDown.time % 60;
          vm.countDown.show = `${min}:${sec < 10 ? '0' : ''}${sec}`;
          console.log(vm.nowState.state)
          if (vm.countDown.time == 0) {
            if (vm.nowState.state === 'work') {
              vm.countDown.time = 0;
              vm.todoList.forEach(todo => {
                if (todo.todo === vm.countDown.todo) {
                  todo.done = true;
                  vm.breakTime();
                  localStorage.setItem('pomodoroTodo', JSON.stringify(vm.todoList));
                }
              });
              clearInterval(vm.countController);
            } else {
              vm.countDown.time = 0;
              vm.workTime();
              clearInterval(vm.countController);
            }
          }
        }, 1000);
      } else {
        vm.countDown.icon = 'play_circle_filled';
        clearInterval(vm.countController);
      }
    },
  },
  computed: {
    todoListStatus: function() {
      const vm = this;
      if (vm.todoShowStatus == 'all') {
        return vm.todoList
      } else if (vm.todoShowStatus == 'todo'){
        return vm.todoList.filter(item => item.done == false)
      } else if (vm.todoShowStatus == 'complete') {
        return vm.todoList.filter(item => item.done == true)
      }
    }
  },
  created() {
    const vm = this;
    vm.todoList = JSON.parse(localStorage.getItem('pomodoroTodo')) || [];
  }
};
</script>
