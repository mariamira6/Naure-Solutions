<template>
  <div>
    <MenuAdmin />
    <div class="container-fluid d-flex flex-row w-100 p-0 m-0">
      <MenuLateral :opciones="opciones" />
      <div class="bg-light flex-grow-1 p-5">
        <div class="d-flex justify-content-between align-items-center mb-3" style="max-width: 100%;">
          <h4 class=" mb-5 text-center">Gestión de servicios</h4>
          <button class="btn btn-info text-white fw-bold" data-bs-toggle="modal" data-bs-target="#insertarModal">Crear
            nuevo
            servicio</button>
        </div>
        <table class="table table-light align-middle">
          <thead>
            <tr class="bg-info">
              <th scope="col">Nombre</th>
              <th scope="col">Descripción</th>
              <th scope="col" class="text-center">Imagen</th>
              <th scope="col" class="text-center">Acciones</th>
            </tr>
          </thead>
          <tbody class="table-group-divider">
            <tr v-for="servicio in servicios" :key="servicio.id">
              <td>{{ servicio.nombre }}</td>
              <td class="text-truncate" style="max-width: 500px;">{{ servicio.descripcion }}</td>
              <td class="text-center">
                <img :src="servicio.imagen" :alt="servicio.nombre" width="45" />
              </td>
              <td class="text-center">
                <button style="border: none;" data-bs-toggle="modal" data-bs-target="#editarModal"
                  @click="servicioSeleccionado = servicio"><img src="../assets/icons/editar.png" alt="Editar Servicio"
                    width="22" height="22" /></button>
                <button style="border: none;" data-bs-toggle="modal" data-bs-target="#eliminarModal"
                  @click="servicioSeleccionado = servicio"><img src="../assets/icons/eliminar.png"
                    alt="Eliminar Servicio" width="22" height="22" /></button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="modal fade" id="insertarModal" tabindex="-1" aria-labelledby="insertarModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content text-start">
            <div class="modal-header">
              <h5 class="modal-title" id="insertarModalLabel">Insertar nuevo servicio</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
            </div>
            <div class="modal-body">
              <form @submit.prevent="insertarServicio">
                <div class="mb-3">
                  <label for="nombre" class="form-label">Nombre del servicio</label>
                  <input type="text" class="form-control" id="nombre" required>
                </div>
                <div class="mb-3">
                  <label for="descripcion" class="form-label">Descripción del servicio</label>
                  <input type="text" class="form-control" id="descripcion" required>
                </div>
                <div class="mb-3">
                  <label for="iamgen" class="form-label">Imagen del servicio</label>
                  <input type="file" id="imagen" accept="image/*" @change="procesarImagen" />
                </div>
                <div class="container-fluid d-flex align-items-center gap-3">
                  <button type="submit" class="btn btn-primary text-center" data-bs-dismiss="modal">Guardar</button>
                  <button type="button" data-bs-dismiss="modal" class="btn btn-secondary text-center">Cancelar</button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
      <div class="modal fade" id="editarModal" tabindex="-1" aria-labelledby="editarModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content text-start">
            <div class="modal-header">
              <h5 class="modal-title" id="editarModalLabel">Editar servicio</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
            </div>
            <div class="modal-body">
              <form @submit.prevent="EditarServicio(servicioSeleccionado)">
                <div class="mb-3">
                  <label for="nombre" class="form-label">Nombre del servicio</label>
                  <input type="text" class="form-control" id="nombreServicio" :value="servicioSeleccionado.nombre"
                    required>
                </div>
                <div class="mb-3">
                  <label for="descripcion" class="form-label">Descripción del servicio</label>
                  <input type="text" class="form-control" id="descripcionServicio"
                    :value="servicioSeleccionado.descripcion" required>
                </div>
                <div class="mb-3">
                  <p>Imagen actual del servicio:</p>
                  <img :src="servicioSeleccionado.imagen" alt="Imagen Servicio" width="45" />
                </div>
                <div class="mb-3">
                  <label for="iamgen" class="form-label">Cambiar imagen del servicio</label>
                  <input type="file" id="imagen" accept="image/*" @change="procesarImagen" />
                </div>
                <div class="container-fluid d-flex align-items-center gap-3">
                  <button type="submit" class="btn btn-primary text-center" data-bs-dismiss="modal">Actualizar</button>
                  <button type="button" data-bs-dismiss="modal" class="btn btn-secondary text-center">Cancelar</button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
      <div class="modal fade" id="eliminarModal" tabindex="-1" aria-labelledby="eliminarModalLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content text-start">
            <div class="modal-header">
              <h5 class="modal-title" id="eliminarModalLabel">Eliminar servicio</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
            </div>
            <div class="modal-body">
              <p>Vas a proceder a eliminar un servicio. ¿Estás seguro/a?</p>
              <div class="d-flex gap-2 align-items-center">
                <button type="button" class="btn btn-primary text-center" data-bs-dismiss="modal"
                  @click="EliminarServicio(servicioSeleccionado.id)">
                  Sí
                </button>
                <button type="button" data-bs-dismiss="modal" class="btn btn-secondary text-center">
                  Cancelar
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MenuLateral from '../components/MenuLateral.vue';
import MenuAdmin from '../components/MenuAdmin.vue';
import { useToast } from 'vue-toast-notification';
import 'vue-toast-notification/dist/theme-bootstrap.css';

export default {
  components: { MenuAdmin, MenuLateral },
  data() {
    return {
      opciones: [
        { id: 1, label: 'Gestión de servicios', url: '/gestionservicios' },
        { id: 2, label: 'Gestión de clientes', url: '' },
        { id: 3, label: 'Contrataciones', url: '' },
        { id: 4, label: 'Análisis de datos', url: '/analisisdatos' }
      ],
      servicios: [],
      error: "",
      imagenBase64: "",
      servicioSeleccionado: {
        id: null,
        nombre: "",
        descripcion: "",
        imagen: ""
      }
    };
  },
  methods: {
    async listarServicios() {
      this.servicios = [];
      this.error = "";

      try {
        let respuesta = await fetch(`http://localhost:5259/api/Servicio/ListarServicios`);

        if (respuesta.ok) {
          let datos = await respuesta.json();
          this.servicios = datos.map(dato => ({
            id: dato.id,
            nombre: dato.nombre,
            descripcion: dato.descripcion,
            imagen: dato.imagen
          }));
        } else {
          throw new Error("Ha habido un error.");
        }
      } catch (e) {
        this.error = 'Error. ' + e.message;
      }
    },
    async insertarServicio() {
      this.error = "";
      const toast = useToast();
      let nombre = document.getElementById("nombre").value;
      let descripcion = document.getElementById("descripcion").value;

      if (this.imagenBase64) {
        let nuevoServicio = {
          nombre,
          descripcion,
          imagen: this.imagenBase64
        };

        const token = localStorage.getItem("token");

        try {
          let respuesta = await fetch("http://localhost:5259/api/Servicio/InsertarServicio", {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
              Token: token
            },
            body: JSON.stringify(nuevoServicio)
          });

          if (respuesta.ok) {
            document.getElementById("nombre").value = "";
            document.getElementById("descripcion").value = "";
            document.getElementById("imagen").value = "";
            this.imagenBase64 = "";

            this.listarServicios();
            toast.success("Servicio creado correctamente", {
              position: 'top-right',
              duration: 3000
            });
          } else {
            throw new Error("Error al insertar el servicio");
          }
        } catch (e) {
          this.error = "Error. " + e.message;
        }
      }
    },
    procesarImagen(event) {
      const archivo = event.target.files[0];
      if (!archivo) return;

      const reader = new FileReader();
      reader.onload = (e) => {
        this.imagenBase64 = e.target.result;
      };
      reader.readAsDataURL(archivo);
    },
    async EditarServicio(servicio) {
      this.error = "";
      const token = localStorage.getItem("token");
      const toast = useToast();
      let nombre = document.getElementById("nombreServicio").value;
      let descripcion = document.getElementById("descripcionServicio").value;
      let imagen = "";

      if (this.imagenBase64) {
        imagen = this.imagenBase64;
      } else {
        imagen = servicio.imagen;
      }

      let servicioActualizado = {
        id: servicio.id,
        nombre,
        descripcion,
        imagen
      };

      try {
        let respuesta = await fetch("http://localhost:5259/api/Servicio/ModificarServicio", {
          method: "PUT", headers: {
            "Content-Type": "application/json",
            Token: token
          },
          body: JSON.stringify(servicioActualizado)
        });

        if (respuesta.ok) {
          this.listarServicios();
          this.imagenBase64 = "";
          toast.success("Servicio actualizado correctamente", {
            position: 'top-right',
            duration: 3000
          });
        } else {
          throw new Error("Error al actualizar el servicio");
        }
      } catch (e) {
        this.error = "Error. " + e.message;
      }
    },
    async EliminarServicio(id) {
      this.error = "";
      const token = localStorage.getItem("token");
      const toast = useToast();

      try {
        let respuesta = await fetch(`http://localhost:5259/api/Servicio/EliminarServicio?id=${id}`, {
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
            Token: token
          }
        });

        if (respuesta.ok) {
          this.listarServicios();
          toast.success("Servicio eliminado correctamente", {
            position: 'top-right',
            duration: 3000
          });

        } else {
          throw new Error("Ha habido un error.");

        }
      } catch (e) {
        this.error = 'Error. ' + e.message;
      }
    }
  },
  mounted() {
    this.listarServicios();
  }
}
</script>

<style></style>
