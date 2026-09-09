# Chapitre 10 -- Base Subscriptions et les spend permissions EIP-712

subscribe({recurringCharge, subscriptionOwner, periodInDays}) ne cree pas de transaction on-chain immediate : elle fait signer au payeur un message EIP-712 type qui autorise un spender (l adresse de l application, subscriptionOwner) a depenser jusqu a un certain montant d USDC par periode. Ce message suit le schema SpendPermission d un contrat singleton partage, le Spend Permission Manager, identifie par son propre domaine EIP-712 (name: Spend Permission Manager, version: 1) et son adresse verifyingContract fixe.

Le typed data complet inclut account (le payeur, injecte plus tard par le wallet -- l application envoie une adresse placeholder que le wallet remplace via un mecanisme mutableData cible sur message.account), spender, token, allowance, period, start, end, salt et extraData. subscribe() delegue sa construction a createSpendPermissionTypedData() (ou sa variante avec periodInSeconds, reservee au testnet pour accelerer les tests), puis envoie une requete wallet_sign avec ce typed data et le flag mutableData, via -- une fois de plus -- le provider ephemere du chapitre precedent.

La reponse du wallet contient la signature et les donnees signees completes (avec l adresse reelle du payeur substituee). subscribe() calcule alors le hash de la permission via getHash() (qui reproduit le calcul de hash du contrat on-chain) et le retourne comme identifiant public de l abonnement -- id. Cet identifiant n est pas un secret d autorisation en lui-meme : la documentation du SDK insiste pour que les applications le stockent cote serveur, lie a l utilisateur authentifie, plutot que de faire confiance a un id fourni par un client non fiable.

Fichiers centraux : `packages/account-sdk/src/interface/payment/subscribe.ts`, `interface/public-utilities/spend-permission/utils.ts`.

[Chapitre suivant : charger une subscription (prepareCharge et charge)](11-charger-subscription.md)
