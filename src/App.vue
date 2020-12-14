<template>
  <div>
    <header>
        <img alt="Github logo" src="./assets/github.svg">
        <h1>Github Status</h1>
        <p>Last Updated <span>{{`${updated !== 'unknown' ? new Date(updated).toUTCString() : 'N/A'}`}}</span></p>
    </header>
    <main>
      <ul role="list">
        <li v-for="component in components" :key="component.id" :class="component.status">
          <div>
            <h2>{{component.name}}</h2>
            <span>{{component.status}}</span>
          </div>
          <p>{{component.description}}</p>
        </li>
      </ul>
    </main>
    <footer>
      <p :class="status.class">{{status.name}}</p>
    </footer>
  </div>

</template>

<script>

export default {
  name: 'App',
  data: function() {
    return {
      components: [],
      status: {
        name: 'unknown'
      },
      updated: 'unknown'
    };
  },
  mounted() {
    const STATUS_API = 'https://kctbh9vrtdwd.statuspage.io/api/v2/summary.json';
    fetch(STATUS_API).then(res => res.json()).then(response => {
        this.components = response.components.filter((_, i) => i !== 3);
        this.status = {
          name: response.status.description,
          class: response.status.description.split(' ').join('_').toLowerCase()
        };
        this.updated = response.page['updated_at'];
    });
  }
}
</script>

<style lang="scss">

body {
  font-family: 'Inter', sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 2rem;
  background-color: #F5F7F8;
  font-size: 16px;
  max-width: 1300px;

  @media(max-width: 700px) {
    padding: 0.5rem;
    justify-content: flex-start;
  }

}

header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;

  img {
    width: 40px;
  }

  span {
    display: block;
  }
}

h2 {
  font-size: 1.1rem;
}

span {
  font-weight: 400;
  color: grey;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-size: 0.8em;
}

[role="list"] {
  margin-top: 2rem;
  list-style-type: none;
  padding: 0;

  @media(min-width: 700px) {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 1rem;
  }

  .operational {
    border-left-color: #2DAF52;
  }

  li {
    margin-top: 1rem;
    border-left: 8px solid lightgrey;
    background-color: white;
    padding: 0.75em 1em;
    div {
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
    p {
      max-width: 55ch;
      font-size: 0.9em;
      margin-top: 1rem;
    }
  }
}

footer p {
  text-align: center;
  margin: 1rem;
  background: lightgray;
  padding: 1em;
  border-radius: 5px;
}

.all_systems_operational {
  background: #2DAF52;
  color: white;
}
</style>
