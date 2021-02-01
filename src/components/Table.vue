<template>
  <div id="wrapper">
    <div class="btn">
      <button @click="getData">Get data</button>
    </div>
    <div v-if="show" class="table">
      <div class="row__head">
        <div class="cell__head">
          <span @click="sortIt" class="arrow" v-if="sorted">&#8657;</span>
          <span @click="sortIt" class="arrow" v-else>&#8659;</span>Stock
        </div>
        <div class="cell__head">Current</div>
        <div class="cell__head">Change</div>
      </div>
      <div v-for="(items, i) of mocData" :key="i" class="row row__body">
        <div v-for="(item, index) of items" :key="index"
             :class="items[index] < 0 && index === 2 ? 'red row__cell' : items[index] > 0 && index === 2 ? 'green row__cell' : 'row__cell'">
          {{ typeof items[index] === "number" ? items[index].toFixed(2) : items[index] }}
        </div>
      </div>
    </div>
    <div v-else-if="failData" class="alternative">No data</div>
    <div v-else-if="loading" class="alternative">Loading...</div>
  </div>
</template>

<script>
import getDataFunc from "@/plugins/getDataFunc";
import {payload} from "@/mocData";

export default {
  name: "wrapper",
  data() {
    return {
      mocData: [],
      show: false,
      failData: false,
      loading: false,
      sorted: false
    }
  },
  methods: {
    async getData() {
      this.failData = false;
      this.loading = true;
      this.show = false;
      try {
        this.mocData = [];
        const response = await getDataFunc(payload);
        for (const item in response) {
          this.mocData.push(response[item]);
        }
        let ara = [];
        let col1 = [];
        let col2 = [];
        let col3 = [];
        this.mocData.forEach(data => {
          data.forEach((value, i) => {
            if (i === 0) {
              col1.push(value);
            } else if (i === 1) {
              col2.push(value);
            } else {
              col3.push(value);
            }
          })
        })
        ara.push(col1, col2, col3);
        this.mocData = ara;
        this.failData = false;
        this.show = true;
        this.loading = false;
      } catch (e) {
        console.log('Упс');
        this.failData = true;
        this.loading = false;
      }
    },
    sortFunc(a, b) {
      if (this.sorted) {
        if (a[0] < b[0]) {
          return -1;
        }
        if (a[0] > b[0]) {
          return 1;
        }
        return 0;
      } else {
        if (a[0] > b[0]) {
          return -1;
        }
        if (a[0] < b[0]) {
          return 1;
        }
        return 0;
      }
    },
    sortIt() {
      this.mocData.sort(this.sortFunc);
      this.sorted = !this.sorted;
    }
  }
}
</script>

<style lang="scss" scoped>
#wrapper {
  min-width: 360px;
  border: 1px solid silver;
  text-align: center;
  padding: 50px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  .btn {
    margin-bottom: 20px;

    button {
      background-color: rgba(204, 204, 204, 0.2);
      border-radius: 8px;
      padding: 5px;
      border: 1px solid silver;
      outline: none;
      font-size: 18px;
      font-weight: 500;
      cursor: pointer;
      opacity: 0.9;

      &:hover {
        opacity: 1;
      }
    }
  }

  .table {
    font-family: "Trebuchet MS", sans-serif;
    border: 1px solid silver;
    min-width: 70%;
    margin: 0 auto;

    .row__head {
      background-color: rgba(204, 204, 204, 0.2);
      display: flex;
      flex-direction: row;

      .stock {
        position: relative;

        .up {
          position: absolute;
          top: 0;
          left: 0;
        }

        .down {
          position: absolute;
          top: 0;
          left: 0;
        }
      }

      .cell__head {
        flex: 1 1 100px;
        border: 1px solid silver;
        padding: 5px;
        font-weight: bold;
        font-size: 1.1em;
        min-width: 100px;

        .arrow {
          margin-right: 15px;
          color: gray;
          cursor: pointer;
        }
      }
    }

    .row {
      display: flex;
      flex-direction: row;

      .row__cell {
        display: flex;
        flex-direction: column;
        flex: 1 1 100px;
        border: 1px solid silver;
        padding: 5px;
        min-width: 100px;

        &.red {
          color: red;
        }

        &.green {
          color: green;
        }
      }
    }

  }
}
</style>
