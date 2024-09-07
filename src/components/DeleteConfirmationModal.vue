<template>
    <div v-if="isVisible" class="modal_overlay" @click.self="close">
      <div class="modal_content">
          <button class="close_button" @click="close">X</button>
          <img src="@/assets/images/error.png" alt="Error Icon" class="error_icon">
          <h2>삭제 확인</h2>
          <p> [삭제] 버튼을 클릭하면 해당 고객 정보가 삭제됩니다. </p>
          <p> 삭제 시 복구가 불가능하므로 주의해야 합니다. </p>
          <div class="button_container">
              <button class="confirm_button" @click="confirm"> 확인 </button>
              <button class="cancel_button" @click="cancel"> 취소 </button>
          </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
      name : 'DeleteConfirmationModal',
      props: {
          isVisible: {
              type: Boolean,
              required: true,
          }
      },
      methods: {
          confirm() {
              this.$emit('confirm');
          },
          cancel() {
              this.$emit('cancel');
          },
          close() {
              this.cancel();
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
    background: rgba(0, 0, 0, 0.5); /* 반투명 배경 */
  }
  .modal_content {
    position: relative; /* 모달 콘텐츠의 상대적 위치 설정 */
    background: white;
    padding: 20px;
    border-radius: 8px;
    width: 500px;
    text-align: center;
    font-size: 17px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  .close_button {
      position: absolute;
      top: 20px;
      right: 20px;
      border: none;
      background: transparent;
      font-size: 24px;
      cursor: pointer;
      color: black;
  }
  .error_icon {
      width: 60px;
      height: auto;
  }
  
  .modal_content h2 {
    margin: 10px;
  }
  .modal_content p {
    margin-bottom: 10px;
  }
  
  .confirm_button {
    background-color: rgb(129, 164, 230); /* 확인 버튼의 배경 색상 */
    color: white; /* 버튼 텍스트 색상 */
    border: none; /* 버튼 테두리 없음 */
    border-radius: 5px; /* 버튼 모서리 둥글게 */
    padding: 10px 20px; /* 버튼 내부 여백 */
    font-size: 16px; /* 버튼 텍스트 크기 */
    cursor: pointer; /* 커서 포인터로 변경 */
    transition: background-color 0.3s; /* 배경 색상 변경 애니메이션 */
    margin-top: 10px
  }
  
  .confirm_button:hover {
    background-color: #0056b3; /* 호버 시 배경 색상 */
  }
  
  .cancel_button {
    background-color: #a8abad; /* 취소 버튼의 배경 색상 */
    color: white; /* 버튼 텍스트 색상 */
    border: none; /* 버튼 테두리 없음 */
    border-radius: 5px; /* 버튼 모서리 둥글게 */
    padding: 10px 20px; /* 버튼 내부 여백 */
    font-size: 16px; /* 버튼 텍스트 크기 */
    cursor: pointer; /* 커서 포인터로 변경 */
    transition: background-color 0.3s; /* 배경 색상 변경 애니메이션 */
    margin-left: 10px;
  }
  
  .cancel_button:hover {
    background-color: #5a6268; /* 호버 시 배경 색상 */
  }
  
  </style>