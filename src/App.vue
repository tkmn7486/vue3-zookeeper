<template>
  <div id="app">
    <!-- タイトル画面 -->
    <div id="title-view" v-if="now_view == 'title'">
      <div class="title-bg">
        <h3>🐼どうぶつケシケシ🦁</h3>
        <button class="btn btn-lg btn-outline-success" @click="startGame()">すた〜と</button>
      </div>
    </div>

    <!-- ゲーム画面 -->
    <div id="pazzle-view" v-else-if="now_view == 'game'">
      <!-- ゲームオーバー時の画面 -->
      <div class="game-over-view" v-if="game_over == true">
        <div class="card">
          <div class="game-over-active">
            <div class="gameover-text-box">
              <h3>獲得ポイント：{{ game_result[0].all_points }}</h3>
              <h2>ランク：{{ game_result[0].your_rank }}</h2>
              <br>
              <div class="title-back-btn">
                <button class="btn btn-outline-success" @click="game_over=false; now_view = 'title';">タイトルへ戻る</button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- パズル表示場所 -->
      <div class="puzzle-place">

        <!-- 上部ナビバー -->
        <div class="nav-bar">
          <div class="row">
            <div class="got-point col">
              <div class="card">
                <h4>POINT：{{ now_point }}</h4>
              </div>
            </div>
            <div class="col">
              <div class="selected-now">
                <div v-if="now_select.length==0">

                </div>
                <div v-else class="selecting-icon">
                  <label for="">選択中：</label>
                  <img :src="require('./assets/'+now_select[0].select+'.svg')" class="pazzle-place-icon">
                </div>
              </div>
            </div>
            <div class="able-move-count col">
              <div class="card count-card">
                <div class="count-img">
                  <img :src="require('./assets/countNumber/'+String(able_move)+'.png')">
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="puzzle-table-place">
          <table border="1" class="puzzle-table">
            <tr v-for="r in [0,1,2,3,4,5,6,7]" :key="r.id">
              <td v-for="i in [0,1,2,3,4,5,6,7]" :key="i.id" @click="selectPuzzle(r, i, puzzle_set[r][i])"><div :class="appear_status[r][i]"><img :class="'pazzle-place-icon '+ puzzle_status[r][i]" :src="require('./assets/'+puzzle_set[r][i]+'.svg')"></div></td>
            </tr>
          </table>
        </div>

        <!-- キャラの立ち絵とスキルゲージ -->
        <div class="chara-place">
          <div class="ready" v-if="skill_point>100">
            <div v-if="chara_animation == 'panda-skill-on'">
              <img :class="chara_animation" src="./assets/panda/skillnow.png">
            </div>
            <div v-else>
              <img src="./assets/panda/ready.png" @click="pandaSkill()">
            </div>
          </div>
          <div class="not-ready" v-else></div>
        </div>
      </div>
    </div>

    <div id="error-view" v-else>
      エラーが発生しました
      <button class="btn btn-success" @click="now_view='title'">タイトルへ戻る</button>
    </div>
  </div>
</template>

<script>
import {ref, onMounted} from 'vue'
export default {
  name: 'App',
  setup(){
    let now_view = ref('title')

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

    let skill_point = ref(0)

    let able_move = ref(5)

    let game_over = ref(false)

    let game_result = ref()

    let chara_skill_animation=ref(false)
    let chara_animation = ref('')

    const selectPuzzle=(vertical, horizontal, selecting)=>{
      if(now_select.value.length==0){
        now_select.value.push({select:selecting, vertical: vertical, horizontal: horizontal})
        puzzle_status.value[vertical][horizontal]='selecting'
        console.log("selecting:", now_select.value)
      }else if(now_select.value.length==1){
        if(now_select.value[0].vertical!=vertical && now_select.value[0].horizontal!=horizontal){
          cantmoveS.currentTime = 0 
          cantmoveS.play()
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

              able_move.value-=1
              now_select.value.push({select:selecting, vertical: vertical, horizontal: horizontal})
              // CSSアニメーションを半回転するよう変更
              puzzle_status.value[now_select.value[0].vertical][now_select.value[0].horizontal]='changing'
              puzzle_status.value[now_select.value[1].vertical][now_select.value[1].horizontal]='changing'
              setTimeout(() => {
                puzzle_set.value[now_select.value[1].vertical].splice(now_select.value[1].horizontal, 1, now_select.value[0].select)
                puzzle_set.value[now_select.value[0].vertical].splice(now_select.value[0].horizontal, 1, now_select.value[1].select)
                flipS.currentTime = 0 
                flipS.play()
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
              cantmoveS.currentTime = 0 
              cantmoveS.play()
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
            cantmoveS.currentTime = 0 
            cantmoveS.play()
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
      
      if(able_move.value == 0){
        console.log("gameover")
        gameOver()
      }
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
                          if(able_move.value<10){
                            able_move.value+=1
                          }
                          deleteS.currentTime = 0 
                          deleteS.play()
                        }else{
                          console.log("横列7揃い")
                          delete_list.value[where].splice(first,7,0,0,0,0,0,0,0)
                          if(able_move.value<10){
                            able_move.value+=1
                          }
                          deleteS.currentTime = 0 
                          deleteS.play()
                        }
                      }else{
                        console.log("横列6揃い")
                        delete_list.value[where].splice(first,6,0,0,0,0,0,0)
                        if(able_move.value<10){
                          able_move.value+=1
                        }
                        deleteS.currentTime = 0 
                        deleteS.play()
                      }
                      }else{
                      console.log("横列5揃い")
                      delete_list.value[where].splice(first,5,0,0,0,0,0)
                      if(able_move.value<10){
                        able_move.value+=1
                      }
                      deleteS.currentTime = 0 
                      deleteS.play()
                    }
                  }else{
                    console.log("横列4揃い:", first)
                    delete_list.value[where].splice(first,4,0,0,0,0)
                    if(able_move.value<10){
                      able_move.value+=1
                    }
                    deleteS.currentTime = 0 
                    deleteS.play()
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
                          if(able_move.value<10){
                            able_move.value+=1
                          }
                          deleteS.currentTime = 0 
                          deleteS.play()
                        }else{
                          console.log("横列7揃い")
                          delete_list.value[where].splice(first,7,0,0,0,0,0,0,0)
                          if(able_move.value<10){
                            able_move.value+=1
                          }
                          deleteS.currentTime = 0 
                          deleteS.play()
                        }
                      }else{
                        console.log("横列6揃い")
                        delete_list.value[where].splice(first,6,0,0,0,0,0,0)
                        if(able_move.value<10){
                          able_move.value+=1
                        }
                        deleteS.currentTime = 0 
                        deleteS.play()
                      }
                    }else{
                      console.log("横列5揃い")
                      delete_list.value[where].splice(first,5,0,0,0,0,0)
                      if(able_move.value<10){
                        able_move.value+=1
                      }
                      deleteS.currentTime = 0 
                      deleteS.play()
                    }
                  }else{
                    console.log("横列4揃い")
                    delete_list.value[where].splice(first,4,0,0,0,0)
                    if(able_move.value<10){
                      able_move.value+=1
                    }
                    deleteS.currentTime = 0 
                    deleteS.play()
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
                        if(able_move.value<10){
                          able_move.value+=1
                        }
                        deleteS.currentTime = 0 
                        deleteS.play()
                      }else{
                        console.log("縦列7揃い")
                        delete_list.value[first][where]=0
                        delete_list.value[first+1][where]=0
                        delete_list.value[first+2][where]=0
                        delete_list.value[first+3][where]=0
                        delete_list.value[first+4][where]=0
                        delete_list.value[first+5][where]=0
                        delete_list.value[first+6][where]=0
                        if(able_move.value<10){
                          able_move.value+=1
                        }
                        deleteS.currentTime = 0 
                        deleteS.play()
                      }
                    }else{
                      console.log("縦列6揃い")
                      delete_list.value[first][where]=0
                      delete_list.value[first+1][where]=0
                      delete_list.value[first+2][where]=0
                      delete_list.value[first+3][where]=0
                      delete_list.value[first+4][where]=0
                      delete_list.value[first+5][where]=0
                      if(able_move.value<10){
                        able_move.value+=1
                      }
                      deleteS.currentTime = 0 
                      deleteS.play()
                    }
                  }else{
                    console.log("縦列5揃い")
                    delete_list.value[first][where]=0
                    delete_list.value[first+1][where]=0
                    delete_list.value[first+2][where]=0
                    delete_list.value[first+3][where]=0
                    delete_list.value[first+4][where]=0
                    if(able_move.value<10){
                      able_move.value+=1
                    }
                    deleteS.currentTime = 0 
                    deleteS.play()
                  }
                }else{
                  console.log("縦列4揃い")
                  delete_list.value[first][where]=0
                  delete_list.value[first+1][where]=0
                  delete_list.value[first+2][where]=0
                  delete_list.value[first+3][where]=0
                  if(able_move.value<10){
                    able_move.value+=1
                  }
                  deleteS.currentTime = 0 
                  deleteS.play()
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
      skill_point.value = skill_point.value+(delete_amount*10)

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
      await reMatchSearch()
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
      skill_point.value = 0;
      able_move.value = 5;
      now_view.value = 'game';
    }

    const gameOver=()=>{
      game_over.value = true

      let rank = ''
      switch(true){
        case 0<=now_point.value && now_point.value<=200:
          rank = 'E'
          break;

        case 200<now_point.value && now_point.value<=400:
          rank = 'D'
          break;

        case 400<now_point.value && now_point.value<=600:
          rank = 'C'
          break;

        case 600<now_point.value && now_point.value<=800:
          rank = 'B'
          break;

        case 800<now_point.value && now_point.value<=1000:
          rank = 'B+'
          break;

        case 1000<now_point.value && now_point.value<=1200:
          rank = 'A'
          break;

        case 1200<now_point.value && now_point.value<=1500:
          rank = 'A+'
          break;

        case 1500<now_point.value && now_point.value<=2000:
          rank = 'A++'
          break;

        case 2000<now_point.value && now_point.value<=2500:
          rank = 'S'
          break;

        case 2500<now_point.value && now_point.value<=3500:
          rank = 'S+'
          break;
  
        case 3500<now_point.value && now_point.value<=5000:
          rank = 'S++'
          break;

        case 5000<now_point.value:
          rank = 'MASTER'
          break;
      }

      game_result.value = [{
        all_points: now_point.value,
        your_rank: rank
      }]
    }

    const pandaSkill=async()=>{
      chara_skill_animation.value = true
      chara_animation.value = 'panda-skill-on' 
      console.log("アニメ開始")
      skillS.currentTime = 0 
      skillS.play()

      // アニメーション終わるのを2秒待つ
      setTimeout(async() => {
        console.log("アニメ終了")
        skill_point.value = skill_point.value - 100;
        chara_skill_animation.value = false
        chara_animation.value = ''
        let panda_count = 0
        // パンダの位置と個数を把握
        for(let i=0;i<puzzle_set.value.length;i++){
          for(let p=0;p<puzzle_set.value[i].length;p++){
            if(puzzle_set.value[i][p]==5){
              panda_count+=1
              puzzle_set.value[i][p]=0
            }
          }
        }
        console.log('変換結果：',delete_list.value,"| パンダ数：", panda_count)
        skillDeleteS.currentTime = 0 
        skillDeleteS.play()
        await reMatchSearch()
      }, 2000);
    }

    // 音関連
    
    let flip_sound = ref(require('./assets/sound/flip.mp3'))
    let skill_sound = ref(require('./assets/sound/pop.mp3'))
    let delete_sound = ref(require('./assets/sound/delete.mp3'))
    let skill_delete_sound = ref(require('./assets/sound/skilldelete.mp3'))
    let decide_sound = ref(require('./assets/sound/decide.mp3'))
    let cantmove_sound = ref(require('./assets/sound/cantmove.mp3'))

    const flipS = new Audio(flip_sound.value);
    const skillS = new Audio(skill_sound.value);
    const deleteS = new Audio(delete_sound.value);
    const skillDeleteS = new Audio(skill_delete_sound.value);
    const decideS = new Audio(decide_sound.value);
    const cantmoveS = new Audio(cantmove_sound.value);

    onMounted(()=>{
      console.log("読み込み完了")
    })

    return{
      now_view,
      puzzle_set,
      puzzle_status,
      appear_status,
      now_select,
      delete_list,
      now_point,
      skill_point,
      able_move,
      game_over,
      game_result,
      chara_skill_animation,
      chara_animation,
      flip_sound,
      skill_sound,
      delete_sound,
      decide_sound,
      skill_delete_sound,
      cantmove_sound,
      selectPuzzle,
      matchSearch,
      reMatchSearch,
      detailMatchSearch,
      addNewPuzzle,
      startGame,
      gameOver,
      pandaSkill,
      flipS,
      skillS,
      deleteS,
      decideS,
      skillDeleteS,
      cantmoveS

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

.puzzle-place{
  padding: 1rem;
}

.nav-bar{
  width: 95%;
  margin: 0 auto;
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
  
  .able-move-count{
    width: 5rem;
  }
}

#title-view{
  background-color: white;
  z-index: 100;
  position: fixed;
  top:0;
  left:0;
  width: 100vw;
  height: 100vh;
}

.title-bg{
  position: absolute;
	left: 50%;
	top: 50%;
	transform: translateX(-50%) translateY(-50%);
  background-color: rgba(255, 255, 255, 0.892);
  animation-name: AppearTitle;
  animation-duration:1s;
  animation-iteration-count:1;
}

.puzzle-table-place{
  margin-top: 2rem;
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

.chara-place{
  img{
    width: 15%;
    position: fixed;
    bottom:0;
    right:0;
  }
}

.count-card{
  width: 5rem;
  height: 5rem;
  margin: 0 0 0 auto;
  padding: 1rem;
  img{
    width: 3rem;
  }
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

.game-over-view{
  position: fixed;
  top:0;
  left:0;
  width: 100vw;
  height: 100vh;
  .game-over-active{
    position: fixed;
    height: 70vh;
    width: 80vw;
    left: 50%;
    top: 50%;
    transform: translateX(-50%) translateY(-50%);
    background-color: rgba(255, 255, 255, 0.957);
    z-index: 2000;
    .gameover-text-box{
      position: fixed;
      left: 50%;
      top: 50%;
      transform: translateX(-50%) translateY(-50%);
    }
  }
}

.panda-skill-on{
  animation-name: PandaSkill;
  animation-duration:2s;
  animation-iteration-count:1;
}

/* アニメーション */

@keyframes AppearTitle {
  0%{
    opacity: 0;
  }
  100%{
    opacity: 1;
  }
}

@keyframes AppearNew {
  0%{
    transform: scale(0);
  }
  100%{
    transform: scale(1);
  }
}


// @keyframes AppearNew {
//   0%{
//     transform: scale(0);
//   }
//   20%{
//     transform: scale(1);
//   }
//   30%{
//     transform:rotate(-30deg); 
//   }
//   40%{
//     transform:rotate(30deg); 
//   }
//   50%{
//     transform:rotate(-30deg); 
//   }
//   60%{
//     transform:rotate(30deg); 
//   }
//   100%{
//     transform: scale(1);
//     transform:rotate(0); 
//   }
// }

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

@keyframes PandaSkill {
  0%{
    transform: scaleY(1);
  }
  5%{
    transform: scaleY(0.7);
  }
  10%{
    transform: scaleY(1);
  }
  15%{
    transform: scaleY(0.7);
  }
  20%{
    transform: scaleY(1);
  }
  50%{
    transform: scaleY(0.5);
  }
  70%{
    transform: scaleY(1.3);
    transform: scale(1.5);
  }
  80%{
    transform: rotate(10deg);
  }
  85%{
    transform: rotate(-10deg);
  }
  90%{
    transform: rotate(10deg);
  }
  95%{
    transform: rotate(-10deg);
    opacity: 1;
  }
  100%{
    transform: scale(1);
    transform: scaleY(1);
  }
}
</style>
