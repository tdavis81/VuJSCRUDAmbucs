<template>
  <div >
  <input id="clicker" v-model="clicker" type="checkbox" style="display:none" />

    <h1 style="margin-top:15px" align="center">Record Awards</h1>
    <input style="width:25%" type="submit" v-on:click="ShowSideBar(-999)" value="Add Record"/>
    <hr style="width:85%; border-width:thick"/>

    <div class="row" id="records">
      
      <div  align="center" style="margin-bottom:35px" class="col-sm-4" v-for="Record in Records" :key="Record.id">
      
        <div class="card">
 
          <div v-on:click="ShowSideBar(Record.id)">

            <img src="@/assets/logo.png" alt="Avatar" style="width:100%">
        
            <div class="container">
            
              <h4><b>{{Record.firstName}} {{Record.lastName}}</b></h4> 
              <h5>{{Record.address}}</h5> 

            </div>

          </div>

        </div>
      
      </div>
    
    </div>  


    <!--Side Panel-->
    <div class="panel-wrap">
    
      <div class="panel">
      
        <h4 class="context-menu exit" v-on:click="exit()" style="float:right">X</h4>
        <h3>{{Title}}</h3>
        <input v-model="firstName" type="text" placeholder="First Name" /> <br/>
        <input v-model="lastName" type="text" placeholder="Last Name" /> <br/>
        <input v-model="birthDate" type="text" placeholder="Birth Date" /> <br/>
        <input v-model="giftedDate" type="text" placeholder="Gifted Date" /> <br/>
        <input v-model="address" type="text" placeholder="Address" /> <br/>
        <input v-model="city" type="text" placeholder="City" /> <br/>
        <input v-model="state" type="text" placeholder="State" /> <br/>
        <input v-model="zipCode" type="text" placeholder="Zip" /> <br/>
        <input  type="submit" value="Save" v-on:click="chooseDirection()"/> <br/> 

      </div>
    
    </div>

    
  </div>

</template>



<script>
// @ is an alias to /src
//import HelloWorld from '@/components/Navbar.vue'
import axios from 'axios';

const url = axios.create({
  baseURL: 'http://localhost:3000/',
  json: true
})

export default {
  name: 'home',
  
   data: function() {
    
    return  {
      
      Records: [],
      firstName: "",
      lastName: "",
      birthDate: "",
      giftedDate: "",
      address: "",
      city:"",
      state:"",
      zipCode:"",
      Title:"",
      clicker: "",
      Records: {}

    }
  },

  created () {
   
    axios.get('https://api.myjson.com/bins/1a1l40')
      .then((response) => { // Use the arrow notation here for vues this < while using function here this is in function not vue instance and the arrow notation fixes that 
       this.Records = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },

  methods:{
    
    chooseDirection() {
      
      if(this.User.id == null) { 
        
        this.AddRecord();
      
      } else {
      
        this.UpdateRecord();
      
      }
    
    },

    AddRecord() {
    
      var newRecord = {
        firstName: this.firstName,
        lastName: this.lastName,
        birthDate: this.birthDate,
        giftedDate: this.giftedDate,
        address: this.address,
        city:this.city,
        state:this.state,
        zipCode:this.zipCode,
      }

      axios.post( url + 'AddRecord', newRecord);

    },

    UpdateRecord() {
      
      this.Record.firstName = this.firstName; 
      this.Record.lastName=this.lastName;
      this.Record.birthDate= this.birthDate;
      this.Record.giftedDate = this.giftedDate;
      this.Record.address = this.address;
      this.Record.city = this.city;
      this.Record.state = this.state;
      this.Record.zipCode = this.zipCode;

      try{

        axios.put('https://api.myjson.com/bins/r18jc/'+ this.User.id,this.User);
        alert("User has been updated");
        

      } catch(e) {
        
        alert("There was an issue, please try again.");
        console.log(e);
        

      }
      this.clicker = false;
    
    },
    
    ShowSideBar(id) {
      
      this.clicker = true;
      document.getElementById("records").style.marginRight = "375px";

      if(id == -999) {
        
        this.Title = "Add Record";
      
      } else {

        this.Title = "Edit / View Record";
        this.Record = this.Records.find(k => k.id == id);
        this.firstName = this.Record.firstName;
        this.lastName = this.Record.lastName;
        this.birthDate = this.Record.birthDate;
        this.giftedDate = this.Record.giftedDate;
        this.address = this.Record.address;
        this.city = this.Record.city;
        this.state = this.Record.state;
        this.zipCode = this.Record.zipCode;

      }
  
    },
  
    exit() {

      this.clicker = false;
      document.getElementById("records").style.marginRight = "0px";
    
    }
  
  },
  
  

  components: {
    //Navbar
  }

}
</script>

<style>

.exit:hover {
    font-weight:bold;
    color:red;
}

.context-menu {cursor: context-menu;}

input[type=text], select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type=submit] {
  width: 100%;
  background-color: #4CAF50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type=submit]:hover {
  background-color: #45a049;
}

.panel-wrap {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  width: 30em;
  transform: translateX(100%);
  transition: .3s ease-out;
}
.panel {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: #333;
  color: #eee;
  overflow: auto;
  padding: 1em;
}

[type="checkbox"]:checked ~ .panel-wrap {
  transform: translateX(0%);
}


.card {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  width: 40%;
}

.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

.container {
  padding: 2px 16px;
}
</style>
