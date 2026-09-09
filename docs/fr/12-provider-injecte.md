# Chapitre 12 -- Detecter un wallet deja injecte

Avant meme de construire un BaseAccountProvider et d ouvrir une popup, createBaseAccountSDK() verifie si un wallet est deja present dans la page. getInjectedProvider() lit window.top?.ethereum en priorite (pour fonctionner correctement meme si l application est chargee dans une iframe), avec repli sur window.ethereum, et ne retient ce provider injecte que s il porte un marqueur precis : la propriete isCoinbaseBrowser mise a true.

Ce marqueur cible un cas d usage particulier : une application ouverte a l interieur du navigateur integre de l app Coinbase (ou d un environnement equivalent qui injecte un wallet portant ce flag). Dans ce contexte, il n y a nul besoin d ouvrir une popup separee ni de negocier un handshake ECDH -- le wallet est deja dans le meme processus applicatif, et l injecte directement suffit a satisfaire l interface ProviderInterface attendue par le reste du SDK.

Si aucun wallet ainsi marque n est trouve, ou si la lecture de window.ethereum echoue (la fonction capture toute exception et journalise l erreur via la telemetrie plutot que de la laisser remonter), le SDK revient au chemin par defaut decrit au chapitre 2 : un nouveau BaseAccountProvider est instancie, avec sa propre popup et son propre handshake. Ce repli silencieux garantit que le SDK fonctionne de maniere identique, que l application tourne dans un navigateur ordinaire ou dans un environnement Coinbase specialise -- seul le chemin de communication interne change.

Fichier central : `packages/account-sdk/src/interface/builder/core/getInjectedProvider.ts`.

[Chapitre suivant : limites et perimetre de ce parcours](13-limites-perimetre.md)
