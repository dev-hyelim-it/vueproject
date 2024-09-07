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
					<form action="">
						<label for="">**작성일자 : </label>
						<input type="text" :value="selectedCustomer.create_date" disabled/>
						<label for="">*고객명 : </label>
						<input type="text" :value="selectedCustomer.cus_name">
						<label for="">*실명번호 : </label>
						<input type="text" :value="selectedCustomer.jumin_id">
						<label for="">*E-mail : </label>
						<input type="email" :value="selectedCustomer.email">
						<label for="">전화번호 : </label>
						<input type="text" :value="selectedCustomer.tel">
						<label for="">*핸드폰 번호 : </label>
						<input type="text" :value="selectedCustomer.phone">
						<label for="">*직 업 : </label>
						<input type="text" :value="selectedCustomer.job">
						<label for="">주 소 : </label>
						<input type="text" :value="selectedCustomer.addr">
					</form>
				</div>
				<div class="manager_box">
					<form action="">
						<label for=""> 관리 담당자 : </label>
						<input type="text" :value="selectedManager ? selectedManager.m_name:''">
						<label for="">**부서 : </label>
						<input type="text" :value="selectedManager ? selectedManager.dept:''" disabled>
						<label for="">**직위 : </label>
						<input type="text" :value="selectedManager ? selectedManager.position:''" disabled>
						<label for="">**연락처 : </label>
						<input type="text" :value="selectedManager ? selectedManager.phone:''" disabled>
					</form>
					<button class="search_btn">★</button>
				</div>
			</div>
			<div class="notice_box">
				<p class="notice">고객관리 담당자를 지정하세요!</p>
			</div>
		</div>
		<div class="right_con contents_box">
			<h2>상담내역 : </h2>
			<div class="consultation_box">
				<p>2023-09-12 오후 2시 통화 요청함</p>
				<p>2023-09-12 오후 2시 미팅 약속을 오전 10시로 변경</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅</p>
				<p>2023-09-12 오후 2시 통화 요청함</p>
				<p>2023-09-12 오후 2시 미팅 약속을 오전 10시로 변경</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅</p>
				<p>2023-09-12 오후 2시 통화 요청함</p>
				<p>2023-09-12 오후 2시 미팅 약속을 오전 10시로 변경</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅</p>
				<p>2023-09-12 오후 2시 통화 요청함</p>
				<p>2023-09-12 오후 2시 미팅 약속을 오전 10시로 변경</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅</p>
				<p>2023-09-12 오후 2시 청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅청약저축 가입 안내 미팅</p>
			</div>
			<div class="btn_box">
				<button class="btn">등록</button>
				<button class="btn">변경</button>
				<button class="btn">삭제</button>
				<button class="btn">신규</button>
				<button class="btn btn_inquiry">고객조회</button>
				<button class="btn">종료</button>
			</div>
		</div>
	</div>
    <NewCustomer />
	
</template>

<script>
import customerData from '../assets/customer.js';
import cusCounsel from '../assets/counsel.js';
import managerData from '../assets/manager.js';
import NewCustomer from './NewCustomer.vue';
// import customer from '../assets/customer.js';

export default {
    name : 'ContainerVue',
	data() {
		return {
			customerData : customerData,
			managerData : managerData,
			cusCounsel : cusCounsel,
			selectedCustomerId: null, //고객 데이터
			selectedManager: null, //관리자 데이터
		}
	},
	methods :{
		updateManagerInfo() {
			if(this.selectedCustomer) {
				this.selectedManager = this.managerData.find(
					manager=>manager.m_id === this.selectedCustomer.m_id
				);
			}
		}
	},
	computed: {
		selectedCustomer() {
			return this.customerData.find(customer => customer.cus_id === this.selectedCustomerId) || {};
		}
	},
    components : {
        NewCustomer,
    }

}
</script>

<style>
@import '../assets/css/style.css';
</style>





<h2>상담내역 : </h2>
<div class="consultation_box">
  <p v-for="(entry, index) in filteredCounsel" :key="index">{{ entry.content }}</p>
</div>
	
//script 코드 수정
data() {
	return {
		customerData : customerData,
		managerData : managerData,
		counselData : counselData,
		selectedCustomerId: null, //고객 데이터
		selectedManager: null, //관리자 데이터
	 } },

computed: {
	selectedCustomer() {
		return this.customerData.find(customer => customer.cus_id === this.selectedCustomerId) || {}; },
	filteredCounsel() {
		return this.counselData.filter(entry => entry.cus_id === (this.selectedCustomer ? this.selectedCustomer.cus_id : null))	} },