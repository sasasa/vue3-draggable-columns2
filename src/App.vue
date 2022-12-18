<template>
  <div class="container">
    <div id="box1" class="box">
      <draggable
        @end="onEnd"
        v-model="items1"
        item-key="no"
        tag="ul"
        group="ITEMS"
      >
        <template v-slot:item="{ element, index }">
          <li>
            {{ element.name }}-(No.{{ element.no }})
            <span class="del" v-on:click="doDelete1(index)">[削除]</span>
          </li>
        </template>
      </draggable>
    </div>
    <div id="box2" class="box">
      <draggable
        @end="onEnd"
        v-model="items2"
        item-key="no"
        tag="ul"
        group="ITEMS"
      >
        <template v-slot:item="{ element, index }">
          <li>
            {{ element.name }}-(No.{{ element.no }})
            <span class="del" v-on:click="doDelete2(index)">[削除]</span>
          </li>
        </template>
      </draggable>
    </div>
    <div id="box3" class="box">
      <draggable
        @end="onEnd"
        v-model="items3"
        item-key="no"
        tag="ul"
        group="ITEMS"
      >
        <template v-slot:item="{ element, index }">
          <li>
            {{ element.name }}-(No.{{ element.no }})
            <span class="del" v-on:click="doDelete3(index)">[削除]</span>
          </li>
        </template>
      </draggable>
    </div>

    <div class="box">
      <code>
        <pre>{{ items1 }}</pre>
      </code>
    </div>
    <div class="box">
      <code>
        <pre>{{ items2 }}</pre>
      </code>
    </div>
    <div class="box">
      <code>
        <pre>{{ items3 }}</pre>
      </code>
    </div>
  </div>
  <button v-on:click="doAdd">追加</button>
</template>

<script setup>
import draggable from "vuedraggable";
import { ref } from "vue";

const items1 = ref([
  { no: 1, name: "キャベツ", categoryNo: "1" },
  { no: 2, name: "ステーキ", categoryNo: "2" },
]);
const items2 = ref([
  { no: 5, name: "きゅうり", categoryNo: "1" },
  { no: 6, name: "ハンバーグ", categoryNo: "2" },
]);
const items3 = ref([
  { no: 7, name: "ごはん", categoryNo: "1" },
  { no: 8, name: "ぱん", categoryNo: "2" },
]);
const onEnd = (originalEvent) => {
  console.log(originalEvent); //originalEventは イベントを取得できる
};
const newNo = ref(null);
const doAdd = () => {
  let no = 0;
  // itemsの中の一番大きなnoを取得して１を足す
  if (items1.value.concat().length > 0) {
    no =
      Math.max.apply(null, [
        ...items1.value.concat().map(function (item) {
          return item.no;
        }),
        ...items2.value.concat().map(function (item) {
          return item.no;
        }),
        ...items3.value.concat().map(function (item) {
          return item.no;
        }),
      ]) + 1;
    newNo.value = newNo.value < no ? no : newNo.value;
  }
  // itemsにアイテムを追加
  items1.value.push({
    no: newNo.value,
    name: "追加リスト" + newNo.value,
    categoryNo: "5",
  });
};
const doDelete1 = (index) => {
  items1.value.splice(index, 1);
};
const doDelete2 = (index) => {
  items2.value.splice(index, 1);
};
const doDelete3 = (index) => {
  items3.value.splice(index, 1);
};
</script>

<style>
.container {
  display: flex;
  flex-wrap: wrap;
}
.box {
  width: 32%;
  padding: 20px 0;
}
#box1 {
  background-color: #fdd;
}
#box2 {
  background-color: #ddf;
}
#box3 {
  background-color: #dfd;
}
ul {
  list-style-type: none;
  padding-right: 2rem;
}
li {
  cursor: pointer;
  padding: 10px;
  border: solid #ddd 1px;
  background-color: #fff;
}
</style>
