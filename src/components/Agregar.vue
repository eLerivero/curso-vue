<script>
import axios from "axios";
import Swal from "sweetalert2";

export default {
  props:{
    modalTitle: {
        type: String,
        required: true
    },
    btnAgregar: {
        type: Boolean,
        required: true
    }
  },
  data() {
    return {
      btnActivo: true,
      formData: {
        nombres: "",
        apellidos: "",
        email: "",
        sexo: "",
      },
    };
  },
  methods: {
  capturarBoton() {
    const formDataCopy = { ...this.formData };
    //condicion para capturar el estado del boton: true:agregar, false:actualizar
    if (this.btnAgregar) {
        this.$emit('registroAgregar', formDataCopy)
    } else {
      
    }
  },
},
};
</script>

<template>
  <!-- Modal -->
  <div
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">
            {{ modalTitle }}
          </h1>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
    <form @submit.prevent="capturarBoton">
        <div class="modal-body">
            <div class="mb-3">
              <label class="form-label">Nombres:</label>
              <input type="text" class="form-control" id="nombres" v-model="formData.nombres"/>
            </div>

            <div class="mb-3">
              <label class="form-label">Apellidos:</label>
              <input type="text" class="form-control" id="apellidos" v-model="formData.apellidos"/>
            </div>

            <div class="mb-3">
              <label class="form-label">Correo:</label>
              <input type="text" class="form-control" id="email" v-model="formData.email"/>
            </div>

            <div class="mb-3">
              <label class="form-label">Sexo:</label>
              <input type="text" class="form-control" id="sexo" v-model="formData.sexo"/>
            </div>

        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-danger"
            title="cerrar"
            data-bs-dismiss="modal"
          >
          <i class="fa-solid fa-rectangle-xmark"></i>
          </button>
          <button type="submit" class="btn btn-success" v-if="btnAgregar" title="Guardar"><i class="fa-solid fa-floppy-disk"></i></button>
          <button type="submit" class="btn btn-warning" v-else title="Editar Usuario"><i class="fa-solid fa-user-pen"></i></button>
        </div>
    </form>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
