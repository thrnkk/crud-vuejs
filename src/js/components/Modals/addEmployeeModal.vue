<template>
    <div id="addEmployeeModal" class="modal fade">

		<div class="modal-dialog">

			<div class="modal-content">

				<form @submit.prevent="submitForm">		

					<div class="modal-header">						

						<h4 class="modal-title">Adicionar Empregado</h4>

						<button type="button" class="close" data-dismiss="modal" @click="resetForm" aria-hidden="true">&times;</button>

					</div>

					<div class="modal-body">	

						<div class="form-group">

							<label>Nome</label>

							<input id="addName" type="text" class="form-control" v-model="name" required autocomplete="off">

						</div>

						<div class="form-group">

							<label>Idade</label>

							<input id="addAge" type="number" class="form-control" v-model="age" required autocomplete="off">

						</div>

						<div class="form-group">

							<label>Salário</label>

							<input id="addSalary" type="number" step="any" class="form-control" v-model="salary" required autocomplete="off">

						</div>

						<div class="form-group">

							<label>Imagem de Perfil</label>

							<input id="addProfileImage" type="text" class="form-control" placeholder="http://" v-model="image" required autocomplete="off">

						</div>	

						<span style="color: #00C851">{{message}}</span>
						
					</div>

					<div class="modal-footer">

                        <input type="button" @click="resetForm" class="btn btn-default" data-dismiss="modal" value="Cancelar">
                        
						<button type="submit" value="submit" class="btn btn-success" >Adicionar</button>

					</div>

				</form>

			</div>

		</div>

	</div>
</template>

<script>

export default {
	data() {
		return {
			name: '',
			age: '',
			salary: '',
			image: '',
			message: ''
		};
	},
	methods: {

		submitForm() {

			console.log(this.name, this.age, this.salary, this.image)

			axios.post('http://rest-api-employees.jmborges.site/api/v1/create', {

				name: this.name,
				salary: this.salary,
				age: this.age,
				profile_image: this.image

			}).then(response => {

				this.message = 'O usuário ' + this.name + ' foi inserido com sucesso!'

				this.$parent.refresh()

			})

		},

		resetForm() {

			this.name = ''
			this.age = ''
			this.salary = ''
			this.image = ''
			this.message = ''

		}

  	}
};
</script>