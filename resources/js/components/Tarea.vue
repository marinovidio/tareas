<template>
	<div>
		<h1 class="text-center">Tareas asignadas</h1>	
		<hr>

		<!-- Button trigger modal -->
			<button @click="update=false; openModal();" type="button" class="btn btn-primary">
			  Nueva Tarea
			</button>

			<!-- Modal -->
			<div class="modal" :class="{show:modal}">
			  <div class="modal-dialog">
			    <div class="modal-content">
			      <div class="modal-header">
			        <h5 class="modal-title">{{titleModal}}</h5>
			        <button @click="closeModal();" type="button" class="close" data-dismiss="modal" aria-label="Close">
			          <span aria-hidden="true">&times;</span>
			        </button>
			      </div>
			      <div class="modal-body">
			       	<div>
			       		<label for="descripcion">Descripción</label>
			       		<input v-model="tarea.descripcion" type="text" class="form-control" id="descripcion" placeholder="descripción de la tarea" name="">
			       	</div>
			       	<div>
			       		<label for="fecha_limite">Fecha limite</label>
			       		<input v-model="tarea.fecha_limite" type="text" class="form-control" id="fecha_limite" placeholder="Fecha limite de la tarea" name="">
			       	</div>
			       	<div>
			       		<label for="trabajador">Trabajador asignado</label>
			       		<input v-model="tarea.trabajador" type="text" class="form-control" id="trabajador" placeholder="Trabajador asigando" name="">
			       	</div>
			      </div>
			      <div class="modal-footer">
			        <button @click="closeModal();" type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>
			        <button @click="save();" type="button" class="btn btn-success">Guardar Cambios</button>
			      </div>
			    </div>
			  </div>
			</div>
		<table class="table table-striped table-hover">
		  <thead class="thead-dark">
		    <tr>
		      <th scope="col">Id</th>
		      <th scope="col">Descripción</th>
		      <th scope="col">Fecha limite</th>
		      <th scope="col">Trabajador asignado</th>
		      <th scope="col" colspan="2" class="text-center">Acción</th>
		    </tr>
		  </thead>
		  <tbody>
		    <tr v-for="tarea in tareas" :key="tarea.id">
		      <th scope="row">{{ tarea.id }}</th>
		      <td>{{ tarea.descripcion }}</td>
		      <td>{{ tarea.fecha_limite }}</td>
		      <td>{{ tarea.trabajador }}</td>
		      <td>
		      	<button @click="update=true; openModal(tarea);" class="btn btn-warning">Editar</button>
		      </td>
		      <td>
		      	<button @click="eliminar(tarea.id)" class="btn btn-danger">Eliminar</button>
		      </td>
		    </tr>
		  </tbody>
		</table>
	</div>
</template>

<script>
	export default {
		data () {
			return {
				tarea: {
					descripcion:'',
					fecha_limite:'',
					trabajador:''
				},

				id:0,
				update:true,
				modal:0,
				titleModal:'',
				tareas:[],
			}
		},
		methods: {
			async list() {
				const res = await axios.get('tareas');

				this.tareas = res.data;
			},
			async eliminar(id) {
				const res = await axios.delete('/tareas/' +id);
				this.list();
			},
			async save() {
				if (this.update) {
					const res = await axios.put('/tareas/' + this.id, this.tarea);

				}else{
					const res = await axios.post('/tareas', this.tarea);
				}
				this.closeModal();
				this.list();
			},
			openModal(data={}) {
				this.modal=1;
				if (this.update) {
					this.id = data.id,
					this.titleModal="Modificar Tarea";
					this.tarea.descripcion = data.descripcion;
					this.tarea.fecha_limite = data.fecha_limite;
					this.tarea.trabajador = data.trabajador;
				
				}else
				{
					this.id=0,
					this.titleModal="Crear Tarea";
					this.tarea.descripcion = '';
					this.tarea.fecha_limite = '';
					this.tarea.trabajador = '';
				}
			},
			closeModal() {
				this.modal=0;
			},

		},
		created() {
			this.list();
		},

	}
</script>

<style>
.show {
	display: list-item;
	opacity:1;
	background: rgba(44, 38, 75, 0.849);
}	
</style>