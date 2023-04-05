<script lang="ts">
    import sha256 from 'js-sha256';

    let number = 0;
    let selectedUni;
    let selectedSpecialty;
    let selectedType;
    let selectedHash;
    async function readFile(file) {
        return new Promise((resolve, reject) => {
            const reader = new FileReader();
            reader.onload = () => resolve(reader.result);
            reader.onerror = reject;
            reader.readAsArrayBuffer(file);
        });
    }
    async function handleFileChange(event) {
        const file = event.target.files[0];
        const fileData = await readFile(file);
        selectedHash = sha256.update(fileData).hex();
    }
    let send = () => {
        console.log(selectedUni, selectedSpecialty, selectedType, selectedHash)
    }
    let options = [
        {
            uni: "University of Twente",
            specialties: [
                {
                    study: "Data Science",
                    type: ["Master", "Bachelor"]
                },
                {
                    study: "Biology",
                    type: ["Master", "Bachelor"]
                },
                {
                    study: "Math",
                    type: ["Master", "Bachelor"]
                }

            ],
        },
        {
            uni: "Delft",
            specialties: [
                {
                    study: "Cyber Security",
                    type: ["Master", "Bachelor"]
                },
                {
                    study: "Biology",
                    type: ["Master", "Bachelor"]
                },
                {
                    study: "Math",
                    type: ["Master", "Bachelor"]
                }

            ],
        },
    ]
</script>

<input type="number" bind:value={number}>
<select bind:value={selectedUni} on:change={() => {selectedSpecialty = ""; selectedType = ""}}>
    {#each options as {uni}}
        <option value={uni}>
            {uni}
        </option>
    {/each}
</select>
<select bind:value={selectedSpecialty}  on:change={() => {selectedType = ""}}>
    {#each options.find(({uni}) => uni === selectedUni)?.specialties || [] as {study}}
        <option value={study}>
            {study}
        </option>
    {/each}
</select>
<select bind:value={selectedType}>
    {#each options.find(({uni}) => uni === selectedUni)?.specialties?.find(({study}) => study === selectedSpecialty)?.type || [] as type}
        <option value={type}>
            {type}
        </option>
    {/each}
</select>
<input type="file" on:change={handleFileChange}>
<button on:click={() => send()}>
    Submit
</button>
