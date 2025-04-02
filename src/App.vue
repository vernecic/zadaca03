<template>
  <div>
    <h3>Košarica</h3>
    <div class="inputi">
      <form @submit.prevent>
        <label for="naziv">Naziv proizvoda</label>
        <input type="text" placeholder="Upiši naziv proizvoda:" v-model="noviProizvod" />
      </form>
      <form @submit.prevent>
        <label for="naziv">Cijena proizvoda</label>
        <input
          type="number"
          step="any"
          placeholder="Upiši cijenu proizvoda:"
          v-model="novaCijena"
        />
      </form>
      <button @click="dodajProizvod" :disabled="!noviProizvod" class="btn-dodaj">
        Dodaj artikl
      </button>
    </div>
    <div class="kategorije">
      <h4>Naziv</h4>
      <h4>Količina</h4>
      <h4>Cijena</h4>
      <h4>Ukupno</h4>
    </div>
    <div v-if="kosarica.length === 0">
      <p>Košarica je prazna!</p>
    </div>
    <div v-else>
      <div v-for="(proizvod, index) in kosarica" :key="index" class="proizvodi">
        <div>{{ proizvod.ime }}</div>
        <div>
          <button @click="smanjiKol(index)">-</button>{{ proizvod.kolicina }}
          <button @click="povecajKol(index)">+</button>
        </div>
        <div>{{ proizvod.cijena }}</div>
        <div class="ukupno">{{ proizvod.cijena * proizvod.kolicina }}€</div>
        <div class="ukloni" @click="ukloniProizvod">Ukloni</div>
      </div>
    </div>
    <div class="ukupnaCijena">UKUPNO: {{ ukupnaCijena }}€</div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const kosarica = ref([])
const noviProizvod = ref('')
const novaCijena = ref('')

const dodajProizvod = () => {
  if (!noviProizvod.value || !novaCijena.value || novaCijena.value <= 0) return

  const proizvodPostoji = kosarica.value.find((proizvod) => proizvod.ime === noviProizvod.value)
  if (proizvodPostoji) {
    proizvodPostoji.kolicina++
  } else {
    kosarica.value.push({ ime: noviProizvod.value, cijena: novaCijena.value, kolicina: 1 })
  }

  noviProizvod.value = ''
  novaCijena.value = ''
}

const smanjiKol = (index) => {
  if (kosarica.value[index].kolicina > 1) {
    kosarica.value[index].kolicina--
  }
}
const povecajKol = (index) => {
  kosarica.value[index].kolicina++
}

const ukloniProizvod = (index) => {
  kosarica.value.splice(index, 1)
}

const ukupnaCijena = computed(() => {
  return kosarica.value.reduce((acc, cur) => acc + cur.cijena * cur.kolicina, 0)
})
</script>

<style scoped>
:global(body) {
  background-color: white;
  color: black;
}

h3 {
  font-size: 1.5rem;
  font-weight: bold;
}

.inputi {
  display: flex;
  border-top: 1px solid black;
  border-bottom: 1px solid black;
  padding: 0.5rem;
}

.btn-dodaj {
  padding: 0.5rem;
  width: 150px;
  border-radius: 20px;
  border: none;
  background-color: rgb(35, 226, 35);
}

.kategorije {
  display: grid;
  grid-template-columns: 2fr 2fr 1fr 1fr 1fr;
  padding: 0.5rem;
  align-items: center;
}

.proizvodi {
  display: grid;
  grid-template-columns: 2fr 2fr 1fr 1fr 1fr;
  padding: 1rem 0.5rem;
  border-bottom: 1px solid #f5f5f5;
  align-items: center;
}

.ukupno {
  margin-left: 100px;
}

.ukloni {
  color: red;
  margin-left: 100px;
  cursor: pointer;
}
</style>
