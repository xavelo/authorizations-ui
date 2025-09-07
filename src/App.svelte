<script>
  import { onMount } from 'svelte';

  const API_BASE_URL = import.meta.env.VITE_API_BASE_URL;

  const API_ENDPOINTS = {
    students: `${API_BASE_URL}/students`,
    teachers: `${API_BASE_URL}/teachers`,
    courses: `${API_BASE_URL}/courses`
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
      <li>{a.title} ({a.id})</li>
    {/each}
  </ul>
{/if}