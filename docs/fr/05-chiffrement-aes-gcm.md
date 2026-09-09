# Chapitre 5 -- Chiffrement AES-GCM des requetes RPC

Une fois le secret partage derive, chaque requete RPC (hors handshake) est chiffree avant de quitter l application. encryptContent() serialise le contenu en JSON -- avec un remplacement special pour les objets Error, reduits a {code, message} pour eviter de tenter de serialiser une stack trace -- puis appelle encrypt(), qui genere un vecteur d initialisation aleatoire de 12 octets et chiffre le texte avec AES-GCM en utilisant le secret partage comme cle. Le resultat, {iv, cipherText}, est ce qui transite reellement dans le message postMessage envoye a la popup.

Cote reception, decryptResponseMessage() effectue l operation inverse : si le contenu du message contient un champ failure, c est une erreur protocolaire non chiffree qui est directement relancee ; sinon, decryptContent() dechiffre avec le meme secret partage et reparse le JSON obtenu en objet RPCResponse.

Ce meme flux de dechiffrement est aussi le point ou le Signer synchronise des informations globales envoyees par le wallet independamment de la requete d origine : si la reponse contient un champ data.chains, la liste des chaines disponibles (avec leurs RPC URL et devises natives) est enregistree dans le store et de nouveaux clients RPC sont crees pour elles ; si elle contient data.capabilities, les capacites du compte cote wallet sont mises a jour localement. Le chiffrement AES-GCM garantit que ces informations, comme le reste du contenu de la reponse, ne sont lisibles que par les deux parties ayant participe au handshake ECDH.

Fichier central : `packages/account-sdk/src/util/cipher.ts`.

[Chapitre suivant : Signer.request, la table de routage des methodes](06-signer-routage.md)
