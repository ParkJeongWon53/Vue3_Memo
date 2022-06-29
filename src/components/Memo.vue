<template>
    <div class="memo">
        <div class="act">
            <button class="btn btn-primary" @click="add()">+ 추가</button>
        </div>
        
        <ul>
            <li v-for="d in state.data" :key="d.id" @click="edit(d.id)">{{ d.content }}</li>
        </ul>
    </div>
</template>


<script>
import {reactive} from "vue"
import axios from "axios"

    export default {
      
        setup() {
          const state = reactive({
            data : []
          })
          const add = ()=>{
            // state.data.push("추가된 내용")
            const content = prompt("내용을 입력해주세요.")

            if(!content){ // 내용없이 저장할때 내용입력 알림창
              alert('내용을 입력해주세요.')
              add()
            }

            axios.post("api/memos",{ content }).then((res)=> {
              state.data = res.data
            })
          }

          const edit = (id) => {
            const content = prompt("내용을 입력해주세요.", state.data[id])
            console.log(content)
            axios.put("/api/memos/" + id, { content }).then((res) => {
              state.data = res.data
            }) //put 일부 데이터 수정용 메서드
          }

          axios.get("/api/memos").then((res) => {
            state.data = res.data
            
          })

          return {
              state,
              add,
              edit
          }
        }
    }
</script>



<style lang="scss" scoped>
.memo { 
  .act {
    text-align: right;
    padding: 10px 10px 5px 5px;

  }
  ul {
    list-style: none;
    padding: 15px;
    margin: 0;
    li {
      padding: 15px;
      margin: 10px 0;
      border: 1px solid #eee;
    }
  }  
}

</style>