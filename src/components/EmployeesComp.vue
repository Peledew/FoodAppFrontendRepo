<template>
  <tr>
    <td>{{ employee.firstName }}</td>
    <td>{{ employee.lastName }}</td>
    <td>{{ employee.position }}</td>
    <td>
      <button class="btnSeeMore" v-on:click="seeMore">See more</button>
    </td>

    <td>
      <button class="delete" v-on:click="deleteEmployee">
        Delete
      </button>
    </td>

    <td>
      <button class="btnEdit" v-on:click="editEmploye">
        Edit
      </button>
    </td>
  </tr>
</template>

<script>
export default {
  name: "EmployeeComp",
  props: ["employee"],
  methods: {
    seeMore: function () {
      this.$router.push("/employee?id=" + this.employee.id);
    },

    editEmploye: function () {
      this.$router.push("/azuriranje?id=" + this.employee.id);
    },

    deleteEmployee: function() {
      fetch("http://localhost:8081/api/employees/" + this.employee.id, {
        method: "DELETE",
      }).then((res) => {
        if (res.ok) {
          window.location.reload();
        }
      });
    },
  },
};
</script>


<style>
</style>
