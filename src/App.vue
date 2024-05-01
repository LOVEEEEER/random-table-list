<template>
  <div class="app">
    <div class="vertical-list">
      <div class="horizontal-list" v-for="(hList, index) in verticalList" :key="index">
        <div class="square" v-for="(num, i) in hList" :key="i" @mouseover="onMouseOver" @mouseleave="onMouseLeave">
          {{ num }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      verticalList: [],
      randomNumbers: [],
    };
  },
  created() {
    this.generateRandomNumbers();
    this.generateVerticalList();

    setInterval(() => {
      this.updateNumbers();
    }, 1000);
  },
  methods: {
    generateRandomNumber(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    generateRandomNumbers() {
      this.randomNumbers = Array.from({ length: 200 }, () => this.generateRandomNumber(0, 100));
    },
    generateVerticalList() {
      this.verticalList = Array.from({ length: 200 }, () => {
        const length = this.generateRandomNumber(10, 30);
        return this.randomNumbers.slice(0, length);
      });
    },
    updateNumbers() {
      const startIndex = Math.floor(window.scrollY / 50);
      const endIndex = startIndex + Math.ceil(window.innerHeight / 50);

      for (let i = startIndex; i < endIndex && i < this.verticalList.length; i++) {
        const hList = this.verticalList[i];
        const index = this.generateRandomNumber(0, hList.length - 1);
        hList[index] = this.generateRandomNumber(0, 100);
        this.verticalList[i] = hList;
      }
    },
    onMouseOver(event) {
      event.target.style.transform = 'scale(0.8)';
    },
    onMouseLeave(event) {
      event.target.style.transform = 'scale(1)';
    },
  },
};
</script>

<style>
.app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.vertical-list {
  display: flex;
  flex-direction: column;
}

.horizontal-list {
  display: flex;
  flex-direction: row;
}

.square {
  width: 40px;
  height: 40px;
  border: 1px solid black;
  border-radius: 10px;
  margin: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: transform 0.3s;
}
</style>

