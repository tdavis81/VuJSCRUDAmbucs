<template>
 
 <div>
   <input id="clicker" v-model="clicker" type="checkbox" style="display:none" />

  <h2 style="float:left;margin:25px">Users</h2>
  <input style="width:10%;float:right;margin-right:10px" v-on:click="ShowSideBar(-999)" type="submit" value="New User"/>
  <table class="table table-striped">
    <thead>
      <tr>
        <th>Email</th>
        <th>Username</th>
        <th>User Role</th>
        <th>Active</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="User in Users" :key="User.id">
        <td>{{User.email}}</td>
        <td>{{User.username}}</td>
        <td>{{User.userRole}}</td>
        <td><input type="checkbox" v-model="User.isActive" onclick="return false;" /></td>
        <td><img style="width: 25px;height: 25px;margin-right:10px;" v-on:click="ShowSideBar(User.id)" src="@/assets/edit.png"/> <img style="width: 25px;height: 25px;margin-right:10px;"  v-on:click="deleteUser(User.id)" src="@/assets/delete.png"/></td>
      </tr>
    </tbody>

  </table>
  
  <!--Side Panel-->
    <div class="panel-wrap">
    
      <div class="panel">
      
        <h4 class="context-menu exit" v-on:click="exit()" style="float:right">X</h4>
        <h4>{{Title}}</h4>
        <input v-model="email" placeholder="Email" type="text"/> <br/>
        <input v-model="username" placeholder="Username" type="text" /> <br/>
        <input v-model="userRole" placeholder="User Role" type="text"/> <br/>
        <label style="float:left;margin-left:6px" for="isActiveCheckbox">Active</label>
        <input style="float:left;margin-left:5px;margin-top:5px" v-model="isActive" name="isActiveCheckbox" type="checkbox"/> <br/>
        <input type="submit" v-model="ButtonName" v-on:click="chooseDirection()"/> <br/>
      </div>
    
    </div>

 </div>
   
</template>



<script>

import axios from 'axios';

const url = axios.create({
  baseURL: 'http://localhost:3000/',
  json: true
})

export default {

  created () {

    axios.get('https://api.myjson.com/bins/r18jc')
      .then((response) =>{
        this.Users = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
    
  },

  data () {

    return {

      Users: [],
      email:"",
      username:"",
      userRole:"",
      isActive:"",
      user: {},
      Title: "",
      ButtonName: "",
      clicker: false
    }

  },

  methods:{

    ShowSideBar(id) {
      
      this.clicker = true;

      if(id == -999) {

        this.Title = "Add User";
        this.ButtonName = "Add";
        this.email = "";
        this.username = "",
        this.userRole = "",
        this.isActive = false

      } else {

        this.ButtonName = "Update";
        this.Title = "Edit / View User";
        this.User = this.Users.find(k => k.id==id);
         
        this.email = this.User.email;
        this.username = this.User.username;
        this.userRole = this.User.userRole;
        this.isActive = this.User.isActive;

      }
      
    },
    exit() {

      this.clicker = false;

    },

    addRecord() {

      var newUser = {
        email: this.email,
        username: this.username,
        userRole: this.userRole,
        isActive: this.isActive
      }

      axios.post( url + 'AddUser', newUser);
    
    },

    updateRecord() {

      this.User.email = this.email;
      this.User.username = this.username;
      this.User.userRole = this.userRole;
      this.user.isActive = this.isActive;

      try{

        axios.put('https://api.myjson.com/bins/r18jc/'+ this.User.id,this.User);
        alert("User has been updated");
        

      } catch(e) {
        
        alert("There was an issue, please try again.");
        console.log(e);
        

      }
      this.clicker = false;
    },

    chooseDirection() {
      
      if(this.User.id == null) { 
        
        this.addRecord();
      
      } else {
      
        this.updateRecord();
      
      }
    
    },

    deleteUser(id){

      if(confirm("Are you sure you want to delete this user?")){

        try{
          axios.delete("http://localhost:3000/DeleteUser/"+id);
          alert("User has been deleted");

        }catch(e) {

          console.log(e);
          alert("There was an error,please try again.");

        }
      } else {

        alert("Cancelled, user not deleted.");

      }
      
    }
  },
}
</script>

<style>
[type="checkbox"]:checked ~ .panel-wrap {
  transform: translateX(0%);
}


</style>