<template>
  <v-container>
    <div class="d-flex flex-row-reverse">
      <v-btn depressed color="primary" @click="openForm()"> เพิ่มข้อมูล </v-btn>
    </div>

    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">#</th>
            <th class="text-left">Fullname</th>
            <th class="text-left">Phone Number</th>
            <th class="text-left">Update</th>
            <th class="text-left">Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item,index) in posts" :key="item.name">
            <td>{{ index+1 }}</td>
            <td>{{ item.fullname }}</td>
            <td>{{ item.phone_number }}</td>
            <td><a @click="openForm(item.id)">update</a></td>
            <td><a @click="Delete(item.id)">delete</a></td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      posts: [
       
      ],
    };
  },
  methods:{
    getAll(){
      axios
      .get(`https://node-crud.onrender.com/customer`)
      .then((response) => {
        // JSON responses are automatically parsed.
        this.posts = response.data.data
      })
      .catch((e) => {
        this.errors.push(e);
      });
    },
    openForm(_id=null){
      // console.log(_id);
      if(_id){
        this.$router.push({ path:`/form/${_id}` });
      }else{
        this.$router.push({ path:`/form` });
      }
    },
    Delete(_id=null){
      axios
      .delete(`https://node-crud.onrender.com/customer/${_id}`)
      .then(() => {
         this.getAll();
      })
      .catch((e) => {
        this.errors.push(e);
      });
    }
  },
  mounted() {
    this.getAll();
  },
};
</script>
