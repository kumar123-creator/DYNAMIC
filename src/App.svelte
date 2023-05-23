<script>
  import { onMount } from 'svelte';
  
  let formFields = [];
  
  onMount(async () => {
    try {
      const response = await fetch('https://api.recruitly.io/api/job?apiKey=TEST64518616D4CF145D4E20BD47169EA7229BA3');
      formFields = await response.json();
    } catch (error) {
      console.error(error);
    }
  });
  
   function handleSubmit(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    const data = Object.fromEntries(formData.entries());
    console.log('Submitted data:', data);
    // Handle form submission or API call with the form data
  }
</script>

<main>
  {#if formFields.length > 0}
    <form on:submit={handleSubmit}>
      {#each formFields as field}
        <label for={field.name}>{field.label}</label>
        {#if field.type === 'text'}
          <input type={field.type} id={field.name} name={field.name} />
        {:else if field.type === 'select'}
          <select id={field.name} name={field.name}>
            {#each field.options as option}
              <option value={option.value}>{option.label}</option>
            {/each}
          </select>
        {/if}
      {/each}
      <button type="submit">Submit</button>
    </form>
  {:else}
    <p>Loading form fields...</p>
  {/if}
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  form {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  label {
    margin-bottom: 0.5rem;
  }

  input,
  select {
    padding: 0.5rem;
    margin-bottom: 1rem;
    width: 300px;
  }

  button {
    padding: 0.5rem 1rem;
  }
</style>
