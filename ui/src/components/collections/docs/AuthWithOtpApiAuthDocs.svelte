<script>
    import CodeBlock from "@/components/base/CodeBlock.svelte";
    import CommonHelper from "@/utils/CommonHelper";

    export let collection;

    let responseTab = 200;
    let responses = [];

    $: responses = [
        {
            code: 200,
            body: JSON.stringify(
                {
                    token: "JWT_TOKEN",
                    record: CommonHelper.dummyCollectionRecord(collection),
                },
                null,
                2,
            ),
        },
        {
            code: 400,
            body: `
                {
                  "code": 400,
                  "message": "Failed to authenticate.",
                  "data": {
                    "otpId": {
                      "code": "validation_required",
                      "message": "Missing required value."
                    }
                  }
                }
            `,
        },
    ];
</script>

<div class="alert alert-success">
    <strong class="label label-primary">POST</strong>
    <div class="content">
        <p>
            /api/collections/<strong>{collection.name}</strong>/auth-with-otp
        </p>
    </div>
</div>

<div class="section-title">Body Parameters</div>
<table class="table-compact table-border m-b-base">
    <thead>
        <tr>
            <th>Param</th>
            <th>Type</th>
            <th width="50%">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <div class="inline-flex">
                    <span class="label label-success">Required</span>
                    <span>otpId</span>
                </div>
            </td>
            <td>
                <span class="label">String</span>
            </td>
            <td>The id of the OTP request.</td>
        </tr>
        <tr>
            <td>
                <div class="inline-flex">
                    <span class="label label-success">Required</span>
                    <span>password</span>
                </div>
            </td>
            <td>
                <span class="label">String</span>
            </td>
            <td>The one-time password.</td>
        </tr>
    </tbody>
</table>

<div class="section-title">Responses</div>
<div class="tabs">
    <div class="tabs-header compact combined left">
        {#each responses as response (response.code)}
            <button
                class="tab-item"
                class:active={responseTab === response.code}
                on:click={() => (responseTab = response.code)}
            >
                {response.code}
            </button>
        {/each}
    </div>
    <div class="tabs-content">
        {#each responses as response (response.code)}
            <div class="tab-item" class:active={responseTab === response.code}>
                <CodeBlock content={response.body} />
            </div>
        {/each}
    </div>
</div>
