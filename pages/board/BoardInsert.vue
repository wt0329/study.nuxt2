<template>
 <v-container class="d-flex align-center justify-center">
  <v-card style="width: 50%" height="500" >

    <v-form @submit.prevent="submitForm">
      <table class="table">
        <tbody>
         <tr>
            <th>구분</th>
            <td>
              <v-select v-model="board.boardCd" label="구분" :items="selectList" item-text="name" item-value="value">
              </v-select>
            </td>
          </tr>
          <tr>
            <th>제목</th>
            <td>
              <v-text-field v-model="board.boardTitle" placeholder="제목"/>
            </td>
          </tr>
          <tr>
            <th>내용</th>
            <td>
              <v-text-field v-model="board.boardCnt" placeholder="내용"/>
            </td>
          </tr>
            <v-file-input v-model="board.multipartFiles" type="file"  multiple/>

        </tbody>
      </table>
      <v-row>
        <v-col cols="12" md="5">
          <v-btn class="mt-2 mr-2" type="button" color="primary" @click="submitForm" >등록</v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-card>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            board: {
                boardCd: "",
                boardTitle: "",
                boardCnt: "",
                multipartFiles:[]
            },
            selectList: [
                {name: '자유', value: '자유'},
                {name: '공지', value: '공지'}
            ]
        };
    },
   methods: {
    async submitForm() {
        window.console.log(this.multipartFiles);
    const formData = new FormData();

    formData.append('boardCd', this.board.boardCd);
    formData.append('boardTitle', this.board.boardTitle);
    formData.append('boardCnt', this.board.boardCnt);


    for (let i = 0; i < this.board.multipartFiles.length; i++) {
    formData.append('files', this.board.multipartFiles[i]);
}
    try {
    const response = await this.$axios.post('/api/v1/board', formData, {
        headers: { 'Content-Type': 'multipart/form-data' }
    });
    window.console.log('Response:', response.data);
    this.$router.push('/board/BoardList');
} catch (error) {
    window.console.error('Error:', error);
}
  }
}
};
</script>
