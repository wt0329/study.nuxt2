<template>
  <div>
    <v-form @submit.prevent="submitForm">
      <v-table>
        <tbody>
         <tr>
            <th>구분</th>
            <td>
               <input v-model="board.boardCd" placeholder="구분"/>
            </td>
          </tr>
          <tr>
            <th>제목</th>
            <td>
              <input v-model="board.boardTitle" placeholder="제목"/>
            </td>
          </tr>
          <tr>
            <th>내용</th>
            <td>
              <input v-model="board.boardCnt" placeholder="내용"/>
            </td>
          </tr>
            <v-file-input v-model="board.multipartFiles" type="file"  multiple/>

        </tbody>
      </v-table>
      <v-row>
        <v-col cols="12" md="5">
          <v-btn class="mt-2 mr-2" type="button" color="primary" @click="submitForm" >등록</v-btn>
        </v-col>
      </v-row>
    </v-form>
  </div>
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
