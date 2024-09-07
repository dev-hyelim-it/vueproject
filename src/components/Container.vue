<template>
	<div class="container">
		<div class="left_con contents_box">
			<div class="l_box">
				<h2>고객성명 : </h2>
				<input type="text">
				<button class="btn btn_name">조건검색</button>
				<button class="btn btn_name">전체검색</button>
				<div class="name_box">
					<label 
					:for="`cName-${customer.cus_id}`" 
					v-for="customer in customerData" 
					:key="customer.cus_id">
						<input 
						type="radio" 
						v-model="selectedCustomerId" 
						:value="customer.cus_id" 
						name="customerName" 
						:id="`cName-${customer.cus_id}`" 
						@change="updateManagerInfo" />
						{{ customer.cus_name }} </label>
				</div>
				
			</div>
			<div class="r_box">
				<div class="customer_box">
					<form action="" v-if="selectedCustomer">
						<label for="">**작성일자 : </label>
						<input type="text" :value="selectedCustomer.create_date" ref="dataVal" disabled/>
						<label for="">*고객명 : </label>
						<input type="text" :value="selectedCustomer.cus_name" ref="nameVal" :readonly="isReadOnly" required>
						<label for="">*실명번호 : </label>
						<input type="text" :value="selectedCustomer.jumin_id" ref="juminVal" :readonly="isReadOnly" required>
						<label for="">*E-mail : </label>
						<input type="email" :value="selectedCustomer.email" ref="emailVal" :readonly="isReadOnly" required>
						<label for="">전화번호 : </label>
						<input type="text" :value="selectedCustomer.tel" ref="telVal" :readonly="isReadOnly">
						<label for="">*핸드폰 번호 : </label>
						<input type="text" :value="selectedCustomer.phone" ref="phoneVal" :readonly="isReadOnly" required>
						<label for="">*직 업 : </label>
						<input type="text" :value="selectedCustomer.job" ref="jobVal" :readonly="isReadOnly" required>
						<label for="">주 소 : </label>
						<input type="text" :value="selectedCustomer.addr" ref="addrVal" :readonly="isReadOnly">
					</form>
				</div>
				<div class="manager_box">
					<form action="">
						<label for=""> 관리 담당자 : </label>
						<input type="text" :value="selectedManager ? selectedManager.m_name:''" ref="managerNameVal" :readonly="isReadOnly">
						<label for="">**부서 : </label>
						<input type="text" :value="selectedManager ? selectedManager.dept:''" ref="managerDeptVal" :readonly="isReadOnly" disabled>
						<label for="">**직위 : </label>
						<input type="text" :value="selectedManager ? selectedManager.position:''" ref="managerPositionVal" :readonly="isReadOnly" disabled>
						<label for="">**연락처 : </label>
						<input type="text" :value="selectedManager ? selectedManager.phone:''" ref="managerPhoneVal" :readonly="isReadOnly" disabled>
					</form>
					<button class="search_btn" @click="handleSearchBtnClick">★</button>
				</div>
			</div>
			<div class="notice_box">
				<p class="notice">고객관리 담당자를 지정하세요!</p>
			</div>
		</div>
		<div class="right_con contents_box">
			<h2>상담내역 : </h2>
			<div class="consultation_box">
				<p v-for="(entry, index) in filteredCounsel" :key="index">{{ entry.content }}</p>
			</div>
			<div class="btn_box">
				<button class="btn" @click="registration">등록</button>
				<button class="btn" @click="modify">변경</button>
				<button class="btn btn_delete" @click="showDeleteConfirmationModal">삭제</button>
				<button class="btn" @click="showNewCustomerModal">신규</button>
				<button class="btn btn_inquiry">고객조회</button>
				<button class="btn" @click="logout">종료</button>
			</div>
		</div>

	</div>
	<NewCustomer :isVisible="isNewCustomerModalVisible" @add-customer="handleAddCustomer" @close="closeNewCustomerModal"/>
	<DeleteConfirmationModal
	:isVisible="isDeleteConfirmationModalVisible" @confirm="confirmDelete" @cancel="closeDeleteConfirmationModal"/>
	<ManagerModal :isVisible="isManagerModalVisible" :managers="managerData" @select-manager="selectManager" @add-manager="addManager" @close="closeManagerModal" />
</template>

<script>
import customerData from '../assets/customer.js';
import counselData from '../assets/counsel.js';
import managerData from '../assets/manager.js';
import NewCustomer from './NewCustomer.vue';
import DeleteConfirmationModal from './DeleteConfirmationModal.vue';
import ManagerModal from './ManagerModal.vue';

export default {
	name : 'ContainerVue',
	emits: ['logout'],
	data() {
		return {
			customerData : customerData,
			managerData : managerData,
			counselData : counselData,
			selectedCustomerId: customerData.length > 0 ? customerData[0].cus_id : null, //고객 데이터
			selectedManager: null, //관리자 데이터
			isNewCustomerModalVisible: false,
			isDeleteConfirmationModalVisible: false,
			isManagerModalVisible: false,
			newcustomer: null,
			isReadOnly: true,
			flagValue: true,
			// 새로 추가된 데이터 속성
			errors: {
			name: false,
			jumin: false,
			email: false,
			tel: false,
			phone: false,
			job: false
		},
			originalCustomer: null // 원래 고객 정보를 저장할 변수
		}
	},
	mounted() {
		// 컴포넌트가 마운트된 후 초기화 작업 수행
		this.updateManagerInfo();
	},
	methods :{
		logout() {
			this.$emit('logout');
		},
		generateUniqueCustomerId() {
		//현재 고객 데이터에서 가장 큰 고객 ID를 찾아서 다음 ID 생성
		const maxId = this.customerData.reduce((max, customer) => Math.max(max, customer.cus_id), 0);
		return maxId + 1;
		},
		updateManagerInfo() {
			if(this.selectedCustomer) {
				this.selectedManager = this.managerData.find(
					manager => manager.m_id === this.selectedCustomer.m_id
				);
			}
		},
		showNewCustomerModal() { //신규 고객 모달을 열고 새로운 고객 데이터 초기화
			this.newCustomer = {
				cus_name: '',
				jumin_id: '',
				email: '',
				tel: '',
				phone: '',
				job: '',
				addr: '',
				m_id: null
			};
			this.isNewCustomerModalVisible= true; //신규 고객 모달 표시
		},
		closeNewCustomerModal() { //신규 고객 모달 닫음
			this.isNewCustomerModalVisible = false;
		},
		handleAddCustomer(newCustomer) {
			const customerId = this.generateUniqueCustomerId(); //고유한 고객 id 생성
			const customerToAdd = {...newCustomer, cus_id: customerId};
			this.customerData.push(customerToAdd); //고객 데이터에 추가
			this.selectedCustomerId = customerId; //새로 추가한 고객 선택
			this.isNewCustomerModalVisible = false; //모달 닫기
			this.newCustomer = null;
			this.showManagerModal(); //신규 고객 등록 후 관리자 선택 모달 열기
		},
		showManagerModal() { //관리자 모달 열기
			this.isManagerModalVisible = true;
		},
		closeManagerModal() { //관리자 모달 닫기
			this.isManagerModalVisible = false;
		},
		handleSearchBtnClick() { //관리자 선택되지 않고, 고객 선택되었을 때 관리자 모달 열기
			if(!this.selectedManager && this.selectedCustomer) {
				this.showManagerModal();
			}
		},
		selectManager(manager) { //관리자 선택할 때
			if(this.newCustomer) { //신규 고객인 경우
				this.newCustomer.m_id = manager.m_id; 
				this.isManagerModalVisible = false;
			} else if (this.selectedCustomer) { //기존 고객인 경우
				this.selectedCustomer.m_id = manager.m_id;
				this.updateManagerInfo(); //관리자 정보 업데이트
				this.isManagerModalVisible = false; //관리자 모달 닫기
			}
		},
		addManager(newManager) { //새로운 관리자 추가
			this.managerData.push(newManager);
			this.showManagerModal();
		},
		showDeleteConfirmationModal() { //삭제 확인 모달 열기
			this.isDeleteConfirmationModalVisible = true;
		},
		closeDeleteConfirmationModal() {
			this.isDeleteConfirmationModalVisible = false;
		},
		confirmDelete() {
			if(this.selectedCustomerId) {
				//고객 삭제 처리
				this.customerData = this.customerData.filter(
					customer => customer.cus_id !== this.selectedCustomerId
				);
				this.counselData = this.counselData.filter(
					entry => entry.cus_id !== this.selectedCustomerId
				);
				this.selectedCustomerId = null; //선택된 고객 id 초기화
				this.selectedManager = null; //선택된 관리자 초기화
				this.closeDeleteConfirmationModal(); //삭제 확인 모달 닫기
			} else {
				alert('삭제할 고객을 선택해주세요.');
			}
		},
		formatdate(value) {
			if(!value) return '';
			var js_date = new Date(value);
			var year = js_date.getFullYear();
			var month = js_date.getMonth() + 1;
			var day = js_date.getDate();
			if(month < 10) month = '0' + month;
			if(day < 10) day = '0' + day;
			return year + '-' + month + '-' + day;
		},
		registration(event) {
			event.preventDefault();
			// 유효성 검사 로직을 여기에 추가합니다.
			// 예: 이메일 형식 검사, 필수 입력 확인 등
			this.errors = {name: false, jumin: false, email: false, tel: false, phone: false, job: false}; //오류 초기화
			const { nameVal, juminVal, emailVal, telVal, phoneVal, jobVal, addrVal } = this.$refs;
			this.modifyName = this.$refs.nameVal.value;
			this.modifyJumin = this.$refs.juminVal.value;
			this.modifyemail = this.$refs.emailVal.value;
			this.modifytel = this.$refs.telVal.value;
			this.modifyphone = this.$refs.phoneVal.value;
			this.modifyjob = this.$refs.jobVal.value;
			this.modifyaddr = this.$refs.addrVal.value;
			this.modifydate = this.$refs.dataVal.value;
			if (this.isReadOnly) {
				return; // 이미 리드온리 상태이면 아무 것도 하지 않음
			}

			//한글이 포함되어 있는지 검사하는 정규 표현식
			const emailPattern = /^[a-zA-Z0-9+-_.]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/;
			//숫자가 포함되어 있는지 검사하는 정규 표현식
			const numberPattern = /\d/;
			const phonenumberPattern = /\d{3}-\d{3,4}-\d{4}/;
			const telPattern = /\d{2,3}-\d{3,4}-\d{4}/;
			const juminPattern = /\d{6}-\d{7}/;
			let hasError = false;

			//유효성 검사
			if(nameVal.value.length <= 1 || numberPattern.test(nameVal.value)) {
				this.errors.name = true;
				hasError = true;
				alert("고객명을 확인해주세요.");
			}
			if(!juminPattern.test(juminVal.value)) {
				this.errors.jumin = true;
				hasError = true;
				alert("주민번호를 확인해주세요.");
			}
			if(!emailPattern.test(emailVal.value)) {
				this.errors.email = true;
				hasError = true;
				alert("이메일 주소를 확인해주세요.");
			}
			if(!phonenumberPattern.test(phoneVal.value)) {
				this.errors.phone = true;
				hasError = true;
				alert("핸드폰 번호를 확인해주세요.");
			}
			if(telVal.value && !telPattern.test(telVal.value)) {
				this.errors.tel = true;
				hasError = true;
				alert("전화번호를 확인해주세요.");
			}
			if(jobVal.value.length == 0) {
				this.errors.job = true;
				hasError = true;
				alert("직업란은 필수 입력 항목 입니다.");
			}
			if (hasError) {
				// 오류가 있는 첫 번째 입력란으로 포커스 이동
				const firstErrorField = Object.keys(this.errors).find(key => this.errors[key]);
				if (firstErrorField) {
					this.$refs[`${firstErrorField}Val`].focus();
				}
				return false;
			}

			const isContentChanged =
				nameVal.value !== this.originalCustomer.cus_name ||
				juminVal.value !== this.originalCustomer.jumin_id ||
				emailVal.value !== this.originalCustomer.email ||
				telVal.value !== this.originalCustomer.tel ||
				phoneVal.value !== this.originalCustomer.phone ||
				jobVal.value !== this.originalCustomer.job ||
				addrVal.value !== this.originalCustomer.addr;

			if (!isContentChanged) {
				alert("변경된 내용이 없습니다.");
				this.isReadOnly = true;
				return false;
			}
			if(this.flagValue) {
				this.selectedCustomer.cus_name = nameVal.value;
				this.selectedCustomer.jumin_id = juminVal.value;
				this.selectedCustomer.email = emailVal.value;
				this.selectedCustomer.tel = telVal.value;
				this.selectedCustomer.phone = phoneVal.value;
				this.selectedCustomer.job = jobVal.value;
				this.selectedCustomer.addr = addrVal.value;

                // const index = this.customerData.findIndex(customer => customer.cus_id === this.selectedCustomerId);
                // if (index !== -1) {
                //     this.customerData[index] = { ...this.selectedCustomer };
                // }
				this.isReadOnly = true;
				alert('변경 사항이 저장되었습니다.');
			}
			// 오늘 날짜를 'YYYY-MM-DD' 형식으로 설정
			this.selectedCustomer.create_date = this.formatdate(new Date());
            // 고객 데이터 업데이트
            this.updateCustomer();
			return true; // 예시로 항상 true를 반환합니다.

			// 변경 사항 저장
			// const index = this.customerData.findIndex(customer => customer.cus_id === this.selectedCustomerId);
			// if (index !== -1) {
			// 	this.customerData[index] = { ...this.selectedCustomer };
			// }

			// this.isReadOnly = true; // 다시 리드온리 상태로 변경
			// alert('변경 사항이 저장되었습니다.');
		},
		modify() {
			// this.originalCustomer = { ...this.selectedCustomer };
			this.$nextTick(() => {
			// 포커스를 '고객명' 필드로 이동
			this.$refs.nameVal.focus();
		});
			this.isReadOnly = false; // 입력 필드를 수정 가능하도록 변경
		},
		updateCustomer() { //고객 업데이트 처리
			if(this.selectedCustomerId) {
				//고객 수정 처리
				const index = this.customerData.findIndex(customer => customer.cus_id === this.selectedCustomerId);
				if(index !== -1) {
					this.customerData[index] = {...this.selectedCustomer};
				}
			} else {
				alert('수정할 고객을 선택해주세요');
			}
		},
		saveCustomer() { //고객 저장 처리
			if(this.selectedCustomerId) { //기존 고객 수정 처리
				this.updateCustomer();
			} else {
				//신규 고객 등록
				this.handleAddCustomer(this.newCustomer);
			}
		},
	},
	computed: {
		selectedCustomer() {
			return this.customerData.find(customer => customer.cus_id === this.selectedCustomerId) || {};
		},
		filteredCounsel() {
			return this.counselData.filter(entry => entry.cus_id === (this.selectedCustomer ? this.selectedCustomer.cus_id : null));
		}
  },
	components : {
		NewCustomer,
		DeleteConfirmationModal,
		ManagerModal,
	}

}
</script>

<style>
@import '../assets/css/style.css';
</style>