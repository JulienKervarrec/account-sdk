# Chapitre 1 -- Presentation de account-sdk et de ses deux visages

Rien n a ete installe, compile ni execute pour ecrire ce parcours : il s agit d une lecture commentee du code source du depot base/account-sdk, le SDK officiel de Base Account.

Le depot expose en realite deux outils distincts sous un seul package npm, @base-org/account. Le premier est un ensemble de fonctions autonomes, Base Pay et Base Subscriptions, utilisables sans jamais instancier de SDK ni connecter de wallet : pay() envoie un paiement USDC en une poignee de lignes, subscribe() cree un abonnement recurrent. Le second est le SDK complet, active par createBaseAccountSDK, qui fournit un provider conforme EIP-1193 pour dialoguer avec un wallet Base Account -- connexion de compte, signatures, envoi de transactions, sous-comptes.

Ces deux visages partagent la meme infrastructure basse couche : un Communicator qui ouvre une popup vers le wallet et lui parle par postMessage, un Signer qui chiffre chaque requete avec une cle partagee ECDH, et un store partage (ou isole) selon le cas d usage. Les chapitres suivants remontent cette infrastructure de bas en haut, puis redescendent vers les deux API de haut niveau, paiements d abord, SDK complet ensuite.

Fichiers centraux : `packages/account-sdk/src/index.ts`, `README.md`.

[Chapitre suivant : createBaseAccountSDK et le provider EIP-1193](02-createbaseaccountsdk-provider.md)
