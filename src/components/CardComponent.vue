<template>
  <div class="background">
    <div class="container">
      <div class="card">
        <div class="left-section">
          <img :src="filteredData.image" class="img-catalog" alt="test" />
        </div>
        <div class="right-section">
          <div class="top">
            <h1 :class="['title-catalog', { 'man-text-color': isMan }]">
              {{ filteredData.title }}
            </h1>
            <div class="category">
              <p>{{ filteredData.category }}</p>

              <div class="rating">
                <p>{{ filteredData?.rating?.rate }}/5</p>
                <div v-for="index in 5" :key="index">
                  <div v-if="index <= filteredData?.rating?.rate">
                    <img v-if="isMan" src="@/assets/img/icon/bullet-man.png" alt="star" />
                    <img v-if="isWomen" src="@/assets/img/icon/bullet-woman.png" alt="star" />
                  </div>
                  <div v-else>
                    <img v-if="isMan" src="@/assets/img/icon/bullet-hole-man.png" alt="star" />
                    <img v-if="isWomen" src="@/assets/img/icon/bullet-hole-woman.png" alt="star" />
                  </div>
                </div>
              </div>
            </div>
            <hr />
            <p class="desc-catalog">{{ filteredData.description }}</p>
          </div>
          <div class="bottom">
            <hr />
            <div class="price">
              <p
                :class="[
                  'price-text',
                  { 'man-text-color': isMan },
                  { 'women-text-color': isWomen }
                ]"
              >
                $ {{ filteredData.price }}
              </p>
            </div>
            <div class="action">
              <button
                :class="[
                  'btn-action',
                  { 'man-text-color btn-buy-man': isMan },
                  { 'women-text-color btn-buy-women': isWomen }
                ]"
              >
                Buy Now
              </button>
              <button
                :class="[
                  'btn-action',
                  { 'man-text-color': isMan },
                  { 'women-text-color': isWomen }
                ]"
                @click="nextPage"
              >
                Next Product
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: [],
      image: '@/assets/img/bg-pattern.svg',
      page: 1,
      isMan: false,
      isWomen: false
    }
  },
  methods: {
    getCatalog() {
      fetch('https://fakestoreapi.com/products/' + this.page)
        .then((response) => response.json())
        .then((data) => {
          if (data.category === "men's clothing") {
            this.data = data
            this.isMan = true
            this.isWomen = false
            this.$emit('isMan', true)
          } else if (data.category === "women's clothing") {
            this.data = data
            this.isWomen = true
            this.isMan = false
            this.$emit('isMan', false)
          } else {
            this.nextPage()
          }
        })
        .catch((error) => {
          console.error('Error:', error)
        })
    },
    nextPage() {
      if (this.page < 20) {
        this.page++
        this.getCatalog()
      } else {
        this.page = 1
        this.getCatalog()
      }
    },
    filterData() {
      return (this.data = this.data.filter(
        (item) => item.category === "men's clothing" || item.category === "women's clothing"
      ))
    }
  },
  mounted() {
    this.getCatalog()
    this.filterData()
  },
  computed: {
    filteredData() {
      return this.data
    }
  }
}
</script>

<style>
/* CSS untuk latar belakang dua warna terpotong secara vertical */
.background {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* CSS untuk kartu */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.card {
  display: flex;
  justify-content: space-between;
  align-items: space-between;
  padding: 3rem 3rem;
  background-color: white;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 70vw;
}

.left-section {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40%;
}
.right-section {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  width: 60%;
}

.category {
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  font-size: 18px;
  color: var(--color-dark-grey);
}

.rating {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.rating p {
  margin-right: 1rem;
}

.title-catalog {
  font-size: 28px;
  font-weight: 600;
  color: var(--color-text-women);
}

.man-text-color {
  color: var(--color-text-man);
}

.women-text-color {
  color: var(--color-text-women);
}

.desc-catalog {
  padding: 1rem 0;
  font-size: 20px;
  font-weight: 400;
}

.price {
  padding: 0.5rem 0;
}

.action {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.btn-action {
  padding: 0.5rem 1rem;
  margin: 0 0.5rem 0 0;
  border-radius: 8px;
  border: 0.2rem solid;
  background-color: var(--color-white);
  font-size: 20px;
  font-weight: 600;
  cursor: pointer;
  width: 50%;
}

.price-text {
  font-size: 28px;
  font-weight: 600;
  padding-bottom: 0.5rem;
}

.img-catalog {
  width: 50%;
}

.btn-buy-man {
  background-color: var(--color-text-man);
  color: var(--color-white);
  border-color: var(--color-text-man);
}

.btn-buy-women {
  background-color: var(--color-text-women);
  color: var(--color-white);
  border-color: var(--color-text-women);
}
</style>
