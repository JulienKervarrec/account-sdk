# Chapitre 11 -- Charger une subscription : prepareCharge et charge

Une spend permission signee n est qu une autorisation ; elle ne devient un paiement effectif que lorsque le spender l exerce on-chain. prepareCharge({id, amount}) commence par retrouver la permission complete a partir de son hash via fetchPermission(), puis valide qu il s agit bien d une permission USDC sur le bon reseau (validateUSDCBasePermission) et, si des adresses attendues sont fournies, que le spender et le payeur correspondent bien (assertPermissionAuthorization) -- une protection cote application contre un identifiant de subscription manipule par un client non fiable.

Le montant a charger peut etre un montant precis, converti en unites USDC via parseUnits, ou la chaine speciale max-remaining-charge, qui delegue a prepareSpendCallData() le soin de calculer l allocation restante directement depuis l etat on-chain de la permission. Le resultat est un tableau d appels prets a etre envoyes : une approbation de la permission aupres du Spend Permission Manager si elle n est pas encore active, suivie d un appel de depense proprement dit -- et, si un recipient est fourni, d un transfert additionnel vers cette adresse.

charge() est la version cote Node de ce flux, reservee aux environnements serveur : elle recupere un smart wallet CDP existant (Coinbase Developer Platform) qui joue le role de spender, construit les memes calls via prepareCharge(), puis les soumet comme une UserOperation ERC-4337 via sendUserOpAndWait(), avec un paymaster optionnel pour sponsoriser le gas. La encore, l option expectedSpender impose que le spender qui execute la charge corresponde bien au smart wallet utilise, evitant qu une charge soit executee sous une autre identite que celle prevue au moment de l abonnement.

Fichiers centraux : `packages/account-sdk/src/interface/payment/prepareCharge.ts`, `charge.ts`.

[Chapitre suivant : detecter un wallet deja injecte](12-provider-injecte.md)
