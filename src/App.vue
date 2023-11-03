<template>
  <div id="app">
    <div class="puzzle-place">
      <div class="nav-bar card">
        <div class="got-point">
          <h4>得点：{{ now_point }}</h4>
        </div>
        <div class="selected-now">
          <div v-if="now_select.length==0">

          </div>
          <div v-else class="selecting-icon">
            <label for="">選択中：</label>
            <img :src="require('./assets/'+now_select[0].select+'.png')" class="pazzle-place-icon">
          </div>
        </div>
      </div>
      <div class="puzzle-table-place">
        <table border="1" class="puzzle-table">
          <tr v-for="r in [0,1,2,3,4,5,6,7]" :key="r.id">
            <td v-for="i in [0,1,2,3,4,5,6,7]" :key="i.id" @click="selectPuzzle(r, i, puzzle_set[r][i])"><div :class="appear_status[r][i]"><img :class="'pazzle-place-icon '+ puzzle_status[r][i]" :src="require('./assets/'+puzzle_set[r][i]+'.png')"></div></td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import {ref, onMounted} from 'vue'
export default {
  name: 'App',
  setup(){
    let puzzle_set = ref([
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
      [(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1,(Math.floor( Math.random() * 5 ))+1],
    ])

    let puzzle_status = ref([
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','','']
    ])

    let appear_status=ref([
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','',''],
      ['','','','','','','','']
    ])

    let now_select=ref([])

    let delete_list=ref([])

    let now_point = ref(0)

    const selectPuzzle=(vertical, horizontal, selecting)=>{
      if(now_select.value.length==0){
        now_select.value.push({select:selecting, vertical: vertical, horizontal: horizontal})
        puzzle_status.value[vertical][horizontal]='selecting'
        console.log("selecting:", now_select.value)
      }else if(now_select.value.length==1){
        if(now_select.value[0].vertical!=vertical && now_select.value[0].horizontal!=horizontal){
          now_select.value=[]
          puzzle_status.value = [
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','','']
          ]
        }else{
          if(now_select.value[0].vertical==vertical||now_select.value[0].vertical==vertical+1||now_select.value[0].vertical==vertical-1){
            if(now_select.value[0].horizontal==horizontal||now_select.value[0].horizontal==horizontal+1||now_select.value[0].horizontal==horizontal-1){

              now_select.value.push({select:selecting, vertical: vertical, horizontal: horizontal})
              // CSSアニメーションを半回転するよう変更
              puzzle_status.value[now_select.value[0].vertical][now_select.value[0].horizontal]='changing'
              puzzle_status.value[now_select.value[1].vertical][now_select.value[1].horizontal]='changing'
              setTimeout(() => {

                puzzle_set.value[now_select.value[1].vertical].splice(now_select.value[1].horizontal, 1, now_select.value[0].select)
                puzzle_set.value[now_select.value[0].vertical].splice(now_select.value[0].horizontal, 1, now_select.value[1].select)
                matchSearch()
                console.log("交換処理完了：", puzzle_set.value)
                now_select.value=[]
                puzzle_status.value = [
                  ['','','','','','','',''],
                  ['','','','','','','',''],
                  ['','','','','','','',''],
                  ['','','','','','','',''],
                  ['','','','','','','',''],
                  ['','','','','','','',''],
                  ['','','','','','','',''],
                  ['','','','','','','','']
                ]
              }, 300);
            }else{
              now_select.value=[]
              puzzle_status.value = [
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','',''],
                ['','','','','','','','']
              ]
            }
          }else{
            now_select.value=[]
            puzzle_status.value = [
              ['','','','','','','',''],
              ['','','','','','','',''],
              ['','','','','','','',''],
              ['','','','','','','',''],
              ['','','','','','','',''],
              ['','','','','','','',''],
              ['','','','','','','',''],
              ['','','','','','','','']
            ]
          }
        }
      }
    }

    const matchSearch=()=>{
      delete_list.value = JSON.parse(JSON.stringify(puzzle_set.value))
      // 各アイコンが１列内に何個存在するかをリスト化
      // 横マッチ
      let horizontal_match_search = []
      for(let i=0; i<puzzle_set.value.length; i++){
        let result=[0,0,0,0,0,0,0,0]
        for(let p=0; p<8; p++){
          result[puzzle_set.value[i][p]]+=1
        }
        horizontal_match_search.push(result)
        console.log("重複結果(横列):",result)
      }

      // 縦マッチ
      let vertical_match_search = []
      for(let i=0; i<puzzle_set.value.length; i++){
        let result=[0,0,0,0,0,0,0,0]
        for(let p=0; p<8; p++){
          result[puzzle_set.value[p][i]]+=1
        }
        vertical_match_search.push(result)
        console.log("重複結果(縦列):",result)
      }

      // 横マッチ確認処理
      for(let l=0; l<horizontal_match_search.length;l++){
        for(let m=0; m<horizontal_match_search[l].length;m++){
          if(horizontal_match_search[l][m]>=4){
            detailMatchSearch("horizontal",horizontal_match_search[l][m],m,l)
          }else{
            continue;
          }
        }
      }

      // 縦マッチ確認処理
      for(let l=0; l<vertical_match_search.length;l++){
        for(let m=0; m<vertical_match_search[l].length;m++){
          if(vertical_match_search[l][m]>=4){
            console.log("4つ以上揃っているアイコンあり")
            detailMatchSearch("vertical",vertical_match_search[l][m],m,l)
          }else{
            continue;
          }
        }
      }
      puzzle_set.value = JSON.parse(JSON.stringify(delete_list.value))
    }

    const detailMatchSearch=(direction,amount,type,where)=>{
      console.log("方向:",direction,"/","列:",where,"/","数量:",amount,"/","タイプ:",type)

      // 水平方向のジャッジ
      if(direction=="horizontal"){
        if(puzzle_set.value[where][puzzle_set.value[where].indexOf(type)]==puzzle_set.value[where][puzzle_set.value[where].indexOf(type)+1]){
          let first = puzzle_set.value[where].indexOf(type)
            if(first+1<8 &&puzzle_set.value[where][first+1]==type){
              if(first+2<8 &&puzzle_set.value[where][first+2]==type){
                if(first+3<8 &&puzzle_set.value[where][first+3]==type){
                  if(first+4<8 &&puzzle_set.value[where][first+4]==type){
                    if(first+5<8 &&puzzle_set.value[where][first+5]==type){
                      if(first+6<8 &&puzzle_set.value[where][first+6]==type){
                        if(first+7<8 &&puzzle_set.value[where][first+7]==type){
                          console.log("横列全揃い")
                          delete_list.value[where]=[0,0,0,0,0,0,0,0]
                        }else{
                          console.log("横列7揃い")
                          delete_list.value[where].splice(first,7,0,0,0,0,0,0,0)
                        }
                      }else{
                        console.log("横列6揃い")
                        delete_list.value[where].splice(first,6,0,0,0,0,0,0)
                      }
                      }else{
                      console.log("横列5揃い")
                      delete_list.value[where].splice(first,5,0,0,0,0,0)
                    }
                  }else{
                    console.log("横列4揃い:", first)
                    delete_list.value[where].splice(first,4,0,0,0,0)
                  }
                }else{
                  console.log("横列3揃い(消せない)")
                }
              }else{
                console.log("横列2揃い(消せない)")
              }
            }else{
              ("横列1のみで消せない")
            }
        }else{
          let first = puzzle_set.value[where].indexOf(type,puzzle_set.value[where].indexOf(type)+1)
            if(first+1<8 && puzzle_set.value[where][first+1]==type){
              if(first+2<8 && puzzle_set.value[where][first+2]==type){
                if(first+3<8 &&puzzle_set.value[where][first+3]==type){
                  if(first+4<8 &&puzzle_set.value[where][first+4]==type){
                    if(first+5<8 && puzzle_set.value[where][first+5]==type){
                      if(first+6<8 && puzzle_set.value[where][first+6]==type){
                        if(first+7<8 && puzzle_set.value[where][first+7]==type){
                          console.log("横列全揃い")
                          delete_list.value[where]=[0,0,0,0,0,0]
                        }else{
                          console.log("横列7揃い")
                          delete_list.value[where].splice(first,7,0,0,0,0,0,0,0)
                        }
                      }else{
                        console.log("横列6揃い")
                        delete_list.value[where].splice(first,6,0,0,0,0,0,0)
                      }
                    }else{
                      console.log("横列5揃い")
                      delete_list.value[where].splice(first,5,0,0,0,0,0)
                    }
                  }else{
                    console.log("横列4揃い:", first)
                    delete_list.value[where].splice(first,4,0,0,0,0)
                  }
                }else{
                  console.log("横列3揃い(消せない)")
                }
              }else{
                console.log("横列2揃い(消せない)")
              }
            }else{
              ("横列1のみで消せない")
            }
        }
      }else{

        let first = 0
        console.log("where:",where)
        console.log("type:", type)
        for(let g=0; g<7;g++){
          if(puzzle_set.value[g][where]==type && puzzle_set.value[g][where]==puzzle_set.value[g+1][where]){
              first = Number(g)
              console.log("初期値：",g)
              break;
          }else{
            continue;
          }
        }

          if(first+1<8 && puzzle_set.value[first+1][where]==type){
            if(first+2<8 && puzzle_set.value[first+2][where]==type){
              if(first+3<8 && puzzle_set.value[first+3][where]==type){
                if(first+4<8 && puzzle_set.value[first+4][where]==type){
                  if(first+5<8 && puzzle_set.value[first+5][where]==type){
                    if(first+6<8 && puzzle_set.value[first+6][where]==type){
                      if(first+7<8 && puzzle_set.value[first+7][where]==type){
                        console.log("縦列全揃い")
                        delete_list.value[first][where]=0
                        delete_list.value[first+1][where]=0
                        delete_list.value[first+2][where]=0
                        delete_list.value[first+3][where]=0
                        delete_list.value[first+4][where]=0
                        delete_list.value[first+5][where]=0
                        delete_list.value[first+6][where]=0
                        delete_list.value[first+7][where]=0
                      }else{
                        console.log("縦列7揃い")
                        delete_list.value[first][where]=0
                        delete_list.value[first+1][where]=0
                        delete_list.value[first+2][where]=0
                        delete_list.value[first+3][where]=0
                        delete_list.value[first+4][where]=0
                        delete_list.value[first+5][where]=0
                        delete_list.value[first+6][where]=0
                      }
                    }else{
                      console.log("縦列6揃い")
                      delete_list.value[first][where]=0
                      delete_list.value[first+1][where]=0
                      delete_list.value[first+2][where]=0
                      delete_list.value[first+3][where]=0
                      delete_list.value[first+4][where]=0
                      delete_list.value[first+5][where]=0
                    }
                  }else{
                    console.log("縦列5揃い")
                    delete_list.value[first][where]=0
                    delete_list.value[first+1][where]=0
                    delete_list.value[first+2][where]=0
                    delete_list.value[first+3][where]=0
                    delete_list.value[first+4][where]=0
                  }
                }else{
                  console.log("縦列4揃い")
                  delete_list.value[first][where]=0
                  delete_list.value[first+1][where]=0
                  delete_list.value[first+2][where]=0
                  delete_list.value[first+3][where]=0
                }
              }else{
                console.log("縦列3揃い(消せない)")
              }
            }else{
              console.log("縦列2揃い(消せない)")
            }
          }else{
            ("縦列1のみで消せない")
          }
      }

      // 消去される数を数える
      let delete_amount = 0
      for(let f=0; f<delete_list.value.length;f++){
        for(let v=0; v<8;v++){
          if(delete_list.value[f][v]==0){
            delete_amount +=1
          }
        }
      }

      console.log("消去数：", delete_amount)
      // ポイント加算処理（消した数×10ポイント）
      now_point.value = now_point.value + (delete_amount*10)

      // 欠けた箇所の補充処理
      if(delete_amount != 0){
        addNewPuzzle()
      }
    }

    const addNewPuzzle=async()=>{
      puzzle_status.value = [
        ['','','','','','','',''],
        ['','','','','','','',''],
        ['','','','','','','',''],
        ['','','','','','','',''],
        ['','','','','','','',''],
        ['','','','','','','',''],
        ['','','','','','','',''],
        ['','','','','','','','']
      ]
      for(let f=0; f<delete_list.value.length;f++){
        for(let v=0; v<8;v++){
          if(delete_list.value[f][v]==0){
            delete_list.value[f][v] = (Math.floor( Math.random() * 5 ))+1;
            console.log('生成乱数：', delete_list.value[f][v])
            appear_status.value[f][v]='appear'
          }
        }
      }
      console.log("PS",puzzle_status.value)
      puzzle_set.value = JSON.parse(JSON.stringify(delete_list.value))
      setTimeout(() => {
        appear_status.value = [
          ['','','','','','','',''],
          ['','','','','','','',''],
          ['','','','','','','',''],
          ['','','','','','','',''],
          ['','','','','','','',''],
          ['','','','','','','',''],
          ['','','','','','','',''],
          ['','','','','','','','']
        ]
      }, 1000);
      delete_list.value=[]
      reMatchSearch()
    }

    const reMatchSearch=()=>{
      console.log("マッチ再確認")
      delete_list.value = JSON.parse(JSON.stringify(puzzle_set.value))
      // 各アイコンが１列内に何個存在するかをリスト化
      // 横マッチ
      let horizontal_match_search = []
      for(let i=0; i<puzzle_set.value.length; i++){
        let result=[0,0,0,0,0,0,0,0]
        for(let p=0; p<8; p++){
          result[puzzle_set.value[i][p]]+=1
        }
        horizontal_match_search.push(result)
        console.log("重複結果(横列):",result)
      }

      // 縦マッチ
      let vertical_match_search = []
      for(let i=0; i<puzzle_set.value.length; i++){
        let result=[0,0,0,0,0,0,0,0]
        for(let p=0; p<8; p++){
          result[puzzle_set.value[p][i]]+=1
        }
        vertical_match_search.push(result)
        console.log("重複結果(縦列):",result)
      }

      // 縦マッチ確認処理
      for(let l=0; l<vertical_match_search.length;l++){
        for(let m=0; m<vertical_match_search[l].length;m++){
          if(vertical_match_search[l][m]>=4){
            detailMatchSearch("vertical",vertical_match_search[l][m],m,l)
          }else{
            continue;
          }
        }
      }

      // 横マッチ確認処理
      for(let l=0; l<horizontal_match_search.length;l++){
        for(let m=0; m<horizontal_match_search[l].length;m++){
          if(horizontal_match_search[l][m]>=4){
            detailMatchSearch("horizontal",horizontal_match_search[l][m],m,l)
          }else{
            continue;
          }
        }
      }
    }

    const startGame=()=>{
      reMatchSearch()
      now_point.value = 0;
    }

    onMounted(()=>{
      startGame()
    })

    return{
      puzzle_set,
      puzzle_status,
      appear_status,
      now_select,
      delete_list,
      now_point,
      selectPuzzle,
      matchSearch,
      reMatchSearch,
      detailMatchSearch,
      addNewPuzzle,
      startGame
    }
  }
}
</script>

<style lang="scss">
#app {
  background-image: url("./assets/background.png");
  background-size: cover;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
}

.nav-bar{
  .got-point{
    display: inline-block;
    text-align: left;
    padding: 10px;
  }
  .selected-now{
    display: inline-block;
    height: 70px;;
    text-align: right;
  }
}

.puzzle-table{
  margin: 0 auto;
  background-color: rgba(0, 0, 0, 0.5);
}

.puzzle-table td{
  width: 10px;
  height: 10px;
}

.pazzle-place-icon{
  width: 70px;
}

.appear{
  animation-name: AppearNew;
  animation-duration:1s;
  animation-iteration-count:1;
}

.selecting{
  animation-name: Selecting;
  animation-duration:1s;
  animation-iteration-count:infinite;
}

.changing{
  animation-name: ChangePazzle;
  animation-duration:0.3s;
  animation-iteration-count:1;
}

/* アニメーション */

@keyframes AppearNew {
  0%{
    transform: scale(0);
  }
  20%{
    transform: scale(1);
  }
  30%{
    transform:rotate(-30deg); 
  }
  40%{
    transform:rotate(30deg); 
  }
  50%{
    transform:rotate(-30deg); 
  }
  60%{
    transform:rotate(30deg); 
  }
  100%{
    transform: scale(1);
    transform:rotate(0); 
  }
}

@keyframes Selecting {
  0%{
    transform: scale(1);
  }
  50%{
    transform: scale(1.3);
  }
  100%{
    transform: scale(1);
  }
}

@keyframes ChangePazzle {
  0%{
    transform:rotateY(0);
  }
  100%{
    transform:rotateY(90deg);
  }
}
</style>
