<template>
    <div id="deleteEmployeeModal" class="modal fade">

		<div class="modal-dialog">

			<div class="modal-content">

				<div class="modal-header">						
					
					<h4 class="modal-title">Excluir {{name}}</h4>

					<button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>

				</div>

				<div class="modal-body">					
		
					<p> Deseja mesmo excluir os dados de {{name}} </p>

					<p class="text-warning"><small>Essa ação não pode ser desfeita.</small></p>

					<span style="color: #00C851">{{message}}</span>

				</div>

				<div class="modal-footer">

					<input type="button" class="btn btn-default" data-dismiss="modal" value="Cancelar">

					<button type="submit" class="btn btn-danger" @click="submitForm" value="Excluir">Excluir</button>

				</div>
					
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

			axios.delete('http://rest-api-employees.jmborges.site/api/v1/delete/' + this.id)
			.then(response => {

				this.message = 'O usuário ' + this.name + ' foi excluído com sucesso!'

				this.$parent.refresh()

			})

			console.log(this.$parent)

			

		},

  	}
};
</script>