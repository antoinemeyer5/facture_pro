<script lang="ts">
    let imagePreview: string | null = $state(null);

    function handleFileChange(event: Event) {
        const fileInput = event.target as HTMLInputElement;
        const file = fileInput.files?.[0];

        if (file && file.type.startsWith("image/")) {
            const reader = new FileReader();

            reader.onload = (e) => {
                imagePreview = e.target?.result as string;
            };

            reader.readAsDataURL(file);
        } else {
            imagePreview = null;
        }
    }

    /* ###################################################################### */

    // entrepreneur
    let E = $state({ nom: null, adresse: null, siret: null, email: null });

    // client
    let C = $state({ nom: null, adresse: null, texte: null, email: null });

    /* ###################################################################### */

    // document
    let D = $state({
        type: "Facture",
        numero: null,
        validite: null,
        reference: "",
    });

    /* ###################################################################### */

    const today = new Date();
    let day: number = $state(today.getDate());
    let month: number = $state(today.getMonth() + 1);
    let year: number = $state(today.getFullYear());

    /* ###################################################################### */

    // table
    let T = $state({
        c1: "Désignation",
        c2: "Quantité",
        c3: "Unité",
        c4: "Prix unitaire",
    });

    // tâches
    interface Task {
        c1: string | null;
        c2: number | null;
        c3: string | null;
        c4: number | null;
    }

    let tasks: Task[] = $state([
        { c1: "Exemple, exemple", c2: 10, c3: "heures", c4: 2 },
    ]);

    let newTask: Task = $state({ c1: null, c2: null, c3: null, c4: null });

    function addTask() {
        if (newTask.c1 != null) {
            // newTask.c1 = (newTask.c1 as string).split(",");
            tasks = [...tasks, newTask];
            newTask = { c1: null, c2: null, c3: null, c4: null };
        }
    }

    function deleteTask(i: number) {
        tasks = tasks.filter((_, j) => i !== j);
    }

    /* ###################################################################### */

    let totalTask = $derived(
        (Math.round(
            tasks.reduce((t, i) => t + (i.c2 as number) * (i.c4 as number), 0),
        ) *
            100) /
            100,
    );
</script>

<div class="page">
    {#if !imagePreview}
        <input
            class="no_print"
            type="file"
            accept="image/*"
            onchange={handleFileChange}
        />
    {:else}
        <img src={imagePreview} alt="Preview" class="preview" />
    {/if}

    <br /> <br /> <br />
    <!--#####################################################################-->

    <div class="e-c-edit">
        <p class="no_print bold">Entrepreneur</p>
        <p class="no_print bold">Client</p>
        <input
            class="no_print"
            type="text"
            placeholder="nom entreprise"
            bind:value={E.nom}
        />
        <input
            class="no_print"
            type="text"
            placeholder="nom client"
            bind:value={C.nom}
        />
        <input
            class="no_print"
            type="text"
            placeholder="adresse"
            bind:value={E.adresse}
        />
        <input
            class="no_print"
            type="text"
            placeholder="adresse"
            bind:value={C.adresse}
        />
        <input
            class="no_print"
            type="number"
            placeholder="siret"
            bind:value={E.siret}
        />
        <input
            class="no_print"
            type="text"
            placeholder="texte"
            bind:value={C.texte}
        />
        <input
            class="no_print"
            type="email"
            placeholder="email"
            bind:value={E.email}
        />
        <input
            class="no_print"
            type="email"
            placeholder="email"
            bind:value={C.email}
        />
    </div>

    <div class="flex justify-content-space-between">
        <div class="only_print">
            <p>{E.nom != null ? E.nom : "?"}</p>
            <p>{E.adresse != null ? E.adresse : "?"}</p>
            <p>N°SIRET : {E.siret != null ? E.siret : "?"}</p>
            <p>{E.email != null ? E.email : "?"}</p>
        </div>
        <div class="only_print c-display">
            <p>{C.nom != null ? C.nom : "?"}</p>
            <p>{C.adresse != null ? C.adresse : "?"}</p>
            <p>{C.texte != null ? C.texte : "?"}</p>
            <p>{C.email != null ? C.email : "?"}</p>
        </div>
    </div>

    <br /> <br /> <br />
    <!--#####################################################################-->

    <div class="flex">
        <select class="no_print bold" bind:value={D.type}>
            <option value="Facture">Facture</option>
            <option value="Devis">Devis</option>
        </select>
        <input
            class="no_print"
            type="number"
            placeholder="numéro"
            bind:value={D.numero}
        />
        <p class="only_print">
            <span class="bold">{D.type} n°</span>
            {D.numero != null ? D.numero : "?"}
        </p>
    </div>

    <br /> <br /> <br />
    <!--#####################################################################-->

    <div>
        <p>
            <span class="bold">Date :</span>
            {day}/{month > 9 ? month : "0" + month}/{year}
        </p>
        {#if D.type == "Devis"}
            <p>
                <span class="bold">Validité :</span>
                <input
                    class="no_print"
                    type="number"
                    placeholder="validité (mois)"
                    bind:value={D.validite}
                />
                <span class="only_print"
                    >{D.validite != null ? D.validite : "?"} mois</span
                >
            </p>
        {/if}
        <p>
            <span class="bold">Référence :</span>
            <input
                class="no_print"
                type="text"
                placeholder="référence"
                bind:value={D.reference}
            />
            <span class="only_print"
                >{D.reference != "" ? D.reference : "?"}</span
            >
        </p>
    </div>

    <br /> <br /> <br />
    <!--#####################################################################-->

    <table>
        <thead>
            <tr class="no_print">
                <th>
                    <input
                        class="bold"
                        type="text"
                        placeholder="Colonne 1"
                        bind:value={T.c1}
                    />
                </th>
                <th>
                    <input
                        class="bold"
                        type="text"
                        placeholder="Colonne 2"
                        bind:value={T.c2}
                    />
                </th>
                <th>
                    <input
                        class="bold"
                        type="text"
                        placeholder="Colonne 3"
                        bind:value={T.c3}
                    />
                </th>
                <th>
                    <input
                        class="bold"
                        type="text"
                        placeholder="Colonne 4"
                        bind:value={T.c4}
                    />
                </th>
                <th>Montant</th>
            </tr>
            <tr class="only_print">
                <th>{T.c1}</th>
                <th>{T.c2}</th>
                <th>{T.c3}</th>
                <th>{T.c4}</th>
                <th>Montant</th>
            </tr>
        </thead>
        <tbody>
            {#each tasks as task, i}
                <tr>
                    <td>{task.c1}</td>
                    <td>{task.c2}</td>
                    <td>{task.c3}</td>
                    <td>{task.c4}</td>
                    <td>{(task.c2 as number) * (task.c4 as number)}</td>
                    <td class="no_print"
                        ><button onclick={() => deleteTask(i)}>Supprimez</button
                        ></td
                    >
                </tr>
            {/each}
            <tr class="no_print">
                <td>
                    <input
                        type="text"
                        placeholder="Colonne 1"
                        bind:value={newTask.c1}
                    />
                </td>
                <td>
                    <input
                        type="number"
                        placeholder="Colonne 2"
                        bind:value={newTask.c2}
                    />
                </td>
                <td>
                    <input
                        type="text"
                        placeholder="Colonne 3"
                        bind:value={newTask.c3}
                    />
                </td>
                <td>
                    <input
                        type="number"
                        placeholder="Colonne 4"
                        bind:value={newTask.c4}
                    />
                </td>
                <td>
                    <button onclick={addTask}>Ajoutez</button>
                </td>
            </tr>
        </tbody>
    </table>

    <br /> <br /> <br />
    <!--#####################################################################-->

    <div class="right">
        <p class="bold">TOTAL : {totalTask} €</p>
    </div>

    <br /> <br /> <br />
    <!--#####################################################################-->

    <div>
        <p class="bold">Note</p>
        <p>
            TVA non applicable, article 293-B du CGI pour code général des
            impôts.
        </p>
    </div>

    <br /> <br /> <br />
    <!--#####################################################################-->

    {#if D.type == "Devis"}
        <div class="right">
            <p class="bold">
                Signature suivie de la mention "Bon pour accord."
            </p>
            <br />
            <br />
            <p>___________________________________________</p>
        </div>
    {/if}

    <br /> <br /> <br />
    <!--#####################################################################-->
</div>

<style>
    /*.page {
        margin-top: 2vh;
        margin-left: auto;
        margin-right: auto;

        background-color: rgb(234, 234, 234);
        border-radius: 10px;
        padding: 15px;
    }*/
    .preview {
        width: 200px;
        height: auto;
    }

    /* ###################################################################### */

    .e-c-edit {
        display: grid;
        grid-template-columns: auto auto;
        justify-content: space-between;
        gap: 2px;
    }
    .c-display {
        width: 40%;
        border: 2px solid black;
        border-radius: 10px;
        padding: 5px;
    }

    /* ###################################################################### */

    table {
        width: 100%;
        border-collapse: collapse;
        text-align: right;
    }
    td,
    th {
        border: 2px solid black;
        padding: 5px;
    }
    table td:first-child,
    th:first-child {
        text-align: left;
        width: 50%;
    }

    /* ###################################################################### */

    .right {
        text-align: right;
    }
</style>
