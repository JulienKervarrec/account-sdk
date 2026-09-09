# Chapitre 4 -- Le handshake et l echange de cles ECDH

Avant qu une seule requete utile ne soit envoyee au wallet, le Signer effectue un handshake qui etablit un secret partage entre l application et le wallet, sans jamais faire transiter ce secret lui-meme sur le reseau. Le mecanisme est un echange Diffie-Hellman sur courbe elliptique (ECDH, courbe P-256), entierement delegue a l API Web Crypto native du navigateur -- aucune bibliotheque cryptographique tierce n est necessaire.

SCWKeyManager encapsule cette logique. loadKeysIfNeeded() tente d abord de recharger une paire de cles existante depuis le store (persistee en hexadecimal) ; si aucune n existe, generateKeyPair() en cree une nouvelle via crypto.subtle.generateKey avec l algorithme ECDH/P-256. Cote application, cette paire de cles est stable tant que la session n est pas nettoyee.

Le handshake proprement dit, dans Signer.handshake(), envoie a la popup un message contenant sa propre cle publique (exportee en hexadecimal) et attend une reponse. Cette reponse porte la cle publique du wallet dans son champ sender. SCWKeyManager.setPeerPublicKey() importe cette cle et invalide l ancien secret partage ; au prochain appel de getSharedSecret(), deriveSharedSecret() combine la cle privee locale et la cle publique du pair via crypto.subtle.deriveKey pour produire directement une cle AES-GCM 256 bits -- le secret ECDH n est jamais manipule sous forme brute, il sort de l API Crypto deja pret a chiffrer.

Fichiers centraux : `packages/account-sdk/src/sign/base-account/SCWKeyManager.ts`, `packages/account-sdk/src/util/cipher.ts`.

[Chapitre suivant : chiffrement AES-GCM des requetes RPC](05-chiffrement-aes-gcm.md)
