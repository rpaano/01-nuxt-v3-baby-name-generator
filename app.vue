<script setup lang="ts">
  import { Gender, Popularity, Length, names } from "@/data"

  interface OptionState {
    gender: Gender;
    popularity: Popularity;
    length: Length;
  }

  const options = reactive<OptionState>({
    gender: Gender.GIRL,
    popularity: Popularity.TRENDY,
    length: Length.LONG,
  })

  const selectedNames = ref<string[]>([]);

  const computeSelectedNames = () => {
    const filteredNames = names.filter((name) => name.gender === options.gender)
                        .filter((name) => name.popularity === options.popularity)
                        .filter((name) => {
                          if(options.length === Length.ALL) return true
                          return name.length === options.length;
                        })

    selectedNames.value = filteredNames.map(name => name.name)
  }

  const removeName = (index: number) => {
    const filteredNames = [...selectedNames.value]
    filteredNames.splice(index, 1)
    selectedNames.value = filteredNames
  }

  const optionArray = [
    {
      title: "1) Choose a gender",
      category: "gender",
      buttons: [Gender.BOY, Gender.UNISEX, Gender.GIRL],
    },
    {
      title: "2) Choose name popularity",
      category: "popularity",
      buttons: [Popularity.TRENDY, Popularity.UNIQUE],
    },
    {
      title: "3) Choose name's length",
      category: "length",
      buttons: [Length.LONG, Length.ALL, Length.SHORT],
    },
  ]
</script>

<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" button below</p>
    <div class="options-container">
      <Option 
        v-for="option in optionArray" 
        :key="option.title+option.category" 
        :option="option" 
        :optionSelect="options"
      />
      <button class="primary" @click="computeSelectedNames">Find Names</button>
    </div>
    <div class="cards-container">
      
      <CardName 
        v-for="(name, index) in selectedNames" 
        :key="name" 
        :name="name" 
        class="card"
        @remove="() => removeName(index)"
        :index="index"
      />
    </div>
  </div>
</template>


<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

.container h1 {
  font-size: 3rem;
}

.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  position: relative;
}

.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  font-size: 1rem;
  padding: 0.75rem 4rem;
  margin-top: 1rem;
  cursor: pointer;
}

.cards-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}
</style>
