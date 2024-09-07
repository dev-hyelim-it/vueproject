<template>
	<div v-if="isVisible" class="modal_overlay" @click.self="close">
		<div class="modal_content">
			<h2>신규 고객 등록</h2>
			<form @submit.prevent="addCustomer">
				<div class="form_group">
					<label for="cus_name">*고객명:</label>
					<input v-model="newCustomer.cus_name" type="text" id="cus_name" required /><br><br>
				</div>

				<div class="form_group">
					<label for="jumin_id">*실명번호:</label>
					<input v-model="newCustomer.jumin_id" type="text" id="jumin_id" required /><br><br>
				</div>

				<div class="form_group">
					<label for="email">*E-mail:</label>
					<input v-model="newCustomer.email" type="email" id="email" required /><br><br>
				</div>

				<div class="form_group">
					<label for="tel">전화번호:</label>
					<input v-model="newCustomer.tel" type="text" id="tel" /><br><br>
				</div>

				<div class="form_group">
					<label for="phone">*핸드폰 번호:</label>
					<input v-model="newCustomer.phone" type="text" id="phone" required /><br><br>
				</div>

				<div class="form_group">
					<label for="job">*직 업:</label>
					<input v-model="newCustomer.job" type="text" id="job" required /><br><br>
				</div>

				<div class="form_group">
					<label for="addr">주 소:</label>
					<input v-model="newCustomer.addr" type="text" id="addr" /><br><br>
				</div>

				<div class="form_actions">
					<button type="submit" class="btn_submit">등록</button>
					<button type="button" class="btn_cancel" @click="close"> 취소 </button>
				</div>
			</form>
		</div>
	</div>
</template>

<script>
export default {
	name:'NewCustomer',
	props : {
		isVisible: {
		type: Boolean,
		required: true
		},
		// validate: {
		// 	type: Function,
		// 	required: true
		// }
	},
	data() {
		return {
		newCustomer: {
			cus_name: '',
			jumin_id: '',
			email: '',
			tel: '',
			phone: '',
			job: '',
			addr: '',
		}
		};
	},
	methods: {
		addCustomer() {
			// 입력값 유효성 검사 예시
			// if (this.validate(this.newCustomer)) {
			// 	alert("ok");
			// } else {
			// 	alert("안돼요):");
			// 	return; // 유효성 검사 실패 시 메서드 종료
			// }
			this.$emit('add-customer', this.newCustomer);
			this.newCustomer = {
				cus_name: '',
				jumin_id: '',
				email: '',
				tel: '',
				phone: '',
				job: '',
				addr: '',
			};
			this.close();
		},
		close() {
		this.$emit('close');
		}
	}
};
</script>

<style scoped>
.modal_overlay {
position: fixed;
top: 0;
left: 0;
width: 100%;
height: 100%;
background: rgba(0, 0, 0, 0.5);
display: flex;
justify-content: center;
align-items: center;
z-index: 1000;
}
.modal_content {
background: white;
padding: 20px;
border-radius: 8px;
box-shadow: 0, 4px, 8px rgba(0, 0, 0, 0.2);
width: 400px;
box-sizing: border-box;
}
h2 {
margin-top: 0;
margin-bottom: 20px;
font-size: 1.5em;
text-align: center;
}
.form_group {
margin-bottom: 15px;
}
.form_group label {
display: block;
font-weight: bold;
margin-bottom: 5px;
}
.form_group input {
width: 100%;
padding: 8px;
border: 1px solid #ccc;
border-radius: 4px;
box-sizing: border-box;
}
.form_actions {
text-align: center;
}
.btn_submit, .btn_cancel {
padding: 10px 15px;
border: none;
border-radius: 4px;
cursor: pointer;
font-size: 16px;
}
.btn_submit {
background-color: rgb(129, 164, 230);
color: white;
margin-right: 10px;
}
.btn_submit:hover {
background-color: #0056b3;
}
.btn_cancel {
background-color: #a8abad;
color: white;
}
.btn_cancel:hover {
background-color: #5a6268;
}
</style>
