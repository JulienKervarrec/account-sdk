# Chapitre 8 -- Base Pay, un paiement USDC en trois lignes

pay({amount, to, testnet}) est concue pour ne demander aucune configuration prealable : ni createBaseAccountSDK, ni connexion explicite. Sous le capot, la fonction valide d abord ses arguments -- validateStringAmount impose au maximum 6 decimales (la precision de l USDC), normalizeAddress verifie le format de l adresse destinataire -- puis translatePaymentToSendCalls() construit une requete wallet_sendCalls conforme ERC-5792.

Cette traduction encode un unique appel : un transfer() ERC-20 vers l adresse de l USDC sur le reseau choisi (base ou baseSepolia selon testnet), avec le montant converti en unites de 6 decimales via parseUnits. Si l appelant a fourni un payerInfo (des champs d information a demander au payeur, comme un email ou une adresse de livraison), une capability dataCallback est ajoutee a la requete ; un dataSuffix optionnel, valide au prealable, est encode dans une capability attribution -- un mecanisme d attribution qui permet a un tiers (une plateforme d affiliation, par exemple) de tracer les paiements generes via son integration.

Cette requete est ensuite executee par executePaymentWithSDK(), qui cree un SDK ephemere pour l occasion (chapitre 9) plutot que de reutiliser une instance persistante. Un detail important pour la fiabilite : les paiements vers une meme destination (meme reseau, meme walletUrl) sont mis en file via une paymentQueue, chaque nouvel appel attendant la fin du precedent avant de demarrer sa propre instance ephemere -- ce qui evite que deux paiements concurrents ne se marchent dessus sur le meme SCWKeyManager.

Fichiers centraux : `packages/account-sdk/src/interface/payment/pay.ts`, `utils/translatePayment.ts`, `utils/sdkManager.ts`.

[Chapitre suivant : le provider ephemere pour les flux a usage unique](09-provider-ephemere.md)
