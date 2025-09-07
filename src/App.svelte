<script>
  import { onMount } from 'svelte';

  const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

  const API_ENDPOINTS = {
    students: `${API_BASE_URL}/students`,
    guardians: `${API_BASE_URL}/guardians`,
    authorizations: `${API_BASE_URL}/authorizations`,
    authorization: `${API_BASE_URL}/authorization`
  };

  let students = [];
  let error_students = '';

  onMount(async () => {
    try {
      const res = await fetch(API_ENDPOINTS.students);
      if (!res.ok) throw new Error(await res.text());
      students = await res.json();
    } catch (err) {
      error_students = err.message;
    }
  });

  let guardians = [];
  let error_guardians = '';

  onMount(async () => {
    try {
      const res = await fetch(API_ENDPOINTS.guardians);
      if (!res.ok) throw new Error(await res.text());
      guardians = await res.json();
    } catch (err) {
      error_guardians = err.message;
    }
  });

  let authorizations = [];
  let error_authorizations = '';

  onMount(async () => {
    try {
      const res = await fetch(API_ENDPOINTS.authorizations);
      if (!res.ok) throw new Error(await res.text());
      authorizations = await res.json();
    } catch (err) {
      error_authorizations = err.message;
    }
  });

  let selectedAuthorization = null;
  let error_authorization_details = '';

  async function loadAuthorizationDetails(id) {
    selectedAuthorization = null;
    error_authorization_details = '';
    try {
      const res = await fetch(`${API_ENDPOINTS.authorization}/${id}`);
      if (!res.ok) throw new Error(await res.text());
      selectedAuthorization = await res.json();
    } catch (err) {
      error_authorization_details = err.message;
    }
  }
</script>

{#if error_students}
  <p class="error">{error_students}</p>
{:else}
  <br/>
  <h3>Estudiantes</h3>
  <ul>
    {#each students as s}
      <li>{s.name} ({s.id})</li>
    {/each}
  </ul>
{/if}

{#if error_guardians}
  <p class="error">{error_guardians}</p>
{:else}
  <br/>
  <h3>Representantes Legales</h3>
  <ul>
    {#each guardians as g}
      <li>{g.name} ({g.id})</li>
    {/each}
  </ul>
{/if}

{#if error_authorizations}
  <p class="error">{error_authorizations}</p>
{:else}
  <br/>
  <h3>Autorizaciones</h3>
  <ul>
    {#each authorizations as a}
      <li>
        {a.title} (
        <span
          class="authorization-id"
          role="button"
          tabindex="0"
          on:click={() => loadAuthorizationDetails(a.id)}
          on:keydown={(e) => (e.key === 'Enter' || e.key === ' ') && loadAuthorizationDetails(a.id)}
          >{a.id}</span
        >)
      </li>
    {/each}
  </ul>
  {#if error_authorization_details}
    <p class="error">{error_authorization_details}</p>
  {:else if selectedAuthorization}
    <div class="authorization-details">
      <h4>Detalles de la autorización</h4>
      <table>
        <tbody>
          {#each Object.entries(selectedAuthorization) as [key, value]}
            <tr>
              <th scope="row">{key}</th>
              <td>{value}</td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  {/if}
{/if}

<style>
  .authorization-id {
    cursor: pointer;
    border-radius: 4px;
    padding: 0 2px;
  }

  .authorization-id:hover {
    background-color: lightgray;
  }

  .authorization-details table {
    border-collapse: collapse;
  }

  .authorization-details th,
  .authorization-details td {
    border: 1px solid #ccc;
    padding: 4px 8px;
    text-align: left;
  }

  .authorization-details th {
    background-color: #f9f9f9;
    font-weight: normal;
  }
</style>
