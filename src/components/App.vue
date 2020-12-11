<template>
  <div>
    <h1>マルバツゲーム App</h1>

      <table id="table">
        <tr v-for="(row,rowsIndex) in states" :key="rowsIndex">
          <td v-for="(state,colsIndex) in row" :key="colsIndex" @click="onSelect(rowsIndex,colsIndex)">
            <div style="color:#f00;" v-if="state==1">○</div>
            <div style="color:#00f;" v-if="state==2">×</div>
          </td>
        </tr>
      </table>

      <div style="text-aligin:center;">
        <div style="color:#f00;" v-if="playerId==1">「○　プレイヤーさん、マスを選んでください。」</div>
        <div style="color:#00f;" v-if="playerId==2">「×　プレイヤーさん、マスを選んでください。」</div>
      </div>
  </div>
</template>

<script>
export default{
  data: function() {
    return {
      states: [
        [-1,-1,-1], //rowの1つ目 配列の中の配列はループ 処理2回で取り出せる
        [-1,-1,-1], //rowの2つ目
        [-1,-1,-1] //rowの3つ目
        ],
      playerId: 1
    }
  },
  methods: {
    onSelect: function(rowsIndex, colsIndex) {
      if(this.states[rowsIndex][colsIndex] != -1) {
        alert('そのマスは、すでに選択されています!');
      }else{
        let states = JSON.parse(JSON.stringify(this.states)) //JSON.stringfy() エンコード（JS→JSON）　JSON.parse デコード（JSON→JS）
        states[rowsIndex][colsIndex] = this.playerId; //playerID
        this.states = states;
        this.playerId = (this.playerId == 1) ? 2 : 1;

        let winnerId = this.getWinnerId();
        if(winnerId != -1) {
          this.states = [
            [-1,-1,-1],
            [-1,-1,-1],
            [-1,-1,-1]
          ];
          this.playerIds = {
            1:'○',
            2:'×'
          };
          alert(this.playerIds[winnerId] + 'さんの勝ちです。おめでとうございます。!');
        }
      }
    }
  },
  created(){
    console.log(this.states,this.playerId)
  },
  watch:{
    states(newStates, oldStates){
      console.log(newStates, oldStates)
    },
    playerId(newPkayerID, oldPlayerId){
      console.log(`現在のプレーヤーは${newPkayerID}の人です。一つ前は ${oldPlayerId}の人です。`)
    }
  }
};
</script>

<style scoped>
table {
  margin: 0 auto;
  border-collapse: collapse;
  border: 3px solid #ccc;
}
td {
  border:1px solid #ccc;
  height: 80px;
  width: 80px;
  text-align: center;
  vertical-align: middle;
  font-size: 75px;
  cursor: pointer;
}

</style>