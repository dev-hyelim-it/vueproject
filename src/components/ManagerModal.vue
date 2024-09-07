<template>
	<div v-if="isVisible" class="modal_overlay" @click.self="close">
	<div class="modal_content">
		<h2> 관리 담당자 선택 </h2>
		<ul>
			<li v-for="manager in managers" :key="manager.m_id">
				<button @click="selectManager(manager)">{{ manager.m_name }} - {{ manager.dept }}</button>
				</li>
			</ul>
			<button class="close_button" @click="close">등록 안함</button>
	</div>
	</div>
</template>

<script>
export default {
	name : 'ManagerModal',
	props: {
		isVisible: Boolean,
		managers: Array,
	},
	data() {
		return {
			newManager: {
				m_id: null,
				m_name: '',
				dept: '',
				position: '',
				phone: '',
			}
		}
	},
	methods: {
		selectManager(manager) {
			this.$emit('select-manager', manager);
		},
		close() {
			this.$emit('close');
		}
	}
}
</script>

<style scoped>
.modal_overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
	background: rgba(0, 0, 0, 0.5);
	z-index: 1000;
}
.modal_content {
	background: white;
	padding: 20px;
	border-radius: 8px;
	width: 400px;
	text-align: center;
}
h2 {
	margin-bottom: 20px;
	font-size: 24px;
	color: black;
}
ul {
	padding: 0;
}
ul li {
	margin-bottom: 10px;
}
button {
	background: rgb(129, 164, 230);
	color: white;
	border: none;
	padding: 10px 15px;
	border-radius: 8px; /* 버튼 모서리 둥글게 */
	font-size: 16px; /* 버튼 폰트 크기 조정 */
	cursor: pointer;
	transition: background-color 0.3s ease; /* 배경색 전환 효과 */
	width: 100%; /* 버튼 너비 100%로 조정 */
}

button:hover {
	background-color: #0056b3; /* 버튼 호버 시 색상 변경 */
}

.close_button {
	background: rgb(247, 135, 135);
}
.close_button:hover {
	background: #b31500;
}
</style>