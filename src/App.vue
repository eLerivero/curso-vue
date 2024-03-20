<script>
import axios from "axios";
import Swal from "sweetalert2";
import '@fortawesome/fontawesome-free/css/all.css';
import Agregar from './components/Agregar.vue';

export default {
  components: {
    Agregar
  },
  data() {
    return {
      modalTitle: 'Agregar Registro',
      btnAgregar: true,
      ListaUsuario: [], // Define ListaUsuario dentro de data
    };
  },
  mounted() {
    this.Lista();
  },
  methods: {
    abrirModal(tipo, userData) {
      const myModal = new bootstrap.Modal(document.getElementById('exampleModal'));
      if (tipo === 'agregar') {
        this.modalTitle = 'Agregar Registro';
        this.btnAgregar = true;
      } else if (tipo === 'actualizar') {
        this.modalTitle = 'Actualizar Registro';
        this.btnAgregar = false;
      }
      myModal.show();
    },
    async agregarRegistro(registroAgregar) {
      console.log(registroAgregar)
    },
    Lista() {
      // Hacemos una petición a la API v1 de Nube Colectiva
      axios
        .get("https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user") // Agrega https:// antes de la URL
        .then((response) => {
          // Asignamos los datos obtenidos a ListaUsuario
          this.ListaUsuario = response.data;
        })
        .catch((error) => {
          // Si hubo algún error, lo mostramos
          console.log(error);
        })
        .finally(() => {
          // Se ejecutó sin problemas
          console.info("Finalizó");
        });
    },
    Delete(row) {
      Swal.fire({
        html: `<strong>¿Estás seguro de eliminar el usuario  </strong><strong class="text-danger">${row.nombres} ${row.apellidos}</strong> <strong>?</strong>`,
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#0A8020",
        cancelButtonColor: "#d33",
        confirmButtonText: "Sí, eliminarlo",
      }).then((result) => {
        if (result.isConfirmed) {
          axios
            .delete(`
              https://65f34f70105614e654a05799.mockapi.io/inea/api/v1/user/${row.id}`
            ) // Agrega https:// antes de la URL
            .then((response) => {
              this.ListaUsuario = [];
              Swal.fire({
                position: "top-end",
                icon: "success",
                html: `<strong class="text-success">El usuario ${row.nombres} ${row.apellidos} se elimino satisfactoriamente.</strong> `,
                showConfirmButton: false,
                timer: 1500,
              });
              this.Lista();
            })
            .catch((error) => {
              // Si hubo algún error, lo mostramos
              Swal.fire({
                position: "top-end",
                icon: "error",
                title: "Ocurrio un fallo",
                showConfirmButton: false,
                timer: 1500,
              });
            })
            .finally(() => {
              // Se ejecutó sin problemas
            });
        }
      });
    },
  },
};
</script>

<template>
  <agregar :modalTitle="modalTitle" :btnAgregar="btnAgregar" @registroAgregar="agregarRegistro">
  </agregar>

  <div class="container">
    <h1>LISTA DE USUARIOS</h1>
    <div class="btn-group" role="group">
      <button class="btn btn-success" type="button" @click="abrirModal('agregar', 'data')"><i
          class="fa-solid fa-user-plus"></i></button>
    </div>
    <hr />
  </div>

  <table id="example" class="table table-striped" style="width:100%">
    <thead>
      <tr>
        <th scope="col">id</th>
        <th scope="col">Nombre</th>
        <th scope="col">Apellido</th>
        <th scope="col">Sexo</th>
        <th scope="col">Email</th>
        <th scope="col">Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(user, index) in ListaUsuario" :key="index">
        <td scope="row">{{ user.id }}</td>
        <th>{{ user.nombres.toUpperCase() }}</th>
        <td>{{ user.apellidos }}</td>
        <td>{{ user.sexo }}</td>
        <td>{{ user.email }}</td>
        <td>
          <div class="btn-group" role="group">
            <button class="btn btn-sm btn-warning" @click="abrirModal('actualizar', user)"><i
                class="fa-solid fa-user-pen"></i></button>
            <button class="btn btn-sm btn-danger" @click="Delete(user)">
              <i class="fa-solid fa-trash"></i>
            </button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped></style>