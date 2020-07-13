<template>
  <!-- Container Tabela -->
	<div class="container">

		<div class="table-wrapper">

			<div class="table-title">

				<div class="row">
					
					<div class="col-sm-3">

						<h2></h2>

					</div>

					<div class="col-sm-9">

						<a href="#addEmployeeModal" class="btn btn-outline-success" data-toggle="modal"><i class="material-icons">&#xE147;</i> <span>Adicionar Novo Empregado</span></a>

						<a href="#deleteSeveralEmployeesModal" class="btn btn-outline-danger" data-toggle="modal"><i class="material-icons">&#xE15C;</i> <span>Excluir Selecionados ({{selectedEmployeeIds.length}})</span></a>						

						<a href="#refreshedModal" @click="refresh" class="btn btn-outline-info" data-toggle="modal"><i class="material-icons">&#xE5D5;</i> <span>Atualizar Informações</span></a>

					</div>

				</div>

			</div>

			<table class="table table-striped table-hover">

				<thead>

					<tr>

						<th>

							<span class="custom-checkbox">

								<input type="checkbox" id="selectAll" @click="selectAllEmployees">

								<label for="selectAll"></label>

							</span>

						</th>

						<th colspan="2">Nome</th>

						<th>Idade</th>

						<th>Salário</th>

						<th></th>

					</tr>

				</thead>

				<tbody>

                    <tr v-for="employee in this.employees" :key="employee.id">

                        <td>
							
                            <span class="custom-checkbox">
                                <input type="checkbox" :id="employee.id" name="options[]" :value="employee.id" v-model="selectedEmployeeIds">
                                <label :for="employee.id"></label>
                            </span>

                        </td>

                        <td style="width: 40px">
    
                            <span class="custom-profile-image">
                            	<img :src="((employee.profile_image != null && employee.profile_image.substring(0, 4) == 'http') ? employee.profile_image : 'https://www.fiscalti.com.br/wp-content/uploads/2020/05/default-user-image-365x365.png')">
                            </span>
    
                        </td>
    
                        <td data-nome="teste">
							
							{{employee.employee_name}}
							
						</td>

                        <td>
							
							{{employee.employee_age}}
							
						</td>

                        <td> 
							
							R$ {{(employee.employee_salary ? employee.employee_salary.toFixed(2) : (0).toFixed(2))}} 
							
						</td>
    
                        <td>
                            <a href="#editEmployeeModal" @click="selectEmployee(employee)" class="edit" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Editar">&#xE150;</i></a>
                            <a href="#deleteEmployeeModal" @click="selectEmployee(employee)" class="delete" data-toggle="modal"><i class="material-icons" data-toggle="tooltip" title="Excluir">&#xE872;</i></a>
                        </td>

                    </tr>

				</tbody>

			</table>

		</div>

    <addEmployeeModal></addEmployeeModal>

	<deleteEmployeeModal :selected-employee="this.selectedEmployee"></deleteEmployeeModal>

	<deleteSeveralEmployeesModal :selected-employee-ids="this.selectedEmployeeIds"></deleteSeveralEmployeesModal>

	<editEmployeeModal :selected-employee="this.selectedEmployee"></editEmployeeModal>

	<refreshedModal></refreshedModal>
    
	</div>


</template>

<script>

import Vue from "vue";
import addEmployeeModal from "./components/Modals/addEmployeeModal";
import deleteEmployeeModal from "./components/Modals/deleteEmployeeModal";
import deleteSeveralEmployeesModal from "./components/Modals/deleteSeveralEmployeesModal";
import editEmployeeModal from "./components/Modals/editEmployeeModal";
import refreshedModal from "./components/Modals/refreshedModal";

export default Vue.extend({
	data() {

		return {

			employees: null,
			selectedEmployee: null,
			selectedEmployeeIds: []

		}

	},
	components: {

		addEmployeeModal, deleteEmployeeModal,
		deleteSeveralEmployeesModal, editEmployeeModal,
		refreshedModal

	},
	mounted() {

		console.log('refresh')

		this.refresh()
		
	},

	methods: {

		refresh() {

			axios.get('http://rest-api-employees.jmborges.site/api/v1/employees').then(response => {
				this.employees = response.data.data

				this.selectedEmployee  = null,
				this.selectedEmployeeIds  = []

			})

		},

		selectEmployee(employee) {

			this.selectedEmployee = employee

		},

		selectAllEmployees() {

			var selected

			// Limpa se todos estiverem selecionados
			if (this.selectedEmployeeIds.length == this.employees.length) {

				selected = []

			// Seleciona todos
			} else {

				selected = []

				this.employees.forEach(function (employee) {

					selected.push(employee.id);

				});

			}

			this.selectedEmployeeIds = selected

		}

	}
});
</script>