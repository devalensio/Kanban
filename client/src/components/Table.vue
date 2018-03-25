<template>
  <div class="container">
    <div class="row">
      <div class="col-md-3">
        <div class="card text-white bg-info col-md-12 items" style="max-width: 18rem;">
          <div class="card-header">Back-Log</div>
          <div class="card-body">
            <div class="card text-white bg-dark mb-3" style="max-width: 18rem;" v-for="(data,i) in tasks" v-bind:key="i" v-if="data.status === 'backlog'">
              <div class="card-header">{{data.title}}</div>
              <div class="card-body">
                <p class="card-title">Point: {{data.point}}</p>
                <p class="card-text">Assigned To: {{data.assigned}}</p>
                <div class="card-footer bg-transparent border-light" style="padding: 0px; padding-top: 17px">
                  <button type="button" class="btn btn-outline-light" @click="deleteData(i)">x</button>
                  <button type="button" class="btn btn-outline-light" @click="upStatus(i, data.status)">></button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-3">
        <div class="card text-white bg-warning col-md-12" style="max-width: 18rem;">
          <div class="card-header">To-Do</div>
          <div class="card-body">
            <div class="card text-white bg-dark mb-3" style="max-width: 18rem;"  v-for="(data,i) in tasks" v-bind:key="i" v-if="data.status === 'todo'">
              <div class="card-header">{{data.title}}</div>
              <div class="card-body">
                <p class="card-title">Point: {{data.point}}</p>
                <p class="card-text">Assigned To: {{data.assigned}}</p>
                <div class="card-footer bg-transparent border-light" style="padding: 0px; padding-top: 17px">
                  <button type="button" class="btn btn-outline-light" @click="downStatus(i, data.status)">&lt;</button>
                  <button type="button" class="btn btn-outline-light" @click="deleteData(i)">x</button>
                  <button type="button" class="btn btn-outline-light" @click="upStatus(i, data.status)">></button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-3">
        <div class="card text-white bg-danger col-md-12" style="max-width: 18rem;">
          <div class="card-header">Doing</div>
          <div class="card-body">
            <div class="card text-white bg-dark mb-3" style="max-width: 18rem;"  v-for="(data,i) in tasks" v-bind:key="i" v-if="data.status === 'doing'">
              <div class="card-header">Header</div>
              <div class="card-body">
                <p class="card-title">Point: {{data.point}}</p>
                <p class="card-text">Assigned To: {{data.assigned}}</p>
                <div class="card-footer bg-transparent border-light" style="padding: 0px; padding-top: 17px">
                  <button type="button" class="btn btn-outline-light" @click="downStatus(i, data.status)">&lt;</button>
                  <button type="button" class="btn btn-outline-light" @click="deleteData(i)">x</button>
                  <button type="button" class="btn btn-outline-light" @click="upStatus(i, data.status)">></button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-3">
        <div class="card text-white bg-secondary mb-3 col-md-12" style="max-width: 18rem;">
          <div class="card-header">Done</div>
          <div class="card-body">
            <div class="card text-white bg-dark mb-3" style="max-width: 18rem;" v-for="(data,i) in tasks" v-bind:key="i" v-if="data.status === 'done'">
              <div class="card-header">Header</div>
              <div class="card-body">
                <p class="card-title">Point: {{data.point}}</p>
                <p class="card-text">Assigned To: {{data.assigned}}</p>
                <div class="card-footer bg-transparent border-light" style="padding: 0px; padding-top: 17px">
                  <button type="button" class="btn btn-outline-light" @click="downStatus(i, data.status)">&lt;</button>
                  <button type="button" class="btn btn-outline-light" @click="deleteData(i)">x</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="modal fade" id="add-task" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">New Task</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="recipient-name" class="col-form-label">Title:</label>
                <input type="text" class="form-control" id="recipient-name" v-model='title'>
              </div>
              <div class="form-group">
                <label for="message-text" class="col-form-label">Description:</label>
                <textarea class="form-control" id="message-text" v-model='description'></textarea>
              </div>
              <div class="form-group">
                <label for="message-text" class="col-form-label">Point:</label>
                <input type="number" class="form-control" id="recipient-name" v-model='point'>
              </div>
              <div class="form-group">
                <label for="message-text" class="col-form-label">Assigned To:</label>
                <input type="text" class="form-control" id="recipient-name" v-model='assigned'>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" data-dismiss="modal" @click="addTask">Submit</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import * as firebase from 'firebase'

const config = {
  databaseURL: 'https://kanban-man.firebaseio.com',
  projectId: 'kanban-man'
}

const firebaseApp = firebase.initializeApp(config)
const db = firebaseApp.database()

export default {
  name: 'Table',
  data () {
    return {
      title: '',
      description: '',
      point: '',
      assigned: '',
      tasks: []
    }
  },
  created: function () {
    this.showTask()
  },
  methods: {
    addTask: function () {
      db.ref('tasks/').push(
        {
          title: this.title,
          description: this.description,
          point: this.point,
          assigned: this.assigned,
          status: 'backlog'
        }
      )
    },
    showTask: function () {
      let app = this
      db.ref('tasks/').on('value', function (snapshot) {
        app.tasks = snapshot.val()
        console.log(app.tasks)
      })
    },
    upStatus: function (id, status) {
      if (status === 'backlog') {
        db.ref('tasks/').child(id).update({
          status: 'todo'
        })
      } else if (status === 'todo') {
        db.ref('tasks/').child(id).update({
          status: 'doing'
        })
      } else if (status === 'doing') {
        db.ref('tasks/').child(id).update({
          status: 'done'
        })
      }
    },
    downStatus: function (id, status) {
      if (status === 'done') {
        db.ref('tasks/').child(id).update({
          status: 'doing'
        })
      } else if (status === 'doing') {
        db.ref('tasks/').child(id).update({
          status: 'todo'
        })
      } else if (status === 'todo') {
        db.ref('tasks/').child(id).update({
          status: 'backlog'
        })
      }
    },
    deleteData: function (id) {
      db.ref('tasks/').child(id).remove()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.items {
  margin: 5px;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
