# Chapitre 3 -- Communicator, le protocole postMessage avec la popup

Le Communicator est la seule classe du SDK qui touche au DOM du navigateur. Son role : ouvrir une fenetre popup vers l origine du wallet (CB_KEYS_URL par defaut, ou une walletUrl personnalisee), lui envoyer des messages, et ecouter ses reponses -- rien de plus.

waitForPopupLoaded() est la methode pivot. Si une popup est deja ouverte et non fermee, elle se contente de lui redonner le focus (utile si l utilisateur avait clique ailleurs) et la retourne immediatement. Sinon, elle ouvre une nouvelle fenetre via openPopup(), puis attend deux evenements en parallele : un message PopupUnload qui declenche un nettoyage (rejet de toutes les requetes en attente, fermeture de la popup) si l utilisateur ferme la fenetre manuellement, et un message PopupLoaded qui signale que la popup est prete a recevoir des donnees. Des reception du PopupLoaded, le Communicator repond avec un message de configuration contenant la version du SDK, son nom de package, les metadonnees de l app et l URL courante.

postRequestAndWaitForResponse() est le patron utilise par tout le reste du SDK pour dialoguer avec la popup : il enregistre un listener filtrant sur requestId avant meme d envoyer le message, pour eviter toute course entre l envoi et la reception. Chaque message recu est valide par son origin (event.origin doit correspondre exactement a l origine du wallet) avant d etre accepte -- une protection essentielle puisque window.postMessage peut en principe recevoir des messages de n importe quelle origine.

Fichier central : `packages/account-sdk/src/core/communicator/Communicator.ts`.

[Chapitre suivant : le handshake et l echange de cles ECDH](04-handshake-ecdh.md)
