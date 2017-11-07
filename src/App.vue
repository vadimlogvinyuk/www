<template>
  <div id="app">

    <nav class="navbar navbar-expand-lg navbar-light bg-light ">

      <a class="navbar-brand" href="#">Notes </a>
      <button class="navbar-toggler" type="button" data-toggle="collapse"
              data-target="#navbarNavDropdown" aria-controls="navbarNavDropdown"
              aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <ul class="navbar-nav">

          <div v-if="Users.length > 1" class="dropdown">
            <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton"
                    data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              {{currentUser.userName}}
            </button>
            <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
              <a class="dropdown-item" href="#" v-for="user in Users"  v-if="user !== currentUser"
                 @click="chooseOtherUser(user)">{{user.userName}}</a>

            </div>
          </div>

          <button type="button" class="btn btn-light" data-toggle="modal"
                  data-target="#exampleModal" data-whatever="@mdo">Add user</button>

          <button v-if="Users.length > 0" type="button" class="btn btn-light" data-toggle="modal"
                  data-target="#allNote" data-whatever="@mdo">All notes</button>
        </ul>


        <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog"
             aria-labelledby="exampleModalLabel" aria-hidden="true">
          <div class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Create new user</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                  <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <div class="modal-body">
                <form>
                  <div class="form-group">
                    <label for="recipient-name" class="col-form-label">name:</label>
                    <input type="text" v-model="name" class="form-control" id="recipient-name">
                  </div>
                </form>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary"   data-dismiss="modal"  @click="OnClickAdd" >Add</button>
              </div>
            </div>
          </div>
        </div>


        <div class="modal fade" id="allNote" tabindex="-1" role="dialog" aria-labelledby="allNoteLabel" aria-hidden="true">
          <div class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="allNoteLabel">notes</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                  <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <div class="modal-body">
                <ul>
                  <li v-for="user in Users">{{user.userName}}
                    <ul>
                  <li v-for="a in user.Notes">{{a.note}}</li>
                    </ul>
                  </li>

                </ul>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <div v-if="Users.length > 0">

      <button type="button" class="btn btn-primary btn-lg btn-block" data-toggle="modal" data-target="#addNoteModal"
              data-whatever="@getbootstrap">add note</button>

      <div class="modal fade" id="addNoteModal" tabindex="-1" role="dialog" aria-labelledby="addNoteModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="addNoteModalLabel">New note</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group">
                  <label for="notesText" class="col-form-label">Text:</label>
                  <textarea v-model="NotesText" class="form-control" id="notesText"></textarea>
                </div>
              </form>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              <button type="button" class="btn btn-primary" data-dismiss="modal" @click="onClickAddNote">Add note</button>
            </div>
          </div>
        </div>
      </div>


    <ul class="list-inline">
      <li class="list-group-item list-group-item-action list-group-item-light list-group-item-danger"
          v-for="(n,index) in currentUser.Notes" @click="n.act = !n.act">
        {{n.note}}

        <button type="button" class="close" aria-label="Close" @click="currentUser.Notes.splice(index,1)">
          <span aria-hidden="true">&times;</span>
        </button>

          <ul  v-if="Users.length > 1" class="navbar-nav">
              <a class="nav-link dropdown-toggle" href="#" id="liDropdownMenuLink"
                 data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                share
              </a>
              <div  class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
                <ul>
                  <li v-for="user in Users"  v-if="user !== currentUser"  @click="shareNote(user,n.note)">
                    {{user.userName}}</li>
                </ul>
              </div>
          </ul>
      </li>
    </ul>

  </div>

  </div>

</template>

<script>
export default {
  name: 'app',

    data () {
    return {
        NotesText:"",
        name: "",
        Users:[],
        currentUser: {},
    }
  },
    methods: {

        OnClickAdd: function () {
            if (this.name == "") {
                alert("Oops, user name is not correct");
                return;
            }

            var userFound = false;
            for (var i=0; this.Users.length > i; i++)
            {
                if (this.name == this.Users[i].userName) userFound=true;
            }

            if (!userFound) {

                var user = {
                    userName:this.name,
                    Notes:[]
                };

                this.Users.push(user);
                this.currentUser = user;
            } else {
                alert("Oops name: "+this.name+ " is use");

            }

            this.name = '';


        },

        onClickAddNote: function () {
            if (this.NotesText.length == 0) {
                alert("please add text ...");
            }else {
                this.currentUser.Notes.push({note:this.NotesText,act: false});
                this.NotesText = '';
            }
            
        },
        chooseOtherUser: function (user) {
            this.currentUser = user;
        },

        shareNote: function(user,noteText) {
            user.Notes.push({note:noteText,act: false});
        }


    }
}




</script>

<style>



</style>
