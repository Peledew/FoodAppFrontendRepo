<template>
<div>
    <h2 style="text-align:center">Employee Edit</h2>

        <!-- <img alt="User" src="../assets/user.png"/> -->

        <label for="firstName">First name:</label>
        <input v-model="employee.firstName" /><br /> <br/>
        <label for="lastName">Last name:</label>
        <input v-model="employee.lastName" /><br /><br/>
        <label for="position">Position:</label>
        <input v-model="employee.position" /><br /><br/>
        <button v-on:click="editEmployee()">Azuriraj</button>

</div>
</template>
<script>
//import { createDecipheriv } from 'crypto';

//import axios from "axios";
export default {
    name: 'EmployeeEdit',
    data: function() {
        return {
            employee: {},
        }
    },

    //dobavljam podatke o selektovanom korisniku
    mounted: function() {
        fetch('http://localhost:8081/api/employees/' + this.$route.query.id)
            .then(response => response.json())
            .then(data => {console.log("Success:", data); this.employee = data})
            .catch((error) => {
                console.error("Error:", error);
            });
    },
    

    methods: {
     /* editEmployee: function (employee) {
      fetch("http://localhost:8081/api/employees/update-licni-podaci", {
        method: 'PUT',
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
        body: JSON.stringify(this.employee),
      })
        .then((response) => response.json)
         .then((data) => {
          console.log("Odje: " + data);
          this.$router.push("/employees");
        }) 
        //.then(data => (this.updatedAt = data.updatedAt))
        .catch((err) => {
          console.log("Error : " + err);
          alert(err);
        });
    },  */

    //  editEmployee : function(employee){
    //     const requestOptions = {
    //         methods: 'PUT',
    //         headers: {
    //             'Accept': "application/json",
    //             'Content-type': "application/json",
    //         },

    //         body: JSON.stringify(this.employee)

    //     };

    //     fetch('http://localhost:8081/api/employees', requestOptions)
    //     .then(response => response.json())
    //     .then(data => (this.updatedAt = data.updatedAt))
    //     .then(data =>{
    //         console.log("Pokusaj" + data);
    //     })


    //} 

   // editEmployee : function(){
   //   console.log(this.employee)
    //},

    async fetchEmployee(){
      const res = await fetch('http://localhost:8081/api/employees/' + this.$route.query.id)
      const data = await res.json()

      console.log(data)
      return data
    },

    async editEmployee(){
      //const res = await fetch('http://localhost:8081/api/employees/' + this.$route.query.id)
      //const data = await res.json()

      //const updEmployee = {...res, firstName: }
      
      /* const updatedEmployee = await fetch('http://localhost:8081/api/employees/' + this.$route.query.id)
      updatedEmployee.firstName = this.employee.firstName
      updatedEmployee.lastName = this.employee.lastName
      updatedEmployee.position = this.employee.position

      console.log(updatedEmployee)

      const res = await fetch('http://localhost:8081/api/employees/' + id, {
        method: 'PUT',
        headers: {
          'Content-type' : 'application/json'
        },
        body: JSON.stringify(updatedEmployee)
      })

      const data = await res.json() */

      fetch("http://localhost:8081/api/employees/" + this.$route.query.id, {
        method: "PUT",
        headers: {
          Accept: "application/json",
          "Content-type": "application/json",
        },
        body: JSON.stringify(this.employee),
      })
        .then((response) => response.json)
        .then((data) => {
          console.log("Success : " + data);
          this.$router.push("/employees");
        })
        .catch((err) => {
          console.log("Error : " + err);
          alert(err);
        });


    }




  },
}
</script>
