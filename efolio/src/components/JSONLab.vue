<template>
  <div class="json-lab">
    <h1>🗄️ JSON Data & Vue Directives Lab</h1>

    <section class="lab-section">
      <h2>📚 Working with JSON Arrays</h2>
      <p>Our <code>authors.json</code> contains an array of author objects.</p>

      <h3>Iterating through Arrays</h3>
      <ul>
        <li
          v-for="a in authors"
          :key="a.id"
          :title="`Author: ${a.name} • Born: ${a.birthYear}`"
          :class="{ highlight: a.name === 'George Orwell' }"
          :style="
            a.name === 'George Orwell'
              ? { backgroundColor: '#fff3cd', border: '1px solid #f0ad4e' }
              : {}
          "
        >
          <strong>{{ a.name }}</strong> ({{ a.birthYear }})
        </li>
      </ul>

      <h3>Filtering Arrays</h3>
      <p>Authors born after 1850:</p>
      <ul>
        <li v-for="a in modernAuthors" :key="a.id">
          {{ a.name }} ({{ a.birthYear }})
        </li>
      </ul>

      <h3>Mapping Arrays</h3>
      <p>Famous works:</p>
      <ul>
        <li v-for="t in allFamousWorks" :key="t">{{ t }}</li>
      </ul>

      <h3>Finding in Arrays</h3>
      <p>Finding by property: {{ orwell?.name }}</p>

      <h3>Nested Arrays/Objects</h3>
      <p>{{ austen?.name }}'s works:</p>
      <ul>
        <li v-for="(w, i) in austen?.famousWorks || []" :key="i">
          {{ w.title }} ({{ w.year }})
        </li>
      </ul>
    </section>

    <section class="lab-section">
      <h2>🏢 Working with JSON Objects</h2>
      <p>Our <code>bookstores.json</code> is a JSON object.</p>

      <h3>Accessing Properties</h3>
      <p>
        Company: <strong>{{ bookstore.name }}</strong>
      </p>
      <p>
        Total Stores: <strong>{{ bookstore.totalStores }}</strong>
      </p>

      <h3>Iterating Object Properties</h3>
      <p>Store Types:</p>
      <ul>
        <li v-for="([type, count], i) in storeTypeEntries" :key="i">
          {{ type }}: {{ count }}
        </li>
      </ul>

      <h3>Nested Objects</h3>
      <p>Opening Hours:</p>
      <ul>
        <li v-for="(hours, key) in bookstore.openingHours" :key="key">
          {{ key }}: {{ hours.open }} – {{ hours.close }}
        </li>
      </ul>

      <h3>Working with Arrays in Objects</h3>
      <p>Top sellers:</p>
      <ul>
        <li v-for="(title, i) in bookstore.topSellers" :key="i">{{ title }}</li>
      </ul>
      <p>We operate in: {{ bookstore.countries.join(", ") }}</p>
      <p>
        Our #1 seller: <strong>{{ bookstore.topSellers[0] }}</strong>
      </p>
    </section>

    <section class="lab-section">
      <h2>v-if & v-else</h2>
      <p>Toggle visibility based on a condition.</p>
      <button @click="showMessage = !showMessage">Toggle Message</button>
      <p v-if="showMessage" class="message success">
        ✨ You're a Vue superstar! ✨
      </p>
      <p v-else>Click the button to see a message.</p>
    </section>

    <section class="lab-section">
      <h2>Attribute, Class and Style Binding with <code>v-bind</code></h2>
      <p>George Orwell is highlighted above in the first list.</p>
    </section>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import authors from "../assets/json/authors.json";
import bookstore from "../assets/json/bookstores.json";

const showMessage = ref(false);

const modernAuthors = computed(() =>
  (Array.isArray(authors) ? authors : []).filter(
    (a) => Number(a.birthYear) > 1850
  )
);

const allFamousWorks = computed(() =>
  (Array.isArray(authors) ? authors : [])
    .flatMap((a) => (Array.isArray(a.famousWorks) ? a.famousWorks : []))
    .map((w) => w.title)
);

const orwell = computed(() =>
  (Array.isArray(authors) ? authors : []).find(
    (a) => a.name === "George Orwell"
  )
);

const austen = computed(() =>
  (Array.isArray(authors) ? authors : []).find((a) => a.id === 1)
);

const storeTypeEntries = computed(() =>
  Object.entries(bookstore.storeTypes || {})
);
</script>

<style scoped>
.json-lab {
  font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
  max-width: 80vw;
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
h1,
h2 {
  color: #333;
}
h1 {
  text-align: center;
}
.lab-section {
  background: #fff;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.message {
  padding: 10px;
  border-radius: 5px;
  margin-top: 10px;
}
.success {
  background: #e7faf3;
  color: #42b883;
  border: 1px solid #42b883;
}
.highlight {
  font-weight: 700; /* Now works with inline yellow highlight */
}
ul {
  list-style: none;
  padding: 0;
}
li {
  background: #f0f0f0;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
}
</style>
