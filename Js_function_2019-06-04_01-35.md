---
tags:
  - js
---

~~~javascript
//重複削除
const positions2 = positions.filter(
  (v1, i1, a1) => a1.findIndex((v2) => v1.x === v2.x && v1.y === v2.y) === i1,
);
~~~

~~~javascript
//重複削除
var positions2 = positions.filter(function(v1,i1,a1){ 
  return (a1.findIndex(function(v2){ 
  return (v1.x===v2.x && v1.y===v2.y) 
  }) === i1);
});
~~~

~~~javascript
const finalAvtivates = [];
const store = {
get() {
return field;
},
restore() {
const history = new module.find();
field = restoreField(history);
},
set(action) {
history.push(action);
const index = finalAvtivates.findIndex(({ userId }) => action.userId);
if (index >= 0) {
finalAvtivates.push(action);
}
},
backup() {
module.set(history);
history.length = 0;
},
};
~~~

~~~javascript
initSet2(bomCount) {
const center = { x: 0, y: 0, bom: false, vacancy: 0 };
// 原点の周囲の座標追加
const directions = [[-1, -1], [0, -1], [1, -1], [1, 0], [1, 1], [0, 1], [-1, 1], [-1, 0]];
const map = directions.map(
(a, b, c) => a && { x: center.x + c[b][0], y: center.y + c[b][1], bom: false },
);
// 原点の周囲の爆弾マップ追加
const num = [0, 1, 2, 3, 4, 5, 6, 7];
for (let i = 0; i < bomCount; i += 1) {
const rn = Math.floor(Math.random() * num.length);
map[num[rn]].bom = true;
num.splice(rn, 1);
}
// 1手目の座標とその周囲の座標追加
// for (let m = 0; m < map.length; m += 1) {
// if (positions.x === map[m].x && positions.y === map[m].y) {
// map[m].bom;
// }
// }
map.unshift(center);
// console.log(positions);
// console.log(map);
return map;
},
~~~

~~~javascript
const directions = require('../../../util/directions.js')();
module.exports = (add, field) => {
console.log(field);
const JudgeDupli = field.find((d) => d.x === +add.x && d.y === +add.y);
const JudgeAdjacent = field.find(
(d) =>
directions.find(
(data) =>
!(d.x === +add.x && d.y === +add.y) &&
data[0] + d.x === +add.x &&
data[1] + d.y === +add.y,
),
0,
);
return JudgeAdjacent;

};
~~~

~~~javascript
it('原点の周囲の爆弾マップと爆弾を置ける余剰数が返ってくる', () => {
// Given
const bomCount = Math.ceil(Math.random() * 3);
// const positions = { x: 1, y: 1 };
// When
const map = bomMap.initSet2(bomCount);
// [{x,y,bom: t|f},...]
// Then
const mapMatcher = directions.map(([x, y]) => ({ x, y }));
const bomReturn = map.filter(({ bom }) => bom).length;
expect(map).toHaveLength(9);
expect(map.map(({ x, y }) => ({ x, y }))).toMatchObject(
expect.arrayContaining([initialBlock, ...mapMatcher]),
);
expect(bomReturn).toBe(bomCount);
});

~~~