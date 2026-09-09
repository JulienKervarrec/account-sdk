# Chapitre 9 -- Le provider ephemere pour les flux a usage unique

EphemeralBaseAccountProvider est une version volontairement restreinte de BaseAccountProvider, concue pour pay() et subscribe(). Sa difference structurelle majeure : plutot que d utiliser le store global persistant (partage entre toutes les instances du SDK sur la page), il cree sa propre instance de store isolee via createStoreInstance({persist: false}) -- aucune ecriture dans localStorage, aucune interference possible avec une autre instance du SDK ou avec un autre paiement concurrent.

Sa methode request() n accepte qu un sous-ensemble reduit de methodes : wallet_sendCalls et wallet_sign (les deux operations necessaires a un paiement ou une signature de permission), wallet_getCallsStatus, et des reponses par defaut pour eth_accounts, net_version et eth_chainId. Toute autre methode leve une erreur explicite listant les methodes reellement supportees plutot que d echouer silencieusement.

L EphemeralSigner qui l accompagne herite du Signer du chapitre 6 mais court-circuite l essentiel de sa table de routage : pour ces flux, chaque requete refait un handshake ECDH complet (chapitre 4), puis nettoie et fait tourner (rotate) les cles ephemeres immediatement apres, dans un bloc finally -- garantissant qu aucune cle de session ne survit au-dela d une seule requete. C est ce choix de conception, isolation du store plus rotation systematique des cles, qui permet a plusieurs paiements Base Pay independants de s executer sans se polluer mutuellement, meme en parallele sur la meme page.

Fichiers centraux : `packages/account-sdk/src/interface/builder/core/EphemeralBaseAccountProvider.ts`, `packages/account-sdk/src/sign/base-account/EphemeralSigner.ts`.

[Chapitre suivant : Base Subscriptions et les spend permissions EIP-712](10-base-subscriptions.md)
