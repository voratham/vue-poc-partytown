<script setup>
import { onMounted, ref, onUnmounted } from 'vue';

// Example values for a and b
const a = 1;
const b = 2;

const countRef = ref(1)

function fetchData() {
  
    for (let i = 1; i <= 1; i++) {
      let baseUrl = "https://jsonplaceholder.typicode.com/posts/"+(0+1)
      console.log('baseUrl :',baseUrl)
      fetch(baseUrl)
      .then(response => response.json())
      .then(data => {
        console.log('Native Fetched data:', data);
        // Dispatch custom event with fetched data
        const event = new CustomEvent('fetchedDataNative', { detail: data });
        document.dispatchEvent(event);
      })
      .catch(error => {
        console.error('Error fetching data:', error);
        // Dispatch custom event with error
        const event = new CustomEvent('fetchNativeError', { detail: error });
        document.dispatchEvent(event);
      });}
}

// Hard-coded script content, including a sum method and using a and b

  


const scriptContent = `
  function sum(x, y) {
    return x + y;
  }

  // Function to fetch data from JSONPlaceholder  
  function fetchData() {
  
    for (let i = 1; i <= 1; i++) {
      let baseUrl = "https://jsonplaceholder.typicode.com/posts/"+(0+1)
      fetch(baseUrl)
      .then(response => response.json())
      .then(data => {
        // console.log('Fetched data:', data);
        // Dispatch custom event with fetched data
        const event = new CustomEvent('fetchedData', { detail: data });
        document.dispatchEvent(event);
      })
      .catch(error => {
        console.error('Error fetching data:', error);
        // Dispatch custom event with error
        const event = new CustomEvent('fetchError', { detail: error });
        document.dispatchEvent(event);
      });}
}


  // Listen for custom events to perform operations
  document.addEventListener('calculateSum', (event) => {
    const { a, b } = event.detail;
    const result = sum(a, b);
    // console.log('start loop')
    // for (let i = 0; i < 60_100_150_000 ; i ++) {
    //   if (i % 100_000_000 === 0) {
    //     console.log('still working at i :', i)
    //   }
    // }
    // console.log('end loop')
    console.log('print from party-town',result)
    fetchData()
  });
`;

const sendToPartytown = () => {
  const event = new CustomEvent('calculateSum', { detail: { a, b } });
  document.dispatchEvent(event);
};

const fetchedData = ref(null);
const fetchError = ref(null);



onMounted(() => {
  const script = document.createElement('script');
  script.type = 'text/partytown'; // Set the type to 'text/partytown'
  script.textContent = scriptContent;

  script.onload = () => {
    // Optional: Code to execute after the script is loaded
    console.log('Inline script loaded successfully.');
  };
  script.onerror = () => {
    console.error('Failed to load inline script.');
  };
  document.head.appendChild(script);

   // Add event listeners for custom events
   document.addEventListener('fetchedData', handleFetchedData);
  document.addEventListener('fetchError', handleFetchError);
  document.addEventListener('fetchedDataNative', handleFetchedData);
  document.addEventListener('fetchNativeError', handleFetchError);
});

onUnmounted(() => {
  // Remove event listeners when the component is unmounted
  document.removeEventListener('fetchedData', handleFetchedData);
  document.removeEventListener('fetchError', handleFetchError);

  document.removeEventListener('fetchedDataNative', handleFetchedData);
  document.removeEventListener('fetchNativeError', handleFetchError);
});




const handleFetchedData = (event) => {
  fetchedData.value = event.detail;
  fetchError.value = null;
};

const handleFetchError = (event) => {
  fetchError.value = event.detail;
  fetchedData.value = null;
};

</script>

<style scoped>
/* Your component styles */
</style>

<template>
  <section>
    <h1>Hello World component</h1>
    <button @click="() => sendToPartytown()" >Click run partytown</button>
    &nbsp;&nbsp;
    <button @click="() => fetchData()" >Click run native</button>
    &nbsp;&nbsp;
    <button @click="() => countRef++" >Click counter {{  countRef }}</button>

    <hr/>
    <div v-if="fetchedData">
        <h3>Fetched Data:</h3>
        <textarea style="width:400px; height:300px;">{{ JSON.stringify(fetchedData, null, 2) }}</textarea>
    </div>
    <div v-if="fetchError">
      <h3>Error:</h3>
      <pre>{{ fetchError }}</pre>
    </div>
  </section>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
