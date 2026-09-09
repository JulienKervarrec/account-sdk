# Parcours francais : account-sdk (SDK Base Account)

Lecture commentee du depot base/account-sdk : le SDK officiel de Base Account, qui expose a la fois des fonctions de paiement autonomes (Base Pay, Base Subscriptions) et un provider complet conforme EIP-1193 pour connecter un wallet Base Account.

Sommaire :

Chapitre 1 Presentation de account-sdk et de ses deux visages. Chapitre 2 createBaseAccountSDK et le provider EIP-1193. Chapitre 3 Communicator, le protocole postMessage avec la popup. Chapitre 4 Le handshake et l echange de cles ECDH. Chapitre 5 Chiffrement AES-GCM des requetes RPC. Chapitre 6 Signer.request, la table de routage des methodes. Chapitre 7 Les sous-comptes et leur financement. Chapitre 8 Base Pay, un paiement USDC en trois lignes. Chapitre 9 Le provider ephemere pour les flux a usage unique. Chapitre 10 Base Subscriptions et les spend permissions EIP-712. Chapitre 11 Charger une subscription (prepareCharge et charge). Chapitre 12 Detecter un wallet deja injecte. Chapitre 13 Limites et perimetre de ce parcours.

Ce parcours est une lecture pedagogique du code source et de la documentation du depot, sans installation ni execution du projet.
