# Anchor Track Submission

- Name / GitHub handle: Pablo
- Program ID (devnet): https://explorer.solana.com/address/FILL_ME?cluster=devnet
- Minted asset: https://explorer.solana.com/address/FILL_ME?cluster=devnet
- Mint transaction: https://explorer.solana.com/tx/FILL_ME?cluster=devnet

How does your program make the NFT soulbound?

> The program CPIs into MPL Core's `CreateV2` and attaches one plugin at
> creation time:
>
> ```rust
> .plugins(vec![PluginAuthorityPair {
>     plugin: Plugin::PermanentFreezeDelegate(PermanentFreezeDelegate { frozen: true }),
>     authority: Some(PluginAuthority::None),
> }])
> ```
>
> Both halves are load-bearing. `frozen: true` makes MPL Core reject transfer
> and burn from the moment the asset exists. `PluginAuthority::None` means no
> account holds the right to update the plugin, so nobody can ever thaw it —
> not the owner, not the update authority, not us.
>
> Frozen with a real authority would only be a temporary lock. And because
> `PermanentFreezeDelegate` can only be added at creation, there is no later
> transaction that could have attached a weaker version of it.
