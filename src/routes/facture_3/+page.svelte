<script lang="ts">
    let A = $state();
    let B = $state();
    let C = $state();
    let D = $state();

    const today = new Date();
    let E = $state(today.getDate());
    let F: number = $state(today.getMonth() + 1);
    let G = $state(today.getFullYear());

    let H = $state();
    let I = $state();
    let J = $state();
    let K = $state();
    let L = $state();

    let X = $state();

    let tasks: any[] = $state([]);
    let new_task: any = $state({ de: null, du: null, c: null });
    function add_task() {
        if (new_task.de != null && new_task.du != null && new_task.c != null) {
            new_task.de = new_task.de.split(",");
            tasks = [...tasks, new_task];
            new_task = { de: null, du: null, c: null };
        }
    }
    function delete_task(i: number) {
        tasks = tasks.filter((_, j) => i !== j);
    }
    let total = $derived(
        (Math.round(tasks.reduce((t, i) => t + i.du * i.c, 0)) * 100) / 100,
    );
</script>

<div class="no_print visu_edit  ">
    <h3>Facture</h3>
    <div class="flex gap-5 mt-30">
        <p class="mt-mb bold">Numéro de facture :</p>
        <p class="mt-mb">N°</p>
        <input type="number" placeholder="numéro" bind:value={A} />
    </div>
    <div class="flex gap-5 mt-30">
        <p class="mt-mb bold">Date d'émission :</p>
        <input class="date" type="number" placeholder="jj" bind:value={E} />
        <p class="mt-mb">/</p>
        <input class="date" type="number" placeholder="mm" bind:value={F} />
        <p class="mt-mb">/</p>
        <input class="date" type="number" placeholder="aaaa" bind:value={G} />
    </div>
    <div class="flex gap-5 mt-30">
        <p class="mt-mb bold">Référence du contrat :</p>
        <input type="text" placeholder="description" bind:value={X} />
    </div>
    <div class="g-2 mt-30">
        <p class="bold">Entrepreneur</p>
        <p class="bold">Client</p>
        <input type="text" placeholder="nom entreprise" bind:value={B} />
        <input type="text" placeholder="nom client" bind:value={C} />
        <input type="text" placeholder="adresse" bind:value={H} />
        <input type="text" placeholder="adresse" bind:value={I} />
        <input type="number" placeholder="siret" bind:value={J} />
        <input type="text" placeholder="texte" bind:value={K} />
        <input type="email" placeholder="email" bind:value={D} />
        <input type="email" placeholder="email" bind:value={L} />
    </div>
    <div class="mt-30">
        <table>
            <thead>
                <tr>
                    <th
                        >Description <div class="tooltip">
                            &#9432;
                            <span class="tooltiptext"
                                >Ajoutez une virgule pour sauter une ligne. La
                                1ère ligne est en gras.</span
                            >
                        </div></th
                    >
                    <th>Surface (m²)</th>
                    <th>Coût surface (€/m²)</th>
                    <th>Total (€)</th>
                </tr>
            </thead>
            <tbody>
                {#each tasks as task, i}
                    <tr>
                        <td class="left"
                            >{#each task.de as d, j}
                                {#if j == 0}
                                    <b>{d}</b>
                                {:else}
                                    <p>{d}</p>
                                {/if}
                            {/each}</td
                        >
                        <td>{task.du}m²</td>
                        <td>{task.c}€/m²</td>
                        <td>{task.du * task.c}€</td>
                        <td>
                            <button
                                class="button"
                                onclick={() => delete_task(i)}>X <div class="tooltip">
                            &#9432;
                            <span class="tooltiptext">Supprimez cette tâche.</span>
                        </div></button
                            >
                        </td>
                    </tr>
                {/each}
                <tr>
                    <td
                        ><input
                            type="text"
                            placeholder="titre, ligne 1, ligne 2"
                            bind:value={new_task.de}
                        /></td
                    >
                    <td
                        ><input
                            type="number"
                            placeholder="surface"
                            bind:value={new_task.du}
                        /></td
                    >
                    <td
                        ><input
                            type="number"
                            placeholder="coût"
                            bind:value={new_task.c}
                        /></td
                    >
                    <td
                        ><button class="button" onclick={add_task}
                            >Ajouter <div class="tooltip">
                                &#9432;
                                <span class="tooltiptext"
                                    >Complétez entièrement les champs avant
                                    d'ajouter des tâches.</span
                                >
                            </div></button
                        ></td
                    >
                </tr>
            </tbody>
        </table>
    </div>
    <div class="mt-30">
        <table>
            <thead>
                <tr>
                    <th>Total (€)</th>
                    <td>{total}€</td>
                </tr>
            </thead>
        </table>
    </div>
    <div class="mt-30">
        <p class="bold">Note</p>
        <p>
            TVA non applicable, article 293-B du CGI pour code général des
            impôts.
        </p>
    </div>
    <div class="mt-30 ta-right">
        <p>Merci pour votre confiance.</p>
    </div>
</div>

<div class="only_print visu_print ">
    <h3>Facture</h3>
    <div class="mt-30">
        <p><b>Numéro de facture :</b> N°{A != null ? A : "?"}</p>
        <p><b>Date d'émission :</b> {E}/{F > 9 ? F : "0" + F}/{G}</p>
        <p><b>Référence du contrat :</b> {X != null ? X : "?"}</p>
    </div>
    <div class="mt-30 g-2">
        <p class="bold">Entrepreneur</p>
        <p class="bold">Client</p>
        <p>{B != null ? B : "?"}</p>
        <p>{C != null ? C : "?"}</p>
        <p>{H != null ? H : "?"}</p>
        <p>{I != null ? I : "?"}</p>
        <p>N°SIRET : {J != null ? J : "?"}</p>
        <p>{K}</p>
        <p>{D}</p>
        <p>{L}</p>
    </div>

    <div class="mt-30">
        <table>
            <thead>
                <tr>
                    <th>Description</th>
                    <th>Surface (m²)</th>
                    <th>Coût surface (€/m²)</th>
                    <th>Total (€)</th>
                </tr>
            </thead>
            <tbody>
                {#if tasks.length == 0}
                    <tr>
                        <td>?</td>
                        <td>?</td>
                        <td>?</td>
                        <td>?</td>
                    </tr>
                {/if}
                {#each tasks as task, i}
                    <tr>
                        <td class="left"
                            >{#each task.de as d, j}
                                {#if j == 0}
                                    <b>{d}</b>
                                {:else}
                                    <p>{d}</p>
                                {/if}
                            {/each}</td
                        >
                        <td>{task.du}m²</td>
                        <td>{task.c}€/m²</td>
                        <td>{task.du * task.c}€</td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>

    <div class="mt-5">
        <table>
            <thead>
                <tr>
                    <th>Total (€)</th>
                    <td>{total}€</td>
                </tr>
            </thead>
        </table>
    </div>

    <div class="mt-30">
        <p class="bold">Note</p>
        <p>
            TVA non applicable, article 293-B du CGI pour code général des
            impôts.
        </p>
    </div>
    <div class="mt-30 ta-right">
        <p>Merci pour votre confiance.</p>
    </div>
</div>

<style>
    .tooltip {
        position: relative;
        display: inline-block;
        cursor: help;
    }

    .tooltip .tooltiptext {
        visibility: hidden;
        width: 120px;
        background-color: black;
        color: #fff;
        text-align: center;
        border-radius: 6px;
        padding: 5px 0;

        /* Position the tooltip */
        position: absolute;
        z-index: 1;
    }

    .tooltip:hover .tooltiptext {
        visibility: visible;
    }

    .g-2 {
        display: grid;
        grid-template-columns: auto auto;
        justify-content: space-between;
        gap: 2px;
    }

    .date {
        width: 50px;
    }

    .visu_edit {
        margin-top: 2vh;
        margin-bottom: 2vh;

        /* height: 96vh; */
        border: 2px solid black;
        border-radius: 10px;
        padding: 10px;
    }
    .visu_print {
        margin-top: 2vh;
        margin-left: auto;
        margin-right: auto;
    }
    input {
        text-align: center;

        border: 2px solid black;
        border-radius: 10px;
        padding: 5px;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        text-align: center;
    }

    td,
    th {
        border: 2px solid black;
        padding: 5px;
    }

    .left {
        text-align: left;
    }
</style>
