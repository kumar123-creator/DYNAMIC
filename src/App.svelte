<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css'

  let formFields = [];
  let formId = '72fbc0da-3810-4ad9-a922-1845f8974eb7';
  let fields = [];

  function fetchData() {
    fetch(`https://api.recruitly.io/api/candidateform/details/${formId}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`)
      .then(response => response.json())
      .then(data => {
        console.log('API Response:', data);
        formFields = Object.entries(data).map(([key, value]) => ({ label: key, value }));
      })
      .catch(error => {
        console.error('API Error:', error);
      });
  }

  function handleSubmit() {
    const inputFields = fields.filter(field => field.value !== '');
    formFields = inputFields.map(field => ({ label: field.label, value: field.value }));
    console.log('Form Fields:', formFields);
  }

  function addField() {
    fields.push({ label: '', value: '' });
  }

  function removeField(index) {
    fields.splice(index, 1);
  }

  onMount(() => {
    const inputFields = document.querySelectorAll('input[type="text"]');
    fields = Array.from(inputFields).map(field => ({ label: field.name, value: field.value }));
    fetchData();
  });
</script>

<style>
  .form-container {
    max-width: 400px;
    margin: 0 auto;
  }

  .form-container label {
    margin-bottom: 0.5rem;
    color: #555;
    font-weight: bold;
  }

  .form-container input[type="text"] {
    border: 1px solid #ccc;
    padding: 0.5rem;
    width: 100%;
    border-radius: 4px;
  }

  .form-container button {
    margin-top: 1rem;
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
  }

  .form-container button:hover {
    background-color: #0056b3;
  }

  .form-field {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
  }

  .form-field input[type="text"] {
    margin-right: 0.5rem;
  }

  .form-field button {
    background-color: #dc3545;
    margin-left: 0.5rem;
  }

  .form-field button:hover {
    background-color: #c82333;
  }
</style>

<div class="form-container">
  <form on:submit|preventDefault={handleSubmit}>
    {#each fields as field, index}
      <div class="form-field">
        <input type="text" class="form-control" bind:value={fields[index].label} placeholder="Label" />
        <input type="text" class="form-control" bind:value={fields[index].value} placeholder="Value" />
        <button type="button" class="btn btn-danger" on:click={() => removeField(index)}>Remove</button>
      </div>
    {/each}

    <div class="form-field">
      <button type="button" class="btn btn-primary" on:click={addField}>Add Field</button>
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>

