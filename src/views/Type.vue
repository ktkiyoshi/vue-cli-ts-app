<template>
  <div>
    <table>
      <tr>
        <td class="none">&nbsp;</td>
        <th v-for="type in types" :key="type.id"
          :style="{ 'background-color' : '#' + type.color }">
          {{ type.short_name }}
        </th>
      </tr>
      <tr v-for="type in types" :key="type.id">
        <th :style="{ 'background-color' : '#' + type.color }">
          {{ type.short_name }}
        </th>
        <td v-for="(relation, idx) in type.attack_relations" :key="idx"
          :class="[
            relation.effect_rate == 2? 'red': '',
            relation.effect_rate == 0.5? 'blue': '',
            relation.effect_rate == 0? 'bold': ''
          ]"
        >
          {{ toSymbol(relation.effect_rate) }}
        </td>
      </tr>
    </table>
  </div>
</template>
<script lang="ts">
import axios from 'axios';
import { Component, Vue } from 'vue-property-decorator';
@Component({
  components: {
  },
})
export default class Type extends Vue {
  // data
  public types: Array<{
    id: number,
    name: string
    short_name: string,
    color: string,
    attack_relations: Array<{
      attack_id: number,
      defense_id: number,
      effect_rate: number;
    }>;
  }> = [];

  // methods
  public toSymbol(rate: number) {
    let symbol: string;
    switch (rate) {
      case 2:   symbol = '●';
                break;
      case 0.5: symbol = '▲';
                break;
      case 0:   symbol = '×';
                break;
      default:  symbol = '';
    }
    return symbol;
  }

  // created
  public created(): void {
    axios.get(process.env.VUE_APP_DOMAIN + '/v1/types')
      .then((response) => {
        this.types = response.data;
      });
  }
}
</script>
<style scoped>
* {
  font-family: 'Hiragino Kaku Gothic Pro','ヒラギノ角ゴ Pro W3','メイリオ',Meiryo,'ＭＳ Ｐゴシック',sans-serif;
}
table {
  border-collapse:separate;
  border-spacing: 3px;
  margin: 0 auto;
}
table th,table td {
  border-radius: 3px;
  text-align: center;
  padding: 7px 10px;
}
table th {
  background-color: #c79852;
  color: white;
  border:solid 1px #927141;
}
table td {
  background-color: #e4d4bc;
  border:solid 1px #af9d85;
}
.none {
  background-color: white;
  border: none;
}
.red  { color: red; }
.blue { color: blue; }
.bold { font-weight: bold; }
</style>