# Chapitre 2 -- createBaseAccountSDK et le provider EIP-1193

createBaseAccountSDK(params) est le point d entree du SDK complet. Il ne retourne pas directement un provider mais un objet avec une methode getProvider() paresseuse : le BaseAccountProvider n est construit qu au premier appel, et reutilise ensuite la meme instance. Avant cela, la fonction fait deux choses une seule fois par page, quel que soit le nombre d instances de SDK creees : initializeGlobalOnce() verifie la politique Cross-Origin-Opener-Policy et rehydrate le store persiste depuis localStorage, tandis que initializeTelemetryOnce() charge le script de telemetrie si l app ne l a pas desactive.

Le choix du provider retourne depend de l environnement : getInjectedProvider() est consulte en premier, et si un wallet Coinbase est deja injecte dans la page (window.ethereum avec le flag isCoinbaseBrowser), c est lui qui est utilise plutot qu une nouvelle instance de BaseAccountProvider -- ce mecanisme est detaille au chapitre 12.

BaseAccountProvider lui-meme est une classe compacte : il compose un Communicator (pour parler a la popup) et un Signer (pour chiffrer et router les requetes), et implemente une seule methode publique, request(args), enveloppee par un decorateur de mesure de performance (withMeasurement). Le SDK expose aussi un espace subAccount avec create, get et addOwner, qui traduisent des appels de haut niveau en requetes wallet_addSubAccount ou wallet_sendCalls sur ce meme provider -- voir chapitre 7.

Fichiers centraux : `packages/account-sdk/src/interface/builder/core/createBaseAccountSDK.ts`, `BaseAccountProvider.ts`.

[Chapitre suivant : Communicator, le protocole postMessage avec la popup](03-communicator-popup.md)
