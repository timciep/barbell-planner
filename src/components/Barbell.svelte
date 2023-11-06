<script lang="ts">
    const BARBELL_WEIGHTS = [45, 33];

    let selectedBarbellWeight: number|null = null;
    let customBarbellWeight: number|null = null;

    let plateOptions = new Map([
        [55, false],
        [45, true],
        [35, true],
        [25, true],
        [10, true],
        [5, true],
        [2.5, true],
    ]);

    let targetWeight: number|null = null;
    
    let plates: number[] = [];

    let actualWeight: number = 0;

    let roundUp: boolean = false;

    $: if (selectedBarbellWeight !== null && targetWeight !== null) {
        plates = [];
        actualWeight = selectedBarbellWeight;
        
        let barbellWeight = selectedBarbellWeight;

        if (customBarbellWeight !== null) {
            barbellWeight = customBarbellWeight;
        }

        let weight = targetWeight - barbellWeight;
        let smallestPlate = 0;

        plateOptions.forEach((plateEnabled, plate) => {
            if (!plateEnabled) {
                return;
            }

            if (smallestPlate === 0 || plate < smallestPlate) {
                smallestPlate = plate;
            }

            while (weight >= plate * 2) {
                plates.push(plate);
                weight -= plate * 2;
                actualWeight += plate * 2;
            }
        });

        if (roundUp && weight > 0) {
            plates.push(smallestPlate);
            weight -= smallestPlate * 2;
            actualWeight += smallestPlate * 2;
        }
    }
</script>

<div class="main">
    <div class="section">
        <h3>Select available plates</h3>

        <div class="loaded-barbell">
            {#each plateOptions as plateOption}
                <button
                    class="plate plate-select weight-{plateOption[0]}"
                    class:active={plateOption[1]}
                    on:click={() => {
                        console.log(plateOptions);
                        if (plateOption[1]) {
                            plateOptions.set(plateOption[0], false);
                        } else {
                            plateOptions.set(plateOption[0], true);
                        }
                        console.log(plateOptions);
                        plateOptions = plateOptions;
                    }}
                >
                    {plateOption[0]}
                    <br>
                    lbs
                </button>
            {/each}
        </div>
    </div>

    <div class="section">
        <h3>Select barbell weight</h3>

        <div class="barbell">
            <div class="barbell-weights">
                {#each BARBELL_WEIGHTS as weight}
                    <button
                        class:active={selectedBarbellWeight === weight}
                        on:click={() => {
                            selectedBarbellWeight = weight;
                            customBarbellWeight = null;
                        }}
                    >
                        {weight}
                        <br>
                        lbs
                    </button>
                {/each}

                <button class="custom" class:active={customBarbellWeight !== null}>
                    <div>
                        <div>
                            <input type="number" min="0" step="1"
                                bind:value={customBarbellWeight}
                                on:keyup={(e) => selectedBarbellWeight = customBarbellWeight}
                            />
                            lbs
                        </div>
                    </div>
                </button>
            </div>
        </div>
    </div>

    <div class="section">
        <h3>Enter target weight</h3>

        <div>
            <input type="number" min="0" step="1"
                bind:value={targetWeight}
            />
            &nbsp;
            lbs
        </div>
    </div>

    <div class="section results">
        <h3>Results</h3>

        {#if selectedBarbellWeight !== null && targetWeight !== null}
            {#if plates.length === 0}
                <div class="warning">
                    The weight you entered is too light for the barbell you selected.
                </div>
            {:else}
                <div class="total-section">
                    <div class="total">
                        Actual weight:
                        <br>
                        <span class="actual-weight">{actualWeight}</span> lbs
                    </div>

                    <div class="round-up">
                        <input id="round-up" type="checkbox" bind:checked={roundUp} />
                        <label for="round-up">Round up</label>
                    </div> 
                </div>

                <div class="loaded-barbell">
                    {#each plates as plate}
                        <div class="plate weight-{plate}">
                            {plate}
                            <br>
                            lbs
                        </div>
                    {/each}
                </div>

                <div class="help">
                    Load each side of barbell with above plates.
                </div>
            {/if}
        {:else}
            {#if selectedBarbellWeight === null}
                <div class="warning">
                    Please select a barbell weight.
                </div>
            {:else}
                <div class="warning">
                    Please enter a target weight.
                </div>
            {/if}
        {/if}
    </div>
</div>

<style>
    .main {
        text-align: center;
        max-width: 800px;
        margin: 0 auto;
    }

    h3 {
        margin: 0 0 0.5rem 0;
    }

    .warning {
        border: 1px solid rgba(255, 204, 0, 0.5);
        border-radius: 0.25rem;
        background: #fff1bb;
        padding: 0.5rem 1rem;
        width: fit-content;
        margin: 0 auto;
    }

    .section {
        padding: .75rem .5rem;
    }

    .barbell {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;
    }

    .barbell-weights {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        width: 100%;
    }

    .barbell-weights button {
        width: 100%;
        padding: 0.5rem 1.2rem;
        margin: 0.5rem;
        border: 1px solid rgba(0, 0, 0, 0.1);
        border-radius: 0.25rem;
        background-color: transparent;
        touch-action: manipulation;
        font-size: 1rem;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        height: 80px;
        cursor: pointer;
    }

    .custom {
        cursor: default !important;
    }

    input[type=number] {
        width: 4rem;
        margin: 5px 0;
        text-align: center;
        font-size: 16px;
        border-radius: 0.25rem;
        border: 1px solid rgba(0, 0, 0, 0.1);
        padding: 0.5rem 0.1rem 0.5rem 1rem;
    }

    input[type=checkbox] {
        margin: 0 0.1rem 0 0;
        width: 1rem;
        height: 1rem;
        border-radius: 0.25rem;
        border: 1px solid rgba(0, 0, 0, 0.1);
        padding: 0;
        cursor: pointer;
    }

    .barbell-weights button.active, .custom.active {
        background-color: var(--color-theme-2);
        color: white;
    }

    .loaded-barbell {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        width: 100%;
    }

    .plate {
        height: 100px;
        border: 1px solid rgba(0, 0, 0, 0.8);
        border-radius: 0.25rem;
        padding: 0 3px;
        background-color: transparent;
        touch-action: manipulation;
        font-size: 1rem;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        margin: 0.1rem;
        color: white;
        -webkit-text-stroke: 2px black;
        paint-order: stroke fill;
    }

    .plate-select {
        cursor: pointer;
        opacity: .25;
    }

    .plate-select.active {
        opacity: 1;
    }

    .plate.weight-55 {
        flex: 4 0 0;
        background-color: #583101;
    }

    .plate.weight-45 {
        flex: 3.5 0 0;
        background-color: #603808;
    }

    .plate.weight-35 {
        flex: 3 0 0;
        background-color: #6F4518;
    }

    .plate.weight-25 {
        flex: 2.5 0 0;
        background-color: #8B5E34;
    }

    .plate.weight-10 {
        flex: 1.5 0 0;
        max-width: 20%;
        background-color: #A47148;
    }

    .plate.weight-5 {
        flex: 1 0 0;
        max-width: 20%;
        background-color: #BC8A5F;
    }

    .plate.weight-2\.5 {
        flex: 0.5 0 0;
        max-width: 15%;
        background-color: #D4A276;
    }

    .total-section {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        gap: 2rem;

        /* background: rgba(0, 185, 3, 0.4);
        border: 1px solid rgba(0, 185, 3, 0.8);
        border-radius: 0.25rem; */

        padding: 0.5rem 1rem;
        margin-bottom: 1rem;
    }

    .actual-weight {
        font-size: 2rem;
        font-weight: 600;
        color: var(--color-theme-green);
    }

    .section.results {
        background-color: #d6e8d6;
    }

    .help {
        font-size: 0.8rem;
        color: rgba(0, 0, 0, 0.5);
        margin: 0.5rem 0;
    }
</style>