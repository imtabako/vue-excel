<script setup>
import { computed, ref } from 'vue';

const excel_table = ref([]);
const arrays = ref([]);

// Столбцы и их наименования
const B = ref('Н'); // 
const b = ref([]);
// console.log(b.value.length)
excel_table.value.push({'title': B, 'arr': b});
arrays.value.push(b);

const C = ref('Артикул'); // 
const c = ref([]);
console.log(c.value.length)
excel_table.value.push({'title': C, 'arr': c});
arrays.value.push(c);

const D = ref('Наименование'); // 
const d = ref([]);
excel_table.value.push({'title': D, 'arr': d});
arrays.value.push(d);

const E = ref('Площадь ед.'); // 
const e = ref([]);
excel_table.value.push({'title': E, 'arr': e});
arrays.value.push(e);

const F = ref('Вес ед.'); // 
const f = ref([]);
excel_table.value.push({'title': F, 'arr': f});
arrays.value.push(f);

const G = ref('Общий вес'); // 
const g = ref([]);
excel_table.value.push({'title': G, 'arr': g});
arrays.value.push(g);

const H = ref('Кол-во, шт.'); // 
const h = ref([]);
excel_table.value.push({'title': H, 'arr': h});
arrays.value.push(h);

const I = ref('s'); // 
const i = ref([]);
excel_table.value.push({'title': I, 'arr': i});
arrays.value.push(i);

const J = ref('Себестоимость'); // 
const j = ref([]);
excel_table.value.push({'title': J, 'arr': j});
arrays.value.push(j);

const K = ref('ИТОГО'); // 
const k = ref([]);
excel_table.value.push({'title': K, 'arr': k});
arrays.value.push(k);

const L = ref('Стоимость, руб. (шт)'); // 
const l = ref([]);
excel_table.value.push({'title': L, 'arr': l});
arrays.value.push(l);

const M = ref('Стоимость, руб. (Общая)'); // 
const m = ref([]);
excel_table.value.push({'title': M, 'arr': m});
arrays.value.push(m);

const N = ref('Стоимость аренды ед. в сутки, руб.'); // 
const n = ref([]);
excel_table.value.push({'title': N, 'arr': n});
arrays.value.push(n);

const O = ref('Аренда комплекта в сутки, руб.'); // 
const o = ref([]);
excel_table.value.push({'title': O, 'arr': o});
arrays.value.push(o);

const P = ref('Кол-во дней'); // 
const p = ref([]);
excel_table.value.push({'title': P, 'arr': p});
arrays.value.push(p);

const Q = ref('Стоимость аренды в месяц, руб'); // 
const q = ref([]);
excel_table.value.push({'title': Q, 'arr': q});
arrays.value.push(q);

const R = ref('20,00'); // 
const r = ref([]);
excel_table.value.push({'title': R, 'arr': r});
arrays.value.push(r);

const X = ref('%'); // 
const x = ref([]);
excel_table.value.push({'title': X, 'arr': x});
arrays.value.push(x);

// from here
const maxIndex = ref(0);
const titles = ref([B, C, D, E, F, G, H, I, J, K, L, M, N, O, P, Q, R, X]);
const columns = ref([b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, x]);

const colSize = ref(18);
const formulaIndecies = ref([5, 7, 9, 10, 11, 13, 15, 16]);
const sumIndecies = ref([5, 7, 9, 11, 13, 15]);

const rows = ref([]);

const g_sum = ref(0);
const i_sum = ref(0);
const k_sum = ref(0);
const m_sum = ref(0);
const o_sum = ref(0);
const q_sum = ref(0);
const q_last = ref(0);

const l_days = ref(0);
const m_price = ref(0);
const l_days2 = ref(0);
const m_price2 = ref(0);

// computed
function formulaValue(row_index, col_index) {
  var result = 0;
  var row = rows.value[row_index];

  switch(col_index) {
    case 5:
      result = row[4] * row[6]; // F$ * H$
      break;
    case 7:
      result = row[3] * row[6]; // E$ * H$
      break;
    case 9:
      result = row[8] * row[6]; // J$ * H$
      break;
    case 10:
      result = row[8] * row[17]; // J$ * X$
      break;
    case 11:
      result = row[10] * row[6]; // L$ * H$
      break;
    case 13:
      result = row[12] * row[6]; // L$ * H$;
      break;
    case 15:
      result = row[13] * row[6]; // O$ * 30
      break;
    case 16:
      if (row_index == 0) {      // R$-1
        result = 20.0;
      } else {
        result = rows.value[row_index-1][col_index];
      }
      break;
  }

  if (isNaN(result)) {
    result = 0;
  }

  rows.value[row_index][col_index] = result;

  return result;
}

function showthemoney() {
  console.log(columns.value);
  console.log(rows.value);
}

function addRow() {
  // columns.value.forEach(arr => arr.value.push(''))
  var empty_row = ref(['', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '']);

  rows.value.push(empty_row.value);
  console.log(rows.value);

  maxIndex.value++;
}

function sumRow(index) {
  var result = rows.value.reduce((total, row) => total + row[index], 0)

  switch(index) {
    case 5:
      g_sum.value = result;
      break;
    case 7:
      i_sum.value = result;
      break;
    case 9:
      k_sum.value = result;
      break;
    case 11:
      m_sum.value = result;
      break;
    case 13:
      o_sum.value = result;
      break;
    case 15:
      q_sum.value = result;
      break;
  }

  q_last.value = q_sum / i_sum;
  return result;
}

addRow();

</script>

<template>
  <main>
    <h1><b>JOPA</b></h1>
    <button @click="showthemoney">AAAAAAAAA</button>

    <table class="table table-bordered align-middle">
      <caption>
        <button class="btn btn-success" @click="addRow">+</button>
      </caption>
      <!--  -->
      <thead>
        <tr>
          <th 
          :class="[ index == 2 && 'thick-left', index == 15 && 'thick-right' ]"
          scope="col" v-for="(title, index) in titles">{{ title.value }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, row_index) in rows">
          <td 
          :class="[ index == 2 && 'thick-left', index == 15 && 'thick-right' ]"
          v-for="(value, index) in row">
            <template v-if="formulaIndecies.includes(index)">
              {{ formulaValue(row_index, index) }}
            </template>
            <template v-else>
              <input class="container-fluid border-0" 
                :class="{ 'text-center': index != 2 }"
                v-model="row[index]"/>
            </template>
          </td>
        </tr>
        <!-- Sum of some columns -->
        <tr>
          <td
            :class="[ index == 2 && 'thick-left', index == 15 && 'thick-right', index >=2 && index <= 15 && 'thick-bottom',
                      index == 0 && 'thick-bottom thick-right' ]"
            v-for="(col, index) in columns">
            <template v-if="index == 0">
              Итог
            </template>
            <template v-if="index == 2">
              Итого:
            </template>
            <template v-if="sumIndecies.includes(index)">
              {{ sumRow(index) }}
            </template>
          </td>
        </tr>
        <!-- Empty with 750 -->
        <tr>
          <td class="thin-left"></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td class="thin-right">{{ q_sum / i_sum || '-' }}</td>
          <td></td>
          <td></td>
        </tr>
        <!-- Empty with weight -->
        <tr>
          <td class="thin-left"></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td>Вес</td>
          <td class="thin-right">{{ g_sum }}</td>
          <td></td>
          <td></td>
        </tr>
        <!-- Additional table with data -->
        <tr><!-- Head -->
          <td class="thin-left"></td>
          <td></td>
          <td class="text-center border-2 border-dark">Наименование</td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark" colspan="2">ед.</td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark">Кол-во дней</td>
          <td class="text-center border-2 border-dark">Стоимость</td>
          <td class="text-center border-2 border-dark">Итого:</td>
          <td></td>
          <td></td>
          <td class="thin-right"></td>
          <td></td>
          <td></td>
        </tr>
        <tr> <!-- Body -->
          <td class="thin-left"></td>
          <td></td>
          <td class="text-center border-2 border-dark">Арендная плата</td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark" colspan="2"><input class="text-center container-fluid border-0" value="сут"/></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"><input class="text-center container-fluid border-0" v-model="l_days"/></td>
          <td class="text-center border-2 border-dark"><input class="text-center container-fluid border-0" v-model="m_price"/></td>
          <td class="text-center border-2 border-dark">{{ l_days * m_price || '-' }}</td>
          <td></td>
          <td></td>
          <td class="thin-right"></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td class="thin-left"></td>
          <td></td>
          <td class="text-center border-2 border-dark">Обеспечительный взнос</td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark" colspan="2"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark">{{ q_sum || '-' }}</td>
          <td></td>
          <td></td>
          <td class="thin-right"></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td class="thin-left"></td>
          <td></td>
          <td class="text-center border-2 border-dark">Транспортные расходы (в две стороны)</td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark" colspan="2"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"></td>
          <td class="text-center border-2 border-dark"><input class="text-center container-fluid border-0" v-model="l_days2"/></td>
          <td class="text-center border-2 border-dark"><input class="text-center container-fluid border-0" v-model="m_price2"/></td>
          <td class="text-center border-2 border-dark">{{ l_days2 * m_price2 || '-' }}</td>
          <td></td>
          <td></td>
          <td class="thin-right"></td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td class="thin-bottom thin-left"></td>
          <td class="thin-bottom"></td>
          <td class="thin-bottom text-center border-2 border-dark">Итого к оплате</td>
          <td class="thin-bottom text-center border-2 border-dark"></td>
          <td class="thin-bottom text-center border-2 border-dark"></td>
          <td class="thin-bottom text-center border-2 border-dark"></td>
          <td class="thin-bottom text-center border-2 border-dark" colspan="2"></td>
          <td class="thin-bottom text-center border-2 border-dark"></td>
          <td class="thin-bottom text-center border-2 border-dark"></td>
          <td class="thin-bottom text-center border-2 border-dark"></td>
          <td class="thin-bottom text-center border-2 border-dark"></td>
          <td class="thin-bottom text-center border-2 border-dark">{{ l_days * m_price + q_sum + l_days2 * m_price2 || '' }}</td>
          <td class="thin-bottom"></td>
          <td class="thin-bottom"></td>
          <td class="thin-bottom thin-right"></td>
          <td></td>
          <td></td>
        </tr>
      </tbody>
    </table>
  </main>
</template>

<script>
</script>

<style scoped>
tr td {
  padding: 0 !important;
}

.thick-right {
  border-right: 3px solid black;
}

.thick-left {
  border-left: 3px solid black;
}
.thick-bottom {
  border-bottom: 3px solid black;
}
.thin-right {
  border-right: 2px solid black;
}

.thin-left {
  border-left: 2px solid black;
}
.thin-bottom {
  border-bottom: 2px solid black;
}

.kp {
  background-color: black;
}
</style>