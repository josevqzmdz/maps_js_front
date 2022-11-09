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
      data: { id: `n0n1`, source: "n0", target: "n1", weight: 2 },
    },
    {
      data: { id: `n1n2`, source: "n1", target: "n2", weight: 3 },
    },
    {
      data: { id: `n2n3`, source: "n2", target: "n3", weight: 5 },
    },
    {
      data: { id: `n3n4`, source: "n3", target: "n4", weight: 5 },
    },
    {
      data: { id: `n4n5`, source: "n4", target: "n5", weight: 3 },
    },
    {
      data: { id: `n5n7`, source: "n5", target: "n7", weight: 3 },
    },
    {
      data: { id: `n7n6`, source: "n7", target: "n6", weight: 3 },
    },
    {
      data: { id: `n4n6`, source: "n4", target: "n6", weight: 3 },
    },
    {
      data: { id: `n7n8`, source: "n7", target: "n8", weight: 2 },
    },
    {
      data: { id: `n8n9`, source: "n8", target: "n9", weight: 3 },
    },
    {
      data: { id: `n6n9`, source: "n6", target: "n9", weight: 2 },
    },
    {
      data: { id: `n3n11`, source: "n3", target: "n11", weight: 3 },
    },
    {
      data: { id: `n10n11`, source: "n10", target: "n11", weight: 2 },
    },
    {
      data: { id: `n9n10`, source: "n9", target: "n10", weight: 4 },
    },
    {
      data: { id: `n10n12`, source: "n10", target: "n12", weight: 2 },
    },
    {
      data: { id: `n11n12`, source: "n11", target: "n12", weight: 2 },
    },
    {
      data: { id: `n12n13`, source: "n12", target: "n13", weight: 3 },
    },
    {
      data: { id: `n0n5`, source: "n0", target: "n5", weight: 3 },
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
          id: "lazaro cardenas",
        },
        position: {
          x: 242 + 30,
          y: 480 + 30,
        },
      },
      {
        group: "nodes",
        data: {
          id: "nueva italia",
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
          id: "los reyes",
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
          id: "ciudad hidalgo",
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
      type="submit">Buscar</button
    >
  </form>
</main>

<style>
  .container {
    background-image: url("./assets/mich.png");
    background-repeat: no-repeat;
  }
</style>
