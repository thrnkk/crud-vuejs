<template>
    <div id="deleteSeveralEmployeesModal" class="modal fade">

		<div class="modal-dialog">

			<div class="modal-content">

				<div class="modal-header">						

					
					<h4 class="modal-title">Excluir Empregados</h4>

					<button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>

				</div>

				<div class="modal-body">					

					<p>Você tem certeza que deseja excluir estes {{selectedEmployeeIds.length}} registro(s) selecionado(s)?</p>

					<p class="text-warning"><small>Essa ação não pode ser desfeita.</small></p>

					<span style="color: #00C851">{{message}}</span>

				</div>

				<div class="modal-footer">

					<input type="button" class="btn btn-default" data-dismiss="modal" value="Cancelar">

					<input type="submit" class="btn btn-danger" @click="submitForm" value="Excluir">

				</div>
					
			</div>

		</div>

	</div>
</template>

<script>

export default {
  	props: {selectedEmployeeIds: Array},
	data() {
		return {

			message: ''

		};
	},
	watch: {

		selectedEmployeeIds: function(employees) {

			if(employees.length > 0) {

				this.message = ''

			}
		
		}

	},
	methods: {

		submitForm() {

			this.selectedEmployeeIds.forEach(function (id) {

				axios.delete('http://rest-api-employees.jmborges.site/api/v1/delete/' + id)
				.then(response => {

				})				

			});
			
			setTimeout(() => (this.$parent.refresh(), this.message = this.selectedEmployeeIds.length + ' registro(s) excluído(s) com sucesso!'), 2000);

		},

  	}
};
</script>