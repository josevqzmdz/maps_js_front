<script>
  import cytoscape from "cytoscape";
  import { onMount } from "svelte";
  let container;
  let cy;

  let origin;
  let destination;
  let isDisabled = false;

  const cities = [
    "aquila",
    "maruata",
    "lazaro cardenas",
    "nueva italia",
    "apatzingan",
    "tepaltepec",
    "los reyes",
    "uruapan",
    "patzcuaro",
    "pahuayo",
    "zamora",
    "zacapu",
    "morelia",
    "hidalgo",
  ];

  const startingEdges = [
    {
      data: { id: `n0n1`, source: "aquila", target: "maruata", weight: 2 },
    },
    {
      data: { id: `n1n2`, source: "aquila", target: "tepalcatepec", weight: 3 },
    },
    {
      data: { id: `n2n3`, source: "maruata", target: "lazaro_cardenas", weight: 5 },
    },
    {
      data: { id: `n3n4`, source: "lazaro_cardenas", target: "nueva_italia", weight: 5 },
    },
    {
      data: { id: `n4n5`, source: "nueva_italia", target: "apatzingan", weight: 3 },
    },
    {
      data: { id: `n5n7`, source: "nueva_italia", target: "patzcuaro", weight: 3 },
    },
    {
      data: { id: `n7n6`, source: "apatzingan", target: "tepalcatepec", weight: 3 },
    },
    {
      data: { id: `n4n6`, source: "apatzingan", target: "uruapan", weight: 3 },
    },
    {
      data: { id: `n7n8`, source: "tepalcatepec", target: "los_reyes", weight: 3 },
    },
    {
      data: { id: `n8n9`, source: "los_reyes", target: "sahuayo", weight: 2 },
    },
    {
      data: { id: `n6n9`, source: "los_reyes", target: "uruapan", weight: 3 },
    },
    {
      data: { id: `n3n11`, source: "uruapan", target: "zamora", weight: 2 },
    },
    {
      data: { id: `n10n11`, source: "sahuayo", target: "zamora", weight: 3 },
    },
    {
      data: { id: `n9n10`, source: "zamora", target: "zacapu", weight: 4 },
    },
    {
      data: { id: `n10n12`, source: "zacapu", target: "patzcuaro", weight: 2 },
    },
    {
      data: { id: `n11n12`, source: "zacapu", target: "morelia", weight: 2 },
    },
    {
      data: { id: `n12n13`, source: "patzcuaro", target: "morelia", weight: 2 },
    },
    {
      data: { id: `n0n5`, source: "morelia", target: "cd_hidalgo", weight: 3 },
    },
  ];

  onMount(() => {
    if (!container) throw Error("Container not found");
    cy = cytoscape({
      autolock: true,
      container: container, // container to render in
      layout: {
        name: "grid",
        rows: 1,
      },
      style: [
        {
          selector: "node",
          style: {
            "background-color": "#666",
            label: "data(id)",
          },
        },

        {
          selector: "edge",
          style: {
            width: 3,
            "line-color": "#ccc",
            "target-arrow-color": "#ccc",
            "target-arrow-shape": "triangle",
            "curve-style": "bezier",
            "source-arrow-shape": "triangle-backcurve",
            label: "data(weight)",
          },
        },
      ],
    });

    cy.panningEnabled(false);
    cy.zoomingEnabled(false);

    if (!cy) throw Error("Cy not initialized");
    cy.add([
      {
        group: "nodes",
        data: {
          id: "aquila",
        },
        position: {
          x: 45 + 30,
          y: 369 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "maruata",
        },
        position: {
          x: 127 + 30,
          y: 450 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "lazaro_cardenas",
        },
        position: {
          x: 242 + 30,
          y: 480 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "nueva_italia",
        },
        position: {
          x: 337 + 30,
          y: 322 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "apatzingan",
        },
        position: {
          x: 245 + 30,
          y: 320 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "tepalcatepec",
        },
        position: {
          x: 149 + 30,
          y: 283 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "uruapan",
        },
        position: {
          x: 315 + 30,
          y: 223 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "los_reyes",
        },
        position: {
          x: 251 + 30,
          y: 215 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "sahuayo",
        },
        position: {
          x: 214 + 30,
          y: 113 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "zamora",
        },
        position: {
          x: 321 + 30,
          y: 120 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "zacapu",
        },
        position: {
          x: 426 + 30,
          y: 191 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "patzcuaro",
        },
        position: {
          x: 456 + 30,
          y: 259 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "morelia",
        },
        position: {
          x: 512 + 30,
          y: 166 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "cd_hidalgo",
        },
        position: {
          x: 617 + 30,
          y: 142 + 30,
        },
      },
    ]);
    cy.add(startingEdges);
  });

  const searchPath = async () => {
    isDisabled = true;
    if (!cy) throw Error("Cy not initialized");

    const res = await fetch(
      `${
        import.meta.env.VITE_API_BASE_URL
      }/api/busqueda?origin=${origin}&destination=${destination}`
    );
    // Expect this type of array
    /*
    [
      { weight: 3, id: "n0n1" },
      { weight: 2, id: "n1n2" },
      { weight: 6, id: "n2n3" },
    ];
    */
    const edges = await res.json();

    let summedEdges = [];

    edges.reduce((prev, curr) => {
      prev = curr.weight + prev;
      summedEdges.push({ ...curr, weight: prev });
      return prev;
    }, 0);

    summedEdges.forEach((edge) => {
      cy.$(`#${edge.id}`).data("weight", edge.weight);
      cy.$(`#${edge.id}`).style("line-color", "#ab4631");
      cy.$(`#${edge.id}`).style("width", 5);
    });

    setTimeout(() => {
      summedEdges.forEach((edge) => {
        cy.$(`#${edge.id}`).data(
          "weight",
          startingEdges.find((edge) => edge.data.id === edge.data.id).data
            .weight
        );
        cy.$(`#${edge.id}`).style("line-color", "#ccc");
        cy.$(`#${edge.id}`).style("width", 3);
      });
      origin = "";
      destination = "";
      isDisabled = false;
    }, 5000);
  };

   // https://stackoverflow.com/questions/69970474/cannot-fetch-data-from-localhost-using-sveltekit
  export async function recibirJson({ fetch }){
    const rest = await fetch("https://localhost:7117/swagger/index.html");
    const ciudades = await res.json();
    if (res.ok){
      return { props: { ciudades: ciudades }};
    }
    return {
      status: res.status,
      error: new Error()
    };
  }

  export let ciudades = [];
</script>

<main class="w-screen h-screen flex flex-col">
  <div class="m-auto w-1/2 container h-full flex" bind:this={container} />
  <form
    class="mx-auto w-full flex space-x-4 p-4"
    on:submit|preventDefault={searchPath}
  >
    <div>
      <label for="origin">Origen</label>
      <select
        name="origin"
        on:change={() => {
          destination = "";
        }}
        bind:value={origin}
      >
        {#each cities as city}
          <option value={city}>{city}</option>
        {/each}
      </select>
    </div>
    <div>
      <label for="destination">Destino</label>
      <select name="destination" bind:value={destination}>
        {#each cities.filter((city) => city !== origin) as city}
          <option value={city}>{city}</option>
        {/each}
      </select>
    </div>
    <button
      class="px-4 py-2 bg-blue-600 font-bold text-white rounded-lg"
      disabled={isDisabled}
      type="submit">Buscar</button>
  </form>

  <div>
    <p>{#each ciudades as ciudad} {ciudad.cities}{/each}</p>
  </div>
</main>

<style>
  .container {
    background-image: url("./assets/mich.png");
    background-repeat: no-repeat;
  }
</style>
