<section>
    <h1>Namebid refunder!</h1>

    <label>The name you bid on</label><br />
    <input type="text" placeholder="Name" bind:value={name} />
    <br />
    <br />
    <button on:click={fix}>Refund</button>
</section>

<script lang="ts">
    import { SessionKit, Chains } from "@wharfkit/session"
    import { WebRenderer } from "@wharfkit/web-renderer"
    import { WalletPluginAnchor } from "@wharfkit/wallet-plugin-anchor"

    const webRenderer = new WebRenderer()

    const sessionKit = new SessionKit({
        appName: "appname",
        chains: [Chains.EOS],
        ui: webRenderer,
        walletPlugins: [new WalletPluginAnchor()],
    })

    let name = ""
    let bidder = ""

    const fix = async () => {
        await sessionKit.logout()
        const { session } = await sessionKit.login()
        const result = session.transact({
            action: {
                account: "eosio",
                name: "bidrefund",
                authorization: [session.permissionLevel],
                data: {
                    bidder:session.actor,
                    newname:name,
                },
            },
        })
        console.log(result)
    }
</script>
