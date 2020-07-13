<template>
    <div id="editEmployeeModal" class="modal fade" v-if="this.selectedEmployee">

		<div class="modal-dialog">

			<div class="modal-content">

				<form @submit.prevent="submitForm">	

					<div class="modal-header">						

						<h4 class="modal-title">Editar {{this.selectedEmployee.employee_name}}</h4>

						<button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>

					</div>

					<div class="modal-body">					

						<div class="form-group">

							<label>Nome</label>

							<input type="text" class="form-control" v-model="name" required>

						</div>

						<div class="form-group">

							<label>Idade</label>

							<input type="number" class="form-control" v-model="age" required>

						</div>

						<div class="form-group">

							<label>Salário</label>

							<input type="number" step="any" class="form-control" v-model="salary" required>

						</div>

						<div class="form-group">

							<label>Imagem de Perfil</label>

							<input type="text" class="form-control" placeholder="http://" v-model="image" required>

						</div>		

						<span style="color: #00C851">{{message}}</span>		

					</div>

					<div class="modal-footer">

						<input type="button" class="btn btn-default" data-dismiss="modal" value="Cancelar">

						<input type="submit" class="btn btn-success" value="Salvar">

					</div>

				</form>

			</div>

		</div>

	</div>
</template>

<script>

export default {
	props: {selectedEmployee: Object},
	data() {
		return {

			id: '',
			name: '',
			age: '',
			salary: '',
			image: '',
			message: ''

		};
	},
	watch: {

		selectedEmployee: function(employee) {

			if(this.id != employee.id) {

				this.id = employee.id,
				this.name = employee.employee_name,
				this.age = employee.employee_age,
				this.salary = employee.employee_salary,
				this.image = employee.profile_image

			}
		
		}

	},
	methods: {

		submitForm() {

			axios.put('http://rest-api-employees.jmborges.site/api/v1/update/' + this.id, {

				name: this.name,
				salary: this.salary,
				age: this.age,
				profile_image: this.image

			}).then(response => {

				this.message = 'O usuário ' + this.selectedEmployee.employee_name + ' foi alterado com sucesso!'

				this.$parent.refresh()

			})

		},

  	}
  
};
</script>