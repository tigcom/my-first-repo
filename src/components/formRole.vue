<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios';
const db = ref(null)  
    onMounted(getRole);
    
    function checkAuth(name, idrole) {
      return db.value.auth.findIndex(au =>
        au.account.fullname === name && au.role.id === idrole
      );
    }
    const authHeader = {
      headers: {
        Authorization: "Basic a2hvYTptYXRraGF1MQ=="
      }
    }
    function getRole() {
      axios.get("http://localhost:1124/rest/role",authHeader)
      .then(function(response) {
        console.log(response.data)
        db.value =response.data;
        console.log(db);
      
      })
      .catch(function(error) {
        console.error(error)
      })
    }
    function  post(auth) {
      console.log(auth);
      
      axios.post("http://localhost:1124/rest/role/add",auth)
      .catch(function(error) {
        console.error(error)
      })
    }
    function  deleteAuth(auth) {
      console.log(auth);
      axios.delete("http://localhost:1124/rest/role/delete",{
    data: auth
  })
      .catch(function(error) {
        console.error(error)
      })
    }
    function update(name,idRole) {
      const ob = {
        username: name,
        roleId: idRole
        }
      if (checkAuth(name, idRole) > -1){
      this.deleteAuth(ob);
      }else{
        
        this.post(ob);
      }
    }
</script>

<template>
    <table  v-if="db">
      <thead>
        <tr>
          <th>Name</th>
          <th v-for="r in db.role">{{ r.name }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="ac in db.acc">
          <td>{{ ac.fullname }}</td>
          <td  v-for="r in db.role">
            <input type="checkbox" @click="update(ac.fullname, r.id)"  :checked="checkAuth(ac.fullname, r.id) > -1"  name="" id="">
          </td>
        </tr>
      </tbody>
    </table>
</template>

<style scoped>

</style>
